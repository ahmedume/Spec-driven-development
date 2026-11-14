# Third-Party Notices

This repository redistributes third-party skills and fonts. Everything third-party is inventoried here, with the license that applies, the required attribution, and what was removed. The repository's own content (README, `docs/`, `AGENTS.md`, and the three harness-authored skills) is MIT-licensed — see [`LICENSE`](LICENSE).

---

## A. Skills with a license file in this repository

### A1. Apache License 2.0 — 9 skills

These skills ship an unmodified Apache License 2.0 `LICENSE.txt` (no copyright appendix recorded; consistent with Anthropic's public skills distribution). You may copy and use them under those terms, keeping their `LICENSE.txt` files intact.

Skills: `artifacts-builder`, `brand-guidelines`, `canvas-design`, `internal-comms`, `mcp-builder`, `skill-creator`, `slack-gif-creator`, `theme-factory`, `webapp-testing`.

### A2. SIL Open Font License 1.1 — 26 fonts in `canvas-design`

`canvas-design/canvas-fonts/` bundles 26 font families, each with its own OFL license file (`*-OFL.txt`). Do not remove or edit those files.

Fonts: Arsenal SC, Big Shoulders, Boldonse, Bricolage Grotesque, Crimson Pro, DM Mono, Erica One, Geist Mono, Gloock, IBM Plex Mono, Instrument Sans, Italiana, JetBrains Mono, Jura, Libre Baskerville, Lora, National Park, Nothing You Could Do, Outfit, Pixelify Sans, Poiret One, Red Hat Mono, Silkscreen, Smooch Sans, Tektur, Work Sans, Young Serif.

### A3. MIT — `ui-ux-pro-max`

- **Source:** `nextlevelbuilder/ui-ux-pro-max-skill` — https://github.com/nextlevelbuilder/ui-ux-pro-max-skill
- **License:** MIT · Copyright (c) 2024 Next Level Builder
- **Included at:** `skills/ui-ux-pro-max/`
- **Modifications:** none.

---

## B. MIT — Jeffallan's claude-skills — 66 skills

Sixty-six skills are sourced from **[Jeffallan/claude-skills](https://github.com/Jeffallan/claude-skills)**, published under the MIT License.

> MIT License
> Copyright (c) 2025
> Permission is hereby granted, free of charge, to any person obtaining a copy
> of this software and associated documentation files (the "Software"), to deal
> in the Software without restriction, including without limitation the rights
> to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
> copies of the Software, and to permit persons to whom the Software is
> furnished to do so, subject to the following conditions:
>
> The above copyright notice and this permission notice shall be included in all
> copies or substantial portions of the Software.

Skills: `angular-architect`, `api-designer`, `architecture-designer`, `atlassian-mcp`, `chaos-engineer`, `cli-developer`, `cloud-architect`, `code-documenter`, `code-reviewer`, `cpp-pro`, `csharp-developer`, `database-optimizer`, `debugging-wizard`, `devops-engineer`, `django-expert`, `dotnet-core-expert`, `embedded-systems`, `fastapi-expert`, `feature-forge`, `fine-tuning-expert`, `flutter-expert`, `fullstack-guardian`, `game-developer`, `golang-pro`, `graphql-architect`, `java-architect`, `javascript-pro`, `kotlin-specialist`, `kubernetes-specialist`, `laravel-specialist`, `legacy-modernizer`, `mcp-developer`, `microservices-architect`, `ml-pipeline`, `monitoring-expert`, `nestjs-expert`, `nextjs-developer`, `pandas-pro`, `php-pro`, `playwright-expert`, `postgres-pro`, `prompt-engineer`, `python-pro`, `rag-architect`, `rails-expert`, `react-expert`, `react-native-expert`, `rust-engineer`, `salesforce-developer`, `secure-code-guardian`, `security-reviewer`, `shopify-expert`, `spark-engineer`, `spec-miner`, `spring-boot-engineer`, `sql-pro`, `sre-engineer`, `swift-expert`, `terraform-engineer`, `test-master`, `the-fool`, `typescript-pro`, `vue-expert`, `vue-expert-js`, `websocket-engineer`, `wordpress-pro`.

**Attribution:** Jeffallan — https://github.com/Jeffallan · upstream MIT License
**Modifications:** the `author:` frontmatter in `backend-mastery` was removed (repository-owner cleanup). No other changes.

---

## C. Harness-authored — 3 skills

`spec-driven-dev`, `backend-mastery`, `template-skill` were authored within this repository's workflow. They are covered by the repository's [`LICENSE`](LICENSE) (MIT).

---

## D. Removed for redistribution reasons

The following 24 third-party skills carried no author attribution and no license file in this repository. Because they granted no redistribution rights and this repository is public, they were removed. Install them from upstream instead:

`awwwards`, `brag-slim`, `caveman`, `changelog-generator`, `code-security-auditor`, `competitive-ads-extractor`, `content-research-writer`, `developer-growth-analysis`, `domain-name-brainstormer`, `elite-frontend-dev`, `file-organizer`, `high-front-end`, `image-enhancer`, `invoice-organizer`, `jury`, `karpathy-guidelines`, `lead-research-assistant`, `meeting-insights-analyzer`, `project-report-generator`, `raffle-winner-picker`, `readme-generator`, `skill-share`, `staff-engineer-review`, `video-downloader`.

Four Anthropic document skills were also removed. They shipped all-rights-reserved licences governed by Anthropic's ToS (use is subject to Anthropic's Consumer or Commercial Terms of Service):

`docx`, `pdf`, `pptx`, `xlsx`.

**Where to get them:**

```bash
npx skills add <owner/repo> --skill <skill-name>
```

or browse [skills.sh](https://skills.sh). The Anthropic document skills live at [github.com/anthropics/skills](https://github.com/anthropics/skills).

These removals are about redistribution rights, not quality.

---

## E. Rules for everyone

1. Keep `LICENSE`, `THIRD_PARTY_NOTICES.md`, and the `*-OFL.txt` files unchanged and together.
2. If you add a skill, note its source and license in its `SKILL.md` frontmatter and update this file in the same commit.
3. Do not strip attribution from vendored skills to make a fork look clean. It defeats the license, not the noise.
4. If you find a skill in this repository missing from this file, that is a bug — open an issue or a PR.
