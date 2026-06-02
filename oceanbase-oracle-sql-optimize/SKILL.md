---
name: oceanbase-oracle-sql-optimize
description: OceanBase Oracle 模式 SQL 优化通用指南。适用于复杂 CTE 查询、Hint 调优、分页明细拆分、执行计划排查、以及避免在结果集层重复子查询等场景。Use when optimizing OceanBase Oracle SQL, tuning CTE-heavy report queries, or diagnosing slow execution plans.
---

# OceanBase Oracle SQL 优化通用指南

这个 skill 只讲**通用优化方法**，不绑定具体业务表。

适用场景：

- SQL 很长，`WITH` 里有很多 CTE
- 执行计划里出现大表 `FULL`
- 统计 SQL 慢，明细 SQL 更慢
- 分页明细一次查全量顶不住
- 同一个口径在统计和明细里反复写
- OceanBase Oracle 模式下需要靠 Hint 和结构调整提速

## 核心原则

1. **先缩主集，再做补充关联**
2. **同一业务口径只定义一次**
3. **统计、分页明细、全量导出分开**
4. **先改结构，再补 Hint**
5. **不要在最终结果层重复写相关子查询**

## 一、CTE 拆分原则

### 1. 先有主集 CTE

任何复杂查询，优先先做一个“主数据集”：

```sql
base_orders as (
  select ...
  from main_table
  where ...
)
```

作用：

- 把时间范围、状态、主口径先锁住
- 让后续所有子逻辑围绕同一批订单/用户
- 降低后续 join 参与行数

### 2. 再拆辅助口径 CTE

常见辅助 CTE：

- 小码表/小清单
- 转换关系表
- 目标订单号集合
- 用户集合
- 分母集合
- 分子集合

推荐顺序：

```sql
small_lookup
main_scope
base_orders
helper_keys
helper_ref
base_detail
metrics
agg
```

### 3. 不要一个 CTE 混太多职责

反例：

- 一个 CTE 里既做主数据过滤
- 又做转套补充
- 又做精准剔除
- 又做分公司归属

推荐拆成多个可解释的小层。

## 二、Hint 使用顺序

Hint 是**第二选择**，不是第一选择。

优先级建议：

1. 先改 SQL 结构
2. 再补索引
3. 再收统计信息
4. 最后再加 Hint

## 三、常用 Hint 场景

### 1. `materialize`

适合：

- 小而反复使用的 CTE
- 已经缩小过的主集
- `distinct` 后结果集明显变小的集合

示例：

```sql
small_lookup as (
  select /*+ materialize */ ...
)
```

### 2. `result_cache`

适合：

- 码表
- 剔除清单
- 行数很小且变化少的配置表

示例：

```sql
lookup_table as (
  select /*+ materialize result_cache cardinality(40) */ ...
)
```

### 3. `cardinality`

适合：

- 优化器误判小表大小

示例：

```sql
select /*+ cardinality(40) */ ...
```

### 4. `parallel`

适合：

- 全表扫描成本合理
- 大表统计
- 汇总型 SQL

不适合：

- 小查询
- 高频点查
- 本来就能很好走索引的 SQL

### 5. `full / no_index`

适合：

- 范围很大
- 走索引反而回表更慢
- 执行计划老是走错小索引

示例：

```sql
select /*+ full(a) no_index(a) */ ...
from big_table a
```

### 6. `use_hash / no_use_nl`

适合：

- 大表 join 大表
- 主集已经物化
- 连接列过滤后仍有较多数据

### 7. `leading`

适合：

- 明确知道 join 顺序时
- 优化器总是从错误的大表先开

## 四、哪些情况不要在结果集层写子查询

尽量避免这种：

```sql
select a.*,
       (select count(1) from t2 where t2.id = a.id) as cnt
from t1 a
```

原因：

- 每行都可能触发一次子查询
- 执行计划容易变成 `FILTER`
- OceanBase / Oracle 上尤其容易炸性能

推荐改成：

```sql
with t2_agg as (
  select id, count(1) as cnt
  from t2
  group by id
)
select a.*, nvl(b.cnt, 0) as cnt
from t1 a
left join t2_agg b
  on a.id = b.id
```

## 五、`NOT EXISTS` 的替代思路

对**小剔除表**，优先考虑：

```sql
left join exclude_list e
  on a.code = e.code
where e.code is null
```

好处：

- 容易和 `materialize/result_cache` 配合
- 避免相关子查询反复执行

但如果剔除表很大，还是要对比执行计划，不要机械替换。

## 六、分页明细的标准拆法

不要让弹窗直接查全量。

建议至少拆成 3 个查询：

1. `DetailCount`
2. `DetailPage`
3. `Detail`（导出用全量）

### Count

```sql
with base_detail as (...)
select count(1) as total_count
from base_detail
```

### Page

```sql
with base_detail as (...)
select *
from (
  select t.*,
         row_number() over(order by order_time desc) as rn
  from base_detail t
)
where rn >= to_number('#{startRow}')
  and rn <= to_number('#{endRow}')
```

### Export

全量明细单独保留，前端只有导出时才调。

## 七、可选条件写法

OceanBase Oracle 下动态 SQL 特别容易因为可选条件拼错报语法错。

正确：

```sql
where 1 = 1
  and orderNo[
    order_no = ''#{orderNo}''
  ]
```

错误：

```sql
where (...)
orderNo[
  and order_no = ''#{orderNo}''
]
```

因为参数为空时会被替换成：

```sql
1 = 1
```

没有 `and` 承接就会直接报错。

## 八、索引字段不要套函数

错误示例：

```sql
to_date(a.order_time, 'YYYY-MM-DD HH24:MI:SS') >= ...
substr(a.order_time, 1, 6) = ...
```

优先改成：

```sql
a.order_time >= '#{startTime}'
and a.order_time <= '#{endTime}'
```

如果字段是日期类型，则：

```sql
a.order_time >= to_date('#{startTime}', 'YYYY-MM-DD HH24:MI:SS')
```

即：**函数尽量作用在参数，不作用在列**。

## 九、金额字段要先确认单位

常见坑：

- 表里存的是“分”
- 口径写成了“元”

如果金额单位是分：

```sql
nvl(first_charge_amount, 0) >= 10000
```

而不是：

```sql
nvl(first_charge_amount, 0) >= 100
```

明细展示如果要转元：

```sql
case
  when first_charge_amount is null or first_charge_amount = ''''
    then null
  else round(to_number(first_charge_amount) / 100, 2)
end
```

## 十、执行计划排查顺序

看执行计划时优先盯这些：

1. 是否有大表 `FULL`
2. 是否有 `FILTER`
3. 是否有 `USE_CONCAT`
4. 是否同一个大表被多次全扫
5. 小表是否被误判成大表

如果看到：

- 大表 `FULL`
- 小表没物化
- `NOT EXISTS`
- 结果列相关子查询

基本都可以先按这个 skill 里的思路收一轮。

## 参考

更细的示例和分页/导出拆分参考见 [reference.md](reference.md)。
