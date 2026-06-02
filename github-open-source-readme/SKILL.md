---
name: github-open-source-readme
description: Create or polish README files for public GitHub open-source repositories, including bilingual English/Chinese README structure, language switching links, badges, screenshots, quick-start commands, license/contact sections, AI-assisted development attribution, GitHub publishing details, and launch-ready visual formatting. Use when the user asks to prepare, beautify, split, translate, or improve README.md/README_ZH.md for an open-source GitHub project, especially before uploading to GitHub. Do not use for enterprise-internal project documentation unless the user explicitly wants public open-source README style.
---

# GitHub Open Source README

## Goal

Produce a polished GitHub-facing README that helps strangers quickly understand, start, trust, and reuse an open-source project.

Prefer two files for bilingual repos:

- `README.md`: English default homepage for GitHub.
- `README_ZH.md`: Chinese version.

Add language switches near the top:

```markdown
<p align="right">
  <strong>English</strong> | <a href="README_ZH.md">中文</a>
</p>
```

```markdown
<p align="right">
  <a href="README.md">English</a> | <strong>中文</strong>
</p>
```

## Workflow

1. Inspect the repository before writing.
   - Read existing README files, package metadata, dependency files, entrypoints, startup scripts, screenshots, license, and git remote.
   - Identify the real project name, technology stack, supported runtime, and exact commands.
   - For apps, verify whether the project can actually start locally before documenting commands.

2. Decide README layout.
   - Public GitHub repositories should lead with project identity, badges, screenshots, highlights, quick start, and usage.
   - Keep enterprise-only deployment details, internal hostnames, private credentials, and internal business wording out of public README text.
   - If the project is primarily Chinese-authored but intended for GitHub, still keep `README.md` in English and link to `README_ZH.md`.

3. Add screenshots when the project has a UI.
   - Use sanitized demo data only.
   - Prefer `docs/images/login.jpg`, `docs/images/dashboard.jpg`, or similarly descriptive names.
   - Use browser automation for local apps when available; crop or regenerate screenshots if secrets, private URLs, tokens, or user data appear.
   - Reference screenshots with relative Markdown paths, for example:

```markdown
![Dashboard](docs/images/dashboard.jpg)
```

4. Add badges without overloading the header.
   - Use stable shields such as license, Python/Node/runtime, GitHub repo status, release/version when applicable.
   - Keep badges truthful; do not claim CI, downloads, stars, or package versions that do not exist.
   - Example:

```markdown
![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green.svg)
```

5. Document startup clearly.
   - Include prerequisites with versions.
   - Include clone, install, run, and open-browser commands.
   - If the app supports background running or launch agents, include it only after the basic foreground start.
   - Use commands that match the repository exactly.

6. Explain real behavior and limitations.
   - Separate "Features" from "Notes" or "FAQ".
   - Be precise about destructive operations, storage estimates, authentication behavior, cleanup/GC behavior, and security assumptions.
   - For Docker Registry projects, mention that deleting manifests/tags may not immediately release disk space; registry garbage collection is usually required.

7. Include open-source finishing sections.
   - `Project Structure`
   - `Security Notes`
   - `FAQ` when users are likely to misunderstand behavior
   - `Contributing` if the user wants community contributions
   - `License` with `MIT` or the actual license
   - `Contact` with author name/email when provided
   - `Acknowledgements` or `AI-assisted Development` if the user used AI coding tools

8. Validate and publish.
   - Check Markdown links and image paths.
   - Ensure no secrets or internal-only values are present.
   - Run project tests or at least syntax checks if README commands mention them.
   - If the user asks to upload to GitHub, use `gh`/git when authenticated, create a concise repo name, commit, push, and report the URL.

## Suggested README Structure

For `README.md`:

```markdown
<p align="right">
  <strong>English</strong> | <a href="README_ZH.md">中文</a>
</p>

# Project Name

[badges]

Short one-paragraph description.

![Main Screenshot](docs/images/dashboard.jpg)

## Highlights

## Requirements

## Quick Start

## Usage

## Project Structure

## Notes / FAQ

## Security Notes

## AI-assisted Development

## Contact

## License
```

For `README_ZH.md`, mirror the same structure in natural Chinese. Do not machine-translate literally; adapt phrasing so it reads like a Chinese open-source README.

## Wording Patterns

Use restrained, practical open-source language:

- "A lightweight web console for ..."
- "Designed for local operation and private registry maintenance."
- "This value is an estimate based on registry API metadata, not a filesystem quota."
- "Deleting a manifest removes the registry reference; disk space is normally reclaimed only after registry garbage collection."

For AI-assisted development:

```markdown
## AI-assisted Development

This project was designed, implemented, documented, and polished with the assistance of OpenAI Codex, under the author's direction and review.
```

Chinese version:

```markdown
## AI 辅助开发

本项目在作者主导与审阅下，借助 OpenAI Codex 完成设计、实现、文档编写与细节打磨。
```

If the user names a model version, include it exactly as the user provided.

## Quality Bar

Before finishing, ensure:

- The README can be understood without previous chat context.
- The first screen shows what the project is, what it does, and why it is useful.
- Startup commands are copy-pasteable and verified when possible.
- Screenshots are visible, useful, and free of secrets.
- English and Chinese versions link to each other and stay consistent.
- License and contact information match actual files/user-provided details.
- No private registry addresses, usernames, passwords, tokens, or internal company names are exposed unless the user explicitly wants them public.
