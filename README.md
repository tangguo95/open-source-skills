<p align="right">
  <strong>English</strong> | <a href="README_ZH.md">中文</a>
</p>

# Open Source Skills

![Skills](https://img.shields.io/badge/Agent%20Skills-6-2F6FED)
![Format](https://img.shields.io/badge/Format-SKILL.md-111827)
![Languages](https://img.shields.io/badge/README-English%20%7C%20Chinese-16A34A)
![License](https://img.shields.io/badge/License-Mixed-lightgrey)

A reusable collection of agent skills for frontend design, skill discovery, README writing, OceanBase Oracle SQL optimization, personal coding habits, and skill authoring.

## Highlights

- Six ready-to-copy skill folders, each centered on a practical coding-agent workflow.
- Bilingual project documentation for English and Chinese readers.
- Clear attribution notes for third-party or adapted skills.
- Preserved per-skill license files where bundled upstream.
- Lightweight structure: no package manager, build step, or runtime dependency is required to use the skills.

## Skills

| Skill | What it does | Source / attribution |
| --- | --- | --- |
| [`frontend-design`](frontend-design/) | Guides agents to build polished, distinctive frontend interfaces instead of generic UI output. Useful for websites, dashboards, landing pages, components, and visual web apps. | Third-party. Inferred from the bundled license, wording, and public listings as an Anthropic frontend design skill. Keeps its own `LICENSE.txt`. |
| [`skill-creator`](skill-creator/) | Helps create, improve, validate, and package agent skills with good metadata, progressive disclosure, references, scripts, and quality checks. | Third-party. Inferred from the bundled license, Claude-specific wording, and public listings as an Anthropic skill. Keeps its own `LICENSE.txt`. |
| [`find-skills`](find-skills/) | Helps agents discover existing skills, search the skills ecosystem, compare source quality, and recommend install commands. | Third-party or adapted. The content closely matches the public `find-skills` skill from [`vercel-labs/skills`](https://github.com/vercel-labs/skills) and skills.sh listings. |
| [`github-open-source-readme`](github-open-source-readme/) | Creates or polishes GitHub-facing README files, including bilingual English/Chinese structure, badges, screenshots, startup commands, security notes, contact, and attribution sections. | Original / personal workflow. |
| [`oceanbase-oracle-sql-optimize`](oceanbase-oracle-sql-optimize/) | Provides practical guidance for optimizing OceanBase Oracle-mode SQL, especially CTE-heavy reports, hints, pagination/detail splits, execution-plan review, and avoiding repeated correlated subqueries. | Original / personal workflow. |
| [`personal-dev-habits`](personal-dev-habits/) | Captures personal coding preferences for comments, helper extraction, Java backend transaction checks, and delivery notes. | Original / personal workflow. |

## Requirements

- A coding-agent environment that supports local skills with `SKILL.md` files.
- Git, if you want to clone this repository.
- Python 3 is only needed when you run the helper scripts under `skill-creator/scripts/`.

## Quick Start

Clone the repository:

```bash
git clone https://github.com/tangguo95/open-source-skills.git
cd open-source-skills
```

Copy a skill into your agent's skill directory. For Codex, for example:

```bash
mkdir -p ~/.codex/skills
cp -R frontend-design ~/.codex/skills/frontend-design
```

Then restart or reload your agent environment if it does not pick up new skills automatically.

## Usage

Install only the skill folders you need:

```bash
cp -R github-open-source-readme ~/.codex/skills/github-open-source-readme
cp -R oceanbase-oracle-sql-optimize ~/.codex/skills/oceanbase-oracle-sql-optimize
```

Trigger a skill by asking for the task it describes, or by naming the skill directly if your agent supports explicit skill invocation.

Examples:

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

### Are all skills original?

No. This is a mixed collection. Some skills are personal workflows, and some are third-party or adapted from public skill ecosystems.

### Which skills appear to be third-party?

`frontend-design` and `skill-creator` include Apache-2.0 license text and Claude/Anthropic-oriented wording, so they are treated as Anthropic-derived third-party skills. `find-skills` closely matches public listings for the `find-skills` skill from `vercel-labs/skills`.

### Can I copy the whole repository into my agent skills directory?

You can, but copying selected folders is usually cleaner. Each skill is independent, so installing only the ones you need keeps your skill list easier to scan.

### Why is the license marked as mixed?

This repository contains multiple skill sources. Some folders have their own `LICENSE.txt`, while other personal skills do not currently have a separate project-wide license.

## Security Notes

- Review third-party skills before installing them in an agent environment.
- Do not add private project paths, internal hostnames, credentials, API keys, or personal data to public skills.
- Keep bundled license files when redistributing or modifying third-party skills.

## AI-assisted Development

This project was organized, documented, and polished with the assistance of OpenAI Codex, under the author's direction and review.

## Contact

Author: [tangguo95](https://github.com/tangguo95)

## License

This repository is a mixed-source skill collection:

- Skills with their own `LICENSE.txt` are governed by that license.
- Third-party or adapted skills without a bundled license should be checked against their upstream repository before redistribution.
- Original skills in this repository may be reused for personal agent workflows unless a stricter license is added later.

