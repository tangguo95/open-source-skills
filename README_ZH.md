<p align="right">
  <a href="README.md">English</a> | <strong>中文</strong>
</p>

# Open Source Skills

这是一个面向公开复用的 agent skills 集合，覆盖前端设计、技能发现、技能编写、开源 README、OceanBase Oracle SQL 优化，以及个人开发习惯等场景。

这个仓库里既有从日常开发中沉淀出来的原创/个人工作流，也有来自公开 skill 生态或基于公开 skill 改造的内容。下面会把每个 skill 的用途、来源判断和许可证注意事项列清楚，避免把第三方内容误写成原创。

## Skills

| Skill | 作用 | 来源 / 归属判断 |
| --- | --- | --- |
| [`frontend-design`](frontend-design/) | 指导 agent 构建更有设计感、更完整的前端界面，避免常见的模板化 UI。适用于网站、后台、落地页、组件和可视化 Web 应用。 | 第三方。根据内置 `LICENSE.txt`、文本风格和公开 skill 列表判断，来源应为 Anthropic 的 frontend design skill。 |
| [`skill-creator`](skill-creator/) | 帮助创建、改进、验证和打包 agent skill，包含 metadata、渐进式披露、references、scripts 和质量校验等规范。 | 第三方。根据内置 `LICENSE.txt`、Claude 相关措辞和公开 skill 列表判断，来源应为 Anthropic 的 skill creator。 |
| [`find-skills`](find-skills/) | 帮助 agent 发现现有 skill、搜索 skill 生态、判断来源质量，并给出安装建议。 | 第三方或改造版。内容与 [`vercel-labs/skills`](https://github.com/vercel-labs/skills) 和 skills.sh 上公开的 `find-skills` 高度一致。 |
| [`github-open-source-readme`](github-open-source-readme/) | 创建或润色适合 GitHub 开源项目的 README，支持中英文结构、徽章、截图、启动命令、安全说明、联系信息和 AI 辅助开发说明。 | 原创 / 个人工作流。 |
| [`oceanbase-oracle-sql-optimize`](oceanbase-oracle-sql-optimize/) | 提供 OceanBase Oracle 模式 SQL 优化指南，适合复杂 CTE 报表、Hint 调优、分页明细拆分、执行计划排查和避免重复相关子查询等场景。 | 原创 / 个人工作流。 |
| [`personal-dev-habits`](personal-dev-habits/) | 记录个人通用开发偏好，包括注释习惯、函数封装取舍、Java 后端事务检查和交付说明。 | 原创 / 个人工作流。 |

## 使用方式

把需要的 skill 文件夹复制到你的 coding agent 支持的 skills 目录中即可。

例如：

```bash
cp -R frontend-design ~/.codex/skills/frontend-design
```

也可以把这个仓库作为本地 skill library，需要哪个就复制哪个到对应项目里。

## 目录结构

```text
.
├── find-skills/
├── frontend-design/
├── github-open-source-readme/
├── oceanbase-oracle-sql-optimize/
├── personal-dev-habits/
└── skill-creator/
```

每个 skill 都包含必需的 `SKILL.md`。部分 skill 还包含：

- `LICENSE.txt`：第三方许可证条款。
- `references/`：按需加载的补充资料。
- `scripts/`：可复用的校验或打包脚本。
- `agents/openai.yaml`：面向 Codex/OpenAI 的 UI metadata。

## 来源说明

以下判断基于本地文件内容和公开 skill 列表：

- `frontend-design` 和 `skill-creator` 带有 Apache-2.0 许可证文本，并包含 Claude/Anthropic 风格的措辞，因此按 Anthropic 衍生第三方 skill 处理。
- `find-skills` 与 `vercel-labs/skills` 中公开的 `find-skills` 内容高度一致。
- `github-open-source-readme`、`oceanbase-oracle-sql-optimize`、`personal-dev-habits` 更像本仓库作者自己的工作流沉淀。

如果你要二次分发或修改第三方 skill，请保留对应许可证和来源说明。

## License

这是一个混合来源的 skill 集合：

- 带有独立 `LICENSE.txt` 的 skill，按该目录内许可证执行。
- 没有内置许可证的第三方 skill，建议在二次分发前确认其上游仓库条款。
- 本仓库中的原创 skill 可用于个人 agent 工作流；如后续添加更明确的许可证，以新许可证为准。

## AI 辅助开发

本仓库在作者主导与审阅下，借助 OpenAI Codex 完成整理、README 编写与发布准备。

