<p align="right">
  <a href="README.md">English</a> | <strong>中文</strong>
</p>

# Open Source Skills

![Skills](https://img.shields.io/badge/Agent%20Skills-6-2F6FED)
![Format](https://img.shields.io/badge/Format-SKILL.md-111827)
![Languages](https://img.shields.io/badge/README-English%20%7C%20Chinese-16A34A)
![License](https://img.shields.io/badge/License-Mixed-lightgrey)

这是一个可复用的 agent skills 集合，覆盖前端设计、技能发现、开源 README 编写、OceanBase Oracle SQL 优化、个人开发习惯，以及 skill 创建与打包。

## Highlights

- 6 个可直接复制使用的 skill 文件夹，每个都围绕一个真实 coding-agent 工作流。
- 提供英文和中文 README，方便公开展示和中文用户阅读。
- 对第三方或改造 skill 标注来源判断，避免误写成原创。
- 保留上游自带的 per-skill license 文件。
- 结构轻量，不需要包管理器、构建步骤或运行时依赖即可使用。

## Skills

| Skill | 作用 | 来源 / 归属判断 |
| --- | --- | --- |
| [`frontend-design`](frontend-design/) | 指导 agent 构建更有设计感、更完整的前端界面，避免常见模板化 UI。适用于网站、后台、落地页、组件和可视化 Web 应用。 | 第三方。根据内置 `LICENSE.txt`、文本风格和公开 skill 列表判断，来源应为 Anthropic 的 frontend design skill。 |
| [`skill-creator`](skill-creator/) | 帮助创建、改进、验证和打包 agent skill，包含 metadata、渐进式披露、references、scripts 和质量校验等规范。 | 第三方。根据内置 `LICENSE.txt`、Claude 相关措辞和公开 skill 列表判断，来源应为 Anthropic 的 skill creator。 |
| [`find-skills`](find-skills/) | 帮助 agent 发现现有 skill、搜索 skill 生态、判断来源质量，并给出安装建议。 | 第三方或改造版。内容与 [`vercel-labs/skills`](https://github.com/vercel-labs/skills) 和 skills.sh 上公开的 `find-skills` 高度一致。 |
| [`github-open-source-readme`](github-open-source-readme/) | 创建或润色适合 GitHub 开源项目的 README，支持中英文结构、徽章、截图、启动命令、安全说明、联系信息和 AI 辅助开发说明。 | 原创 / 个人工作流。 |
| [`oceanbase-oracle-sql-optimize`](oceanbase-oracle-sql-optimize/) | 提供 OceanBase Oracle 模式 SQL 优化指南，适合复杂 CTE 报表、Hint 调优、分页明细拆分、执行计划排查和避免重复相关子查询等场景。 | 原创 / 个人工作流。 |
| [`personal-dev-habits`](personal-dev-habits/) | 记录个人通用开发偏好，包括注释习惯、函数封装取舍、Java 后端事务检查和交付说明。 | 原创 / 个人工作流。 |

## Requirements

- 一个支持 `SKILL.md` 本地 skill 的 coding-agent 环境。
- 如需 clone 仓库，需要 Git。
- 只有运行 `skill-creator/scripts/` 下的辅助脚本时，才需要 Python 3。

## Quick Start

克隆仓库：

```bash
git clone https://github.com/tangguo95/open-source-skills.git
cd open-source-skills
```

把需要的 skill 复制到你的 agent skills 目录。以 Codex 为例：

```bash
mkdir -p ~/.codex/skills
cp -R frontend-design ~/.codex/skills/frontend-design
```

如果你的 agent 环境不会自动加载新 skill，复制后重启或刷新一次。

## Usage

建议只安装自己需要的 skill：

```bash
cp -R github-open-source-readme ~/.codex/skills/github-open-source-readme
cp -R oceanbase-oracle-sql-optimize ~/.codex/skills/oceanbase-oracle-sql-optimize
```

可以通过描述任务触发 skill；如果你的 agent 支持显式 skill 调用，也可以直接点名。

示例：

```text
Use $frontend-design to polish this React dashboard.
Use $github-open-source-readme to prepare this project for GitHub.
Use $oceanbase-oracle-sql-optimize to review this slow report SQL.
```

## Project Structure

```text
.
├── README.md
├── README_ZH.md
├── find-skills/
│   └── SKILL.md
├── frontend-design/
│   ├── LICENSE.txt
│   └── SKILL.md
├── github-open-source-readme/
│   ├── SKILL.md
│   └── agents/openai.yaml
├── oceanbase-oracle-sql-optimize/
│   ├── SKILL.md
│   └── reference.md
├── personal-dev-habits/
│   └── SKILL.md
└── skill-creator/
    ├── LICENSE.txt
    ├── SKILL.md
    ├── references/
    └── scripts/
```

## Notes / FAQ

### 这些 skill 都是原创的吗？

不是。这是一个混合来源集合，里面既有个人工作流，也有第三方或基于公开 skill 改造的内容。

### 哪些 skill 看起来是第三方？

`frontend-design` 和 `skill-creator` 带有 Apache-2.0 许可证文本，并包含 Claude/Anthropic 风格措辞，因此按 Anthropic 衍生第三方 skill 处理。`find-skills` 与 `vercel-labs/skills` 中公开的 `find-skills` 内容高度一致。

### 可以把整个仓库复制到 agent skills 目录吗？

可以，但通常建议只复制需要的 skill 文件夹。每个 skill 都是独立的，按需安装能让 skill 列表更清爽。

### 为什么 License 写 mixed？

因为仓库里包含多个来源。有些目录自带 `LICENSE.txt`，而个人原创 skill 目前还没有单独的全仓库许可证。

## Security Notes

- 安装第三方 skill 前，建议先阅读源码。
- 不要把私有项目路径、内网地址、账号密码、API key 或个人数据写进公开 skill。
- 二次分发或修改第三方 skill 时，请保留原有许可证文件和来源说明。

## AI 辅助开发

本项目在作者主导与审阅下，借助 OpenAI Codex 完成整理、文档编写与细节打磨。

## Contact

作者：[tangguo95](https://github.com/tangguo95)

## License

这是一个混合来源的 skill 集合：

- 带有独立 `LICENSE.txt` 的 skill，按该目录内许可证执行。
- 没有内置许可证的第三方或改造 skill，建议在二次分发前确认其上游仓库条款。
- 本仓库中的原创 skill 可用于个人 agent 工作流；如后续添加更明确的许可证，以新许可证为准。

