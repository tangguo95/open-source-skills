# OceanBase Oracle SQL 优化参考

## 1. 复杂报表 SQL 的推荐骨架

```sql
with small_lookup as (
  select /*+ materialize result_cache cardinality(40) */ ...
  from small_table
),
main_scope as (
  select /*+ materialize */ ...
  from scope_table
  where ...
),
base_orders as (
  select ...
  from main_table
  where ...
),
helper_keys as (
  select distinct ...
  from base_orders
  where ...
),
helper_ref as (
  select ...
  from ref_table
  join helper_keys ...
),
base_detail as (
  select ...
  from base_orders
  left join small_lookup ...
  left join helper_ref ...
),
metrics as (
  select ...
  from base_detail
  group by ...
)
select ...
from metrics
```

## 2. 明细分页推荐骨架

### Count SQL

```sql
with base_detail as (...)
select count(1) as total_count
from base_detail
```

### Page SQL

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

### Export SQL

```sql
with base_detail as (...)
select *
from base_detail
order by order_time desc
```

## 3. 常用 Hint 组合

### 小表物化

```sql
select /*+ materialize result_cache cardinality(40) */ ...
```

### 大表全扫并行

```sql
select /*+ materialize parallel(a,16) full(a) no_index(a) */ ...
from big_table a
```

### Hash Join

```sql
select /*+ leading(k d) use_hash(d) no_use_nl(d) */ ...
from keys k
join detail d on ...
```

### Hash 聚合

```sql
select /*+ use_hash_aggregation gby_pushdown */ ...
from ...
group by ...
```

## 4. 子查询改 JOIN 的通用模板

### 反例

```sql
select a.*,
       (select count(1) from t2 where t2.id = a.id) as cnt
from t1 a
```

### 改写

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

## 5. `NOT EXISTS` 改写模板

### 反例

```sql
where not exists (
  select 1
  from exclude_list e
  where e.code = a.code
)
```

### 改写

```sql
left join exclude_list e
  on e.code = a.code
where e.code is null
```

## 6. 可选条件模板

### 正确

```sql
where 1 = 1
  and areaCode[
    area_code = '#{areaCode}'
  ]
  and startTime[
    order_time >= '#{startTime}'
  ]
```

### 错误

```sql
where (...)
areaCode[
  and area_code = '#{areaCode}'
]
```

## 7. 金额字段（分转元）

### 判断阈值

```sql
nvl(amount, 0) >= 10000
```

### 明细展示

```sql
case
  when amount is null or amount = ''''
    then null
  else round(to_number(amount) / 100, 2)
end
```

## 8. 订单量 vs 用户量

### 订单量

```sql
select distinct region_code, order_no
from ...
```

再：

```sql
select region_code, count(*) as order_cnt
from order_set
group by region_code
```

### 用户量

```sql
select distinct region_code, user_id
from ...
```

再：

```sql
select region_code, count(*) as user_cnt
from user_set
group by region_code
```

不要直接混着用。

## 9. 统计和明细保持同口径

如果统计口径有专用数据集，例如：

- `active_raw_orders`
- `active_order_base_filtered`

那么明细里同一指标也应该复用同一套数据集，不能偷懒复用普通主集。

## 10. 典型报错排查

### ORA-00900 / SQL syntax error

优先检查：

- 可选条件 `paramName[...]` 是否缺 `and`
- 字符串 SQL 里的单引号是否转义不够
- `''` 是否写成了 `'`

### 统计慢

优先检查：

- 是否大表全扫多次
- 是否结果列里还有相关子查询
- 是否 CTE 被优化器反复展开
- 是否可以拆成分页 / 总数 / 导出三条 SQL
