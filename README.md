<p align="right">
  <strong>English</strong> | <a href="README_ZH.md">中文</a>
</p>

# Open Source Skills

A small collection of agent skills for coding, design, documentation, SQL optimization, and skill authoring.

This repository is meant to be a reusable public skill library. Some skills are original working notes distilled from day-to-day development, and some are collected from or adapted from public skill ecosystems. Attribution and license notes are listed below so users can tell what is personal, what is third-party, and what should be checked before reuse.

## Skills

| Skill | What it does | Source / attribution |
| --- | --- | --- |
| [`frontend-design`](frontend-design/) | Guides agents to build polished, distinctive frontend interfaces instead of generic UI output. Useful for websites, dashboards, landing pages, components, and visual web apps. | Third-party. Inferred from the bundled license, wording, and public listings as an Anthropic frontend design skill. Keeps its own `LICENSE.txt`. |
| [`skill-creator`](skill-creator/) | Helps create, improve, validate, and package agent skills with good metadata, progressive disclosure, references, scripts, and quality checks. | Third-party. Inferred from the bundled license, Claude-specific wording, and public listings as an Anthropic skill. Keeps its own `LICENSE.txt`. |
| [`find-skills`](find-skills/) | Helps agents discover existing skills, search the skills ecosystem, compare source quality, and recommend install commands. | Third-party or adapted. The content closely matches the public `find-skills` skill from [`vercel-labs/skills`](https://github.com/vercel-labs/skills) and skills.sh listings. |
| [`github-open-source-readme`](github-open-source-readme/) | Creates or polishes GitHub-facing README files, including bilingual English/Chinese structure, badges, screenshots, startup commands, security notes, contact, and attribution sections. | Original / personal workflow. |
| [`oceanbase-oracle-sql-optimize`](oceanbase-oracle-sql-optimize/) | Provides practical guidance for optimizing OceanBase Oracle-mode SQL, especially CTE-heavy reports, hints, pagination/detail splits, execution-plan review, and avoiding repeated correlated subqueries. | Original / personal workflow. |
| [`personal-dev-habits`](personal-dev-habits/) | Captures personal coding preferences for comments, helper extraction, Java backend transaction checks, and delivery notes. | Original / personal workflow. |

## How to Use

Copy the skill folder you need into the skills directory supported by your coding agent.

For example:

```bash
cp -R frontend-design ~/.codex/skills/frontend-design
```

Or keep this repository as a local skill library and copy selected folders into each project as needed.

## Repository Structure

```text
.
├── find-skills/
├── frontend-design/
├── github-open-source-readme/
├── oceanbase-oracle-sql-optimize/
├── personal-dev-habits/
└── skill-creator/
```

Each skill has a required `SKILL.md`. Some skills also include:

- `LICENSE.txt` for third-party license terms.
- `references/` for supporting documents that should be loaded only when needed.
- `scripts/` for reusable validation or packaging helpers.
- `agents/openai.yaml` for Codex/OpenAI-facing UI metadata.

## Attribution Notes

The following source judgments are based on local file contents and public skill listings:

- `frontend-design` and `skill-creator` include Apache-2.0 license text and Claude/Anthropic-oriented wording, so they are treated as Anthropic-derived third-party skills.
- `find-skills` closely matches public listings for the `find-skills` skill from `vercel-labs/skills`.
- `github-open-source-readme`, `oceanbase-oracle-sql-optimize`, and `personal-dev-habits` appear to be personal/original skills in this repository.

If you redistribute or modify third-party skills, keep their license files and attribution intact.

## License

This repository is a mixed collection:

- Skills with their own `LICENSE.txt` are governed by that license.
- Third-party skills without a bundled license should be checked against their upstream repository before redistribution.
- Original skills in this repository may be reused for personal agent workflows unless a stricter license is added later.

## AI-assisted Development

This repository was organized and documented with the assistance of OpenAI Codex, under the author's direction and review.

