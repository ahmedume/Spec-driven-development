# Spec-Driven Dev Harness

A portable AI coding environment: **107 skills**, a written method, and a design reference library — one clone, and every harness on the machine inherits the same standard.

Works with **OpenCode, Claude Code, Codex CLI, Cursor, Cline, and anything that reads the open Agent Skills format or `AGENTS.md`.**

The point is not the skills individually. It's that a new machine, a new teammate, or a new model starts at the same baseline as the one you've spent a year tuning.

---

## Why it's portable

A skill is a folder: `<skill-id>/SKILL.md` with `name` and `description` frontmatter, plus optional `references/`, `scripts/`, and `assets/`. That is the **open Agent Skills standard** — not an OpenCode invention. Every major agent reads the same folder shape; only the directory it scans differs.

So the skills are never rewritten per tool. The repo stays one source of truth, and each harness points at it.

---

## The problem this solves

Every AI coding setup decays the same way. You write a few good instructions, they get copied into three projects, two copies go stale, and eventually nobody knows which `AGENTS.md` is canonical. A new teammate starts from zero. A new model starts from zero. The knowledge lives in someone's chat history.

This repo is the opposite: **one clone, wired into every harness, and every session inherits the same standard.** Skills are plain markdown, so they're readable, diffable, and portable. Nothing is compiled, nothing is hosted.

---

## What you get

| | |
| --- | --- |
| **Skills** | 107, covering planning, engineering, design, data, infra, testing, review, documents, and business |
| **Method docs** | 9 structured guides — prompt engineering through to the 19-phase development lifecycle |
| **Design reference** | A 44-style frontend taxonomy and a 29-link component/inspiration index |
| **Format** | Markdown only. No build step, no runtime, no vendor lock-in |
| **Harnesses** | OpenCode, Claude Code, Codex CLI, Cursor, Cline, and `AGENTS.md`-native tools |
| **Attribution** | All 107 skills inventoried in [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md) — see *Licence and provenance* below |

---

## Install

### 1. Clone once, anywhere

```bash
git clone <your-repo-url> ~/.config/harness
```

Windows (PowerShell):

```powershell
git clone <your-repo-url> "$env:USERPROFILE\.config\harness"
```

The path is yours to choose — everything below points at `$HARNESS`, so substitute your own.

### 2. Point your harness at it

| Harness | Skills directory it scans | Wire it up |
| --- | --- | --- |
| **OpenCode** | any path listed in config, plus `~/.config/opencode/skills` | add `"skills": ["~/.config/harness/skills"]` to `~/.config/opencode/opencode.jsonc` |
| **Claude Code** | `~/.claude/skills` (personal) · `.claude/skills` (project) | junction or symlink |
| **Codex CLI** | `~/.agents/skills` (personal) · `.agents/skills` (repo) | junction or symlink |
| **Cursor** | `~/.cursor/skills` · `~/.agents/skills` · `.cursor/skills` · `.agents/skills` | junction or symlink |
| **Cline / Roo / others** | `.claude/skills`, `.agents/skills`, `.cursor/skills` | junction or symlink |
| **Any agent** | `AGENTS.md` | one line of text — see below |

**OpenCode** is the only one that takes a config path. Add this to `~/.config/opencode/opencode.jsonc`:

```jsonc
{
  "skills": ["~/.config/harness/skills"]
}
```

`~/` resolves from your home directory, so the path needs no editing per machine.

**For everything else**, link the clone's `skills/` folder into the directory the harness scans. On Windows, a **junction** needs no administrator rights:

```powershell
cmd /c mklink /J "$env:USERPROFILE\.claude\skills"   "$env:USERPROFILE\.config\harness\skills"
cmd /c mklink /J "$env:USERPROFILE\.agents\skills"   "$env:USERPROFILE\.config\harness\skills"
cmd /c mklink /J "$env:USERPROFILE\.cursor\skills"   "$env:USERPROFILE\.config\harness\skills"
```

macOS / Linux:

```bash
ln -s ~/.config/harness/skills ~/.claude/skills
ln -s ~/.config/harness/skills ~/.agents/skills
ln -s ~/.config/harness/skills ~/.cursor/skills
```

A junction is a pointer, not a copy — the files exist once, and `git pull` upgrades every harness at once.

### 3. The universal fallback: `AGENTS.md`

`AGENTS.md` is the cross-agent instruction convention, read by Codex, Cursor, Copilot, Cline, OpenCode, and most others. If a tool has no native skill support, this is the whole install:

```markdown
# Project instructions

Skills live in ~/.config/harness/skills. When a task matches a skill's
description, read skills/<skill-id>/SKILL.md before starting, and follow it.
```

Commit that to the repo root (or your home directory for a personal default). It costs nothing and works everywhere.

A ready-made version ships in this repo as [`AGENTS.md`](AGENTS.md) — copy it and adjust.

### 4. Verify

Restart the harness, then ask for a skill by name:

```
> use the the-fool skill on this plan
```

If it loads, the wiring is correct.

---

### Why point at the clone instead of copying

Copying 107 directories into each harness's folder looks simpler and isn't:

- **Updates** mean re-copying 819 files, per harness, forever.
- **Copies drift**, and you can no longer tell which version is live.
- **Windows symlinks** for directories need elevation; junctions don't, which is why the commands above use `mklink /J`.

One clone plus pointers means `git pull` is the upgrade path for every tool on the machine.

---

## Layout

```
spec-driv-dev/
├── README.md                  ← you are here
├── AGENTS.md                  ← harness default rules (copy into any project)
├── docs/                      ← the written method (9 guides)
│   ├── 01-prompt-engineering.md
│   ├── 02-context-engineering.md
│   ├── 03-skill-files.md
│   ├── 04-product-documents.md
│   ├── 05-prompt-anti-patterns.md
│   ├── 06-development-workflow.md
│   ├── 07-harness-rules.md
│   ├── 08-architecture-review.md
│   └── 09-spec-interrogation.md
├── skills/                    ← 107 skill directories
│   └── <skill-id>/
│       ├── SKILL.md           ← the instructions
│       └── references/        ← loaded on demand, not always-on
├── All-about-prompts/         ← original sources (docx + converted .md)
│   └── README.md              ← provenance: which raw file became which guide
└── frontend_design_styles.docx / .md ← source for the style taxonomy (44 styles)
```

The skill ID is derived from the **folder name**, not the `name:` field in the frontmatter. Where they differ, the folder wins.

---

## The pipeline

The reason this is called *spec-driven*: the spec is the product, and the code is a rendering of it.

```
                    ┌─────────────────────────────────────────┐
  one-line idea ──▶ │ 1. QUESTIONS                            │
                    │    ask before assuming; recommend, don't │
                    │    just ask                              │
                    └────────────────┬────────────────────────┘
                                     ▼
                    ┌─────────────────────────────────────────┐
                    │ 2. SPEC PACKAGE (9 files)                │
                    │    PRD · SRS/SPEC · SDS · TECH_STACK     │
                    │    BUILD_PLAN · DESIGN · SKILL · README  │
                    └────────────────┬────────────────────────┘
                                     ▼
                    ┌─────────────────────────────────────────┐
                    │ 3. ARCHITECTURE REVIEW                  │
                    │    what breaks first at 10k users?       │
                    └────────────────┬────────────────────────┘
                                     ▼
                    ┌─────────────────────────────────────────┐
                    │ 4. BUILD — one feature at a time        │
                    │    compact · tested · committed          │
                    └────────────────┬────────────────────────┘
                                     ▼
                    ┌─────────────────────────────────────────┐
                    │ 5. REVIEW & SHIP                        │
                    │    quality gate, security, docs, report  │
                    └─────────────────────────────────────────┘
```

### Which skill fires at which stage

| Stage | Load | What it does for you |
| --- | --- | --- |
| **Frame the problem** | `the-fool` | Challenges the premise before you spend a week on it |
| **Ask the questions** | `feature-forge` | Turns a conversation into user stories and acceptance criteria |
| **Write the spec** | `spec-driven-dev` | Produces the 9-file package |
| **Design the system** | `architecture-designer` · `api-designer` | Diagrams, ADRs, API contracts |
| **Review the design** | `architecture-designer` + [`08-architecture-review.md`](docs/08-architecture-review.md) | Catches the bottleneck before it's a rewrite |
| **Build the UI** | `elite-frontend-dev` | Bespoke, non-templated frontend |
| **Build the API** | `backend-mastery` · `fastapi-expert` / `nestjs-expert` / … | Correctness-first, tested |
| **Build the data layer** | `postgres-pro` · `sql-pro` | Schema, indexes, query plans |
| **Test it** | `test-master` · `playwright-expert` | Unit through E2E |
| **Review it** | `code-reviewer` · `staff-engineer-review` | Prioritised findings, not a list of nits |
| **Secure it** | `security-reviewer` | Severity-rated audit |
| **Ship it** | `devops-engineer` | CI/CD, containers, deploy |
| **Document it** | `readme-generator` · `project-report-generator` | README and formal report |

### The quality gate

Per feature, no exceptions:

```
implemented → types pass → lint passes → tests pass → manual test → commit
```

---

## Skill catalog

All 107 skills, grouped by what you'd reach for them to do.

### Spec, planning & architecture — 11

| Skill | What it does |
| --- | --- |
| `spec-driven-dev` | **The headline skill.** Turns a one-line idea into a 9-file spec package — PRD, SRS, SDS, tech stack, build plan, design system, project skill — before any code is written. |
| `feature-forge` | Runs a structured requirements workshop: user stories, EARS-format functional requirements, acceptance criteria, implementation checklist. |
| `architecture-designer` | Designs high-level architecture, writes ADRs, evaluates technology trade-offs, produces component diagrams. |
| `api-designer` | Designs REST and GraphQL surfaces: resource modelling, versioning, pagination, error standards, OpenAPI specs. |
| `spec-miner` | Reverse-engineers a specification out of an undocumented or legacy codebase. |
| `legacy-modernizer` | Plans incremental migration: service boundaries, dependency maps, strangler-fig roadmaps, API facades. |
| `microservices-architect` | Decomposes monoliths into bounded contexts; picks communication patterns, sagas, CQRS, event sourcing. |
| `graphql-architect` | Schema design, Apollo Federation, resolvers with DataLoader, live subscriptions. |
| `the-fool` | Structured critical reasoning: devil's advocate, pre-mortem, red team, assumption audit. |
| `karpathy-guidelines` | Behavioural guardrails against overcomplication — surgical changes, surfaced assumptions, verifiable success criteria. |
| `project-report-generator` | Generates a formal project report, chaptered from introduction through testing to conclusion. |

### Frontend & design — 12

| Skill | What it does |
| --- | --- |
| `elite-frontend-dev` | **The default site-build skill.** Bespoke design system, intentional motion, non-templated output. Trigger it before writing any HTML/CSS/JS. |
| `awwwards` | Builds award-calibre sites against the Site of the Day bar: concept, art direction, type, motion, WebGL, transitions, SEO, accessibility, visual QA. |
| `high-front-end` | Cinematic, scroll-driven 3D: WebGL scenes, camera moves through space, Three.js/R3F with GSAP ScrollTrigger. |
| `jury` | Scores a live site like an Awwwards jury: screenshots every page type on desktop and throttled mobile, measures LCP/CLS/FPS/axe/keyboard, returns a ranked fix list. |
| `ui-ux-pro-max` | UI/UX design intelligence backed by a searchable pattern database. |
| `brand-guidelines` | Applies a brand's official colours, typography, and voice to any artifact. |
| `canvas-design` | Static visual art as PNG/PDF — posters, illustrations, print-grade design. |
| `theme-factory` | Applies or generates a theme (10 presets) across slides, docs, reports, and landing pages. |
| `artifacts-builder` | Builds large multi-component HTML artifacts with React, Tailwind, shadcn/ui, routing, and state. |
| `image-enhancer` | Upscales, sharpens, and cleans images — especially screenshots for docs and decks. |
| `brag-slim` | Turns a project directory or URL into a short launch video with music, motion, and share copy. |
| `slack-gif-creator` | Creates Slack-optimised animated GIFs and emoji animations within platform size limits. |

### Languages — 11

| Skill | What it does |
| --- | --- |
| `typescript-pro` | Advanced type systems: generics, conditional and mapped types, branded types, type guards, tRPC end-to-end safety. |
| `javascript-pro` | Modern ES2023+, async patterns, ESM, Node APIs, browser performance, Web Workers. |
| `python-pro` | Python 3.11+ with strict typing, async, dataclasses, dependency injection, pytest suites, mypy strict. |
| `golang-pro` | Concurrent Go: goroutines, channels, generics, gRPC/REST services, pprof profiling, table-driven tests. |
| `rust-engineer` | Ownership, lifetimes, trait hierarchies, tokio async, `Result`-based error handling, FFI, zero-cost abstractions. |
| `cpp-pro` | Modern C++20/23: concepts, ranges, coroutines, SIMD, memory management, CMake. |
| `csharp-developer` | C# and .NET 8+: ASP.NET Core APIs, EF Core, async with cancellation, CQRS via MediatR, Blazor. |
| `java-architect` | Enterprise Java: Spring Boot 3.x, WebFlux, JPA tuning, Spring Security with OAuth2/JWT. |
| `kotlin-specialist` | Coroutines, Flow, KMP, Compose UI, Ktor, sealed classes, type-safe DSLs. |
| `swift-expert` | SwiftUI, protocol-oriented design, async/await, actors, Combine, Vapor. |
| `php-pro` | PHP 8.3+, Laravel and Symfony, PHPStan level 9, Swoole async, PSR standards, typed DTOs. |

### Frameworks & platforms — 23

| Skill | What it does |
| --- | --- |
| `react-expert` | React 18/19: components, hooks, Server Components, Suspense, `useActionState`, performance. |
| `nextjs-developer` | Next.js 14+ App Router: RSC, server actions, route handlers, middleware, metadata, streaming, Vercel. |
| `vue-expert` | Vue 3 Composition API, Nuxt 3 SSR/SSG, Pinia, Quasar/Capacitor, PWA, Vite tuning. |
| `vue-expert-js` | The same Vue toolkit in JavaScript only, with JSDoc types instead of a TS compiler. |
| `angular-architect` | Angular 17+ standalone components, lazy routing, guards, NgRx, RxJS, bundle optimisation. |
| `react-native-expert` | React Native + Expo: navigation hierarchies, native modules, FlatList performance, SafeArea, keyboard. |
| `flutter-expert` | Flutter 3 and Dart: widgets, Riverpod/Bloc, GoRouter, platform channels, performance. |
| `nestjs-expert` | NestJS modules, controllers, services, DTOs, guards, interceptors, DI, Swagger, e2e tests. |
| `backend-mastery` | Backend architecture plus test discipline: correctness-first reasoning, rigorous tests, actually running them. |
| `fastapi-expert` | FastAPI and Pydantic v2: async endpoints, SQLAlchemy async, JWT, WebSockets, OpenAPI. |
| `django-expert` | Django and DRF: models and indexes, `select_related`/`prefetch_related`, serializers, viewsets, JWT. |
| `laravel-specialist` | Laravel 10+: Eloquent, Sanctum, Horizon queues, API resources, Livewire, Pest tests. |
| `rails-expert` | Rails 7+: Active Record tuning, Turbo Frames/Streams, Action Cable, Sidekiq, RSpec. |
| `spring-boot-engineer` | Spring Boot 3.x: REST controllers, Security 6, JPA, WebFlux, Spring Cloud. |
| `dotnet-core-expert` | .NET 8 minimal APIs, clean architecture, EF Core, MediatR CQRS, JWT, AOT compilation. |
| `fullstack-guardian` | End-to-end features with layered security: database → API → UI, auth, validation, encoding, parameterised queries. |
| `shopify-expert` | Liquid themes, `theme.json` sections, Shopify apps, Storefront API, checkout extensions, Functions. |
| `wordpress-pro` | Custom themes and plugins, Gutenberg blocks and patterns, WooCommerce, REST endpoints, hardening. |
| `salesforce-developer` | Apex, Lightning Web Components, SOQL tuning, triggers, batch jobs, platform events, DX and CI/CD. |
| `embedded-systems` | Firmware: STM32/ESP32, FreeRTOS, interrupt handlers, DMA, power optimisation, timing debug. |
| `game-developer` | Unity/Unreal: ECS, physics, netcode with lag compensation, shaders, 60+ FPS budgets. |
| `cli-developer` | CLI tools: argument parsing, subcommands, progress UI, shell completions, cross-platform terminals. |
| `websocket-engineer` | Real-time messaging with WebSockets or Socket.IO: bidirectional events, Redis-backed horizontal scaling, presence, rooms. |

### Data — 7

| Skill | What it does |
| --- | --- |
| `postgres-pro` | EXPLAIN analysis, JSONB, extensions, replication, VACUUM tuning, performance monitoring. |
| `sql-pro` | Complex queries, window functions, CTEs, indexing strategy, plan analysis, dialect migration. |
| `database-optimizer` | Slow queries across Postgres and MySQL: rewrites, index design, config tuning, partitioning, lock contention. |
| `pandas-pro` | DataFrame manipulation: joins, pivots, resampling, NaN handling, groupby, performance on large frames. |
| `spark-engineer` | DataFrame and RDD jobs, shuffle tuning, partitioning, executor memory, structured streaming, Parquet. |
| `ml-pipeline` | MLflow/W&B tracking, Kubeflow/Airflow DAGs, Feast feature stores, model registry, retraining automation. |
| `rag-architect` | Chunking, embeddings, vector stores, hybrid search, reranking, retrieval evaluation. |

### Infrastructure & operations — 7

| Skill | What it does |
| --- | --- |
| `devops-engineer` | Dockerfiles, CI/CD pipelines, Kubernetes manifests, Terraform/Pulumi, GitOps, incident runbooks. |
| `cloud-architect` | AWS/Azure/GCP architecture, migration plans, cost optimisation, disaster recovery, landing zones. |
| `terraform-engineer` | Reusable modules, state migration and imports, provider config, multi-environment workflows, IaC testing. |
| `kubernetes-specialist` | Deployments, RBAC, NetworkPolicies, pod security, storage, Helm, GitOps, multi-cluster management. |
| `monitoring-expert` | Prometheus/Grafana dashboards, alert rules, structured logging, tracing, k6/Artillery load tests, profiling. |
| `sre-engineer` | SLIs and SLOs, error budgets, incident response, capacity models, toil reduction. |
| `chaos-engineer` | Chaos experiments, failure injection, game days, blast-radius control, rollback runbooks. |

### Testing & QA — 5

| Skill | What it does |
| --- | --- |
| `test-master` | Test files, mocking strategies, coverage analysis, test architecture, performance and security test plans. |
| `playwright-expert` | E2E suites: page objects, fixtures, reporters, CI integration, API mocking, visual regression. |
| `webapp-testing` | Drives a local web app with Playwright to verify behaviour, capture screenshots, and read console logs. |
| `debugging-wizard` | Hypothesis-driven debugging: parses errors, traces stack frames, correlates logs, isolates the root cause. |
| `code-security-auditor` | Static pre-execution audit of an untrusted codebase: malicious behaviour, supply chain, vulnerabilities. |

### Review & security — 4

| Skill | What it does |
| --- | --- |
| `code-reviewer` | Structured review of a diff: bugs, injection and XSS, smells, N+1 queries, naming, architecture, prioritised report. |
| `staff-engineer-review` | Staff-plus review: implementation against plan, architectural decisions, quality, actionable feedback. |
| `security-reviewer` | Security audit with severity ratings and remediation guidance; SAST, dependency, and cloud checks. |
| `secure-code-guardian` | Implements auth, input validation, hashing, parameterised queries, CORS/CSP, JWTs, secure sessions. |

### AI, prompts & agents — 5

| Skill | What it does |
| --- | --- |
| `prompt-engineer` | Writes, refactors, and evaluates prompts — templates, output schemas, rubrics, eval suites. |
| `mcp-builder` | Guide for building high-quality MCP servers in Python (FastMCP) or TypeScript. |
| `mcp-developer` | Implements MCP tool handlers, resource providers, transports, and Zod/Pydantic schema validation. |
| `fine-tuning-expert` | LoRA/QLoRA, JSONL dataset prep, hyperparameters, PEFT, DPO/RLHF, quantised deployment. |
| `skill-creator` | Creates and updates skill files that extend agent capabilities. |

### Documents & content — 8

| Skill | What it does |
| --- | --- |
| `docx` | Create, edit, and analyse Word documents, including tracked changes and comments. |
| `pptx` | Create and edit PowerPoint decks: layouts, speaker notes, comments. |
| `pdf` | Extract text and tables, create, merge, split, and fill PDF forms. |
| `xlsx` | Create and edit spreadsheets with formulas, formatting, analysis, and charts. |
| `readme-generator` | Detailed README generation: features, stack, architecture, API docs, setup. |
| `code-documenter` | Docstrings, JSDoc, OpenAPI specs, documentation portals, user guides. |
| `content-research-writer` | Research-backed writing: citations, hooks, outlines, per-section feedback. |
| `domain-name-brainstormer` | Generates domain ideas and checks availability across TLDs. |

### Workflow & team — 6

| Skill | What it does |
| --- | --- |
| `internal-comms` | Status reports, leadership updates, newsletters, FAQs, and incident comms in your company's format. |
| `meeting-insights-analyzer` | Analyses transcripts for behavioural patterns: filler words, dominance, missed listening. |
| `changelog-generator` | Turns commit history into customer-facing release notes. |
| `developer-growth-analysis` | Reviews your recent chat history for coding patterns and gaps, curates resources, sends a report. |
| `skill-share` | Creates skills and shares them to a Slack workspace. |
| `template-skill` | Starter template for writing a new skill. |

### Business & research — 4

| Skill | What it does |
| --- | --- |
| `competitive-ads-extractor` | Pulls and analyses competitor ads from ad libraries to find working messaging. |
| `lead-research-assistant` | Finds and qualifies leads, with an outreach strategy per segment. |
| `raffle-winner-picker` | Fair random winner selection from lists or spreadsheets. |
| `invoice-organizer` | Reads invoices and receipts, extracts fields, renames, and files by category. |

### Utilities — 4

| Skill | What it does |
| --- | --- |
| `file-organizer` | Organises files by context, finds duplicates, suggests a better structure. |
| `video-downloader` | Downloads video from YouTube and other platforms for offline use. |
| `atlassian-mcp` | Jira (JQL), Confluence (CQL), sprints and backlogs, via MCP. |
| `caveman` | Ultra-compressed response mode that cuts output tokens while keeping technical accuracy. |

---

## Documentation

The method behind the skills, in [`docs/`](docs/README.md):

| Document | Read it when |
| --- | --- |
| [Prompt engineering](docs/01-prompt-engineering.md) | You need a better first answer. CRISP, meta-prompting, prompt chains. |
| [Context engineering](docs/02-context-engineering.md) | You're building a system, not a chat. Five layers, system prompt template. |
| [Skill files](docs/03-skill-files.md) | You're writing or fixing a skill. Structure, principles, hygiene. |
| [Product documents](docs/04-product-documents.md) | You need to define what to build. MVP → PRD → SRS → SDS. |
| [Prompt anti-patterns](docs/05-prompt-anti-patterns.md) | Output keeps coming back generic. Nine mistakes and their fixes. |
| [Development workflow](docs/06-development-workflow.md) | You want the full 19-phase lifecycle with a skill mapped to each phase. |
| [Harness rules](docs/07-harness-rules.md) | You're onboarding an agent or a person. The operating rules. |
| [Architecture review](docs/08-architecture-review.md) | You're about to approve an architecture. The questions people skip. |
| [Interrogating a spec](docs/09-spec-interrogation.md) | A feature claims to compute a score or prediction. Interrogate it before building. |

---

## Design resources

### The 29 design links

From the design team's shortcut folder (`Desktop/Design/ui-ux components/`, URLs below and in [`Desktop/Design/ui-ux components/README.md`](../../../Design/ui-ux%20components/README.md)).

**Component libraries — code you copy into a project**

| Site | URL | What it gives you | Free / Paid |
| --- | --- | --- | --- |
| 21st | [21st.dev](https://21st.dev) | Community React + Tailwind components and templates, built to be pasted into a real project | Freemium |
| Animmaster Lib | [animmasterlib.dev](https://animmasterlib.dev) | 300 pro-grade animated components reverse-engineered from top-tier sites: scroll, mouse, page transitions, WebGL | Paid |
| Lightswind UI | [lightswind.com](https://lightswind.com) | 300+ animated React components, 550+ blocks; open-source core (MIT), pro assets paid | Freemium |
| Origin UI | [originui.com](https://originui.com) | Clean, unopinionated shadcn-compatible components — the neutral base layer | Free |
| React Bits | [reactbits.dev](https://reactbits.dev) | Free animated React components: text effects, backgrounds, carousels | Free |
| Skiper UI | [skiper-ui.com](https://skiper-ui.com) | 100+ uncommon shadcn/ui components as one-file installs, no extra packages | Freemium |
| Vengeance UI | [vengeanceui.com](https://vengeanceui.com) | Animated, motion-led components with a darker, designed default aesthetic | Freemium |
| UI8 | [ui8.net](https://ui8.net) | UI kits, design systems, Figma files, 3D/illustration assets | Paid |
| Vanta.js | [vantajs.com](https://vantajs.com) | Drop-in animated 3D backgrounds (fog, waves, net, birds) via WebGL | Free |
| Lenis | [lenis.darkroom.engineering](https://lenis.darkroom.engineering) | Smooth-scroll library — the standard behind most "expensive-feeling" scroll | Free |
| GSAP | [gsap.com](https://gsap.com) | The animation platform: ScrollTrigger, SplitText, timelines | Free |
| Remotion | [remotion.dev](https://remotion.dev) | Frame-accurate video from React code, renders locally or in CI | Freemium |

**Inspiration galleries — what to show the client**

| Site | URL | What it gives you | Free / Paid |
| --- | --- | --- | --- |
| Awwwards | [awwwards.com](https://awwwards.com) | Jury-scored Site of the Day bar for ambition | Free |
| Dribbble | [dribbble.com](https://dribbble.com) | Shot-level UI, brand and colour ideas; worst case in feasibility | Freemium |
| Landbook | [land-book.com](https://land-book.com) | Landing-page gallery filtered by industry, style, and section | Free |
| SEESAW | [seesaw.website](https://seesaw.website) | Hand-picked, daily, category-filtered (AI, fintech, dev tools) | Free |
| Framer Community | [framer.com/community](https://framer.com/community) | Live sites, templates, and remixable projects you can inspect | Free |
| Webflow — animation sites | [webflow.com/animation-websites](https://webflow.com/animation-websites) | Curated animation-led Webflow sites; reference when the brief says "make it move" | Free |

**AI design and generation — prompt in, page out**

| Site | URL | What it gives you | Free / Paid |
| --- | --- | --- | --- |
| Lovable | [lovable.dev](https://lovable.dev) | Full-stack apps from conversation, live preview, GitHub sync | Freemium |
| Stitch | [stitch.withgoogle.com](https://stitch.withgoogle.com) | Google's UI generator: describe a screen, get editable UI exportable to code/Figma | Free |
| Superdesign | [superdesign.dev](https://superdesign.dev) | Design-agent IDE generating multiple UI variants side by side | Freemium |
| Design Extractor | [design-extractor.com](https://design-extractor.com) | Paste a URL, get a `DESIGN.md` — YAML tokens + written rationale for your coding agent | Freemium |
| DesignPrompts | [designprompts.dev](https://designprompts.dev) | Pick a page template + one of ~48 "looks", preview, copy the prompt — free, no sign-up | Free |
| MotionSites | [motionsites.ai](https://motionsites.ai) | Premium hero-section prompts by industry (SaaS, real estate, wellness, 3D) | Freemium |
| MotionVideo | [motionvideo.xyz](https://motionvideo.xyz) | Agent skills for motion: scene starters + motion tokens that render showreels with Remotion | Free |
| Open Design | `%LOCALAPPDATA%\Programs\Open Design\Open Design.exe` (local app, no URL) | Desktop design application installed on this machine | Local install |

**Prompt and code references — raw material**

| Site | URL | What it gives you | Free / Paid |
| --- | --- | --- | --- |
| God of Prompt | [godofprompt.ai](https://godofprompt.ai) | 30,000+ prompts across marketing, design, dev, business | Freemium |
| liquidGL | [github.com/naughtyduk/liquidGL](https://github.com/naughtyduk/liquidGL) | Open-source WebGPU/WebGL liquid-glass effect library — the technique behind most liquid-chrome/glass heroes | Free |
| awesome-opus5-5-videos | [github.com/yihui-dev/awesome-opus5-5-videos](https://github.com/yihui-dev/awesome-opus5-5-videos) | Viral coding-agent videos with the prompts that produced them | Free |

Two reference files for the design side, kept outside this repo so they stay visible on the desktop:

| File | Contents |
| --- | --- |
| [`Desktop/Design/frontend-design-styles.md`](../../../Design/frontend-design-styles.md) | 44 frontend visual styles in 6 families. Each with a definition, where it earns its keep, a motion rating, and which skill to load. Includes a quick-pick index and pairing rules. |
| [`Desktop/Design/ui-ux components/README.md`](../../../Design/ui-ux%20components/README.md) | The same 29 links, grouped, with notes on licences and shortcut behaviour. |

The style taxonomy is sourced from `frontend_design_styles.docx` / `frontend_design_styles.md`, kept here for provenance.

### The six style families

| Family | Count | Idea |
| --- | --- | --- |
| **Material & surface** | 7 | How the design pretends to be physical |
| **Digital-native & retro-tech** | 8 | The screen talking about itself |
| **Historical & art movements** | 12 | Borrowed authority |
| **Layout & information design** | 7 | Structure *is* the style |
| **Expressive & anti-convention** | 7 | The design resists the grid |
| **Restrained & functional** | 2 | When the content is the design |

Plus 1 unclassified entry (`YSK aesthetic`) that isn't a real style family.

---

## Conventions

**Skills are pushed as-is.** This repo is a vendored copy, not a fork. Where a skill has a flaw, the fix belongs upstream, not here — otherwise every clone diverges. Run `git pull` to update.

**`SKILL.md` is the entry point.** Every skill has one. Everything in `references/` is loaded on demand and never sits in the always-on context. That's intentional: 107 skills × full reference material would consume the entire context window before your first message.

**Descriptions are routing signals.** The `description:` field decides whether a skill loads. It's written as *when to use this*, not *what this is*.

**Docs over code.** If a decision isn't written down, it didn't happen. The `docs/` directory is canonical; the `.docx` files in `All-about-prompts/` are provenance.

**Markdown only.** No build step, no dependencies, no CI. A skill is a file.

---

## Known quirks

Honest list, so nobody spends an afternoon on them.

| Quirk | Impact | Workaround |
| --- | --- | --- |
| **Four frontend skills claim "build me a website"** — `elite-frontend-dev`, `high-front-end`, `awwwards`, `ui-ux-pro-max` | The model picks one arbitrarily | Name the skill explicitly: *"use `high-front-end` only"* |
| **`spec-driven-dev` references two files that don't exist** (`references/examples.md`, `references/package-template.md`) | Broken reference, not a broken skill | The skill still runs; see `docs/04-product-documents.md` for the 9-file structure |
| **Folder names differ from frontmatter `name:`** — `backend-mastery` → `backend-testing-mastery`, `high-front-end` → `cinematic-3d-web` | Cosmetic only | The skill ID comes from the path, so both names work |
| **`ui-ux-pro-max` hardcodes `~/.opencode/skills/...` paths** and asserts React Native is the stack | Wrong paths on this layout | Use it for the pattern database, not the file paths |
| **`skill-creator` contains placeholder text** | Low quality guidance | Prefer the method in [`docs/03-skill-files.md`](docs/03-skill-files.md) |
| **`spec-driven-dev/SKILL-coding-assist.md` has no frontmatter** | Undiscoverable by the model | Read it directly; it's a strong 700-line engineering-rules doc |
| **`template-skill` description is literally placeholder text** | It is a template, by design | Ignore it in normal use |

Fixing any of these means forking the skills, which contradicts "push as-is". Left as documentation instead.

---

## Maintenance

```bash
cd ~/.config/harness   # wherever you cloned it
git pull
```

Restart the harness — skills are re-read at startup. Because every tool points at the same clone, one pull updates all of them.

To add a skill: create `skills/<new-skill>/SKILL.md` with `name`, `description`, and `version` frontmatter, then follow [`docs/03-skill-files.md`](docs/03-skill-files.md). Keep the description trigger-first, and keep the body short — every rule dilutes the others.

---

## Licence and provenance

Everything third-party is inventoried in [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md): 9 skills on Apache License 2.0, 4 proprietary Anthropic document skills (all rights reserved), 66 Jeffallan skills with no license recorded (all rights reserved, private internal use), 28 uncredited skills (all rights reserved), and 26 SIL-OFL fonts bundled in `canvas-design`.

Short version:

- Licensed files in the repo (`LICENSE.txt`, `*-OFL.txt`) are part of the goods — keep them, don't edit them.
- Execution of this repo inside your organisation is fine.
- **Do not redistribute this repo publicly as-is.** The unlicensed majority does not grant redistribution rights.
- Fix licence hygiene up front if you want this public: see Section E of the notices file.

The method documents in `docs/`, `AGENTS.md`, and the `All-about-prompts/` conversions were written for this harness.
