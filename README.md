# Spec-Driven Dev Harness 
##( Dev note : Ask your agent to configure it for you if you dont wanna do it manually)

A portable AI coding harness: **79 skills**, a written method, and a design reference pack — one clone, and every harness on the machine inherits the same standard.

Works with **OpenCode, Claude Code, Codex CLI, Cursor, Cline, and anything that reads the open Agent Skills format or `AGENTS.md`.**

The point is not the skills individually. It's that a new machine, a new teammate, or a new model starts at the same baseline as a fully-tuned one.

---

## Why it's portable

A skill is a folder: `<skill-id>/SKILL.md` with `name` and `description` frontmatter, plus optional `references/`, `scripts/`, and `assets/`. That is the **open Agent Skills standard** — not any one tool's convention. Every major agent reads the same folder shape; only the directory it scans differs.

So the skills are never rewritten per tool. The repo stays one source of truth, and each harness points at it.

---

## The problem this solves

Every AI coding setup decays the same way. Instructions get written once, copied into three projects, and two copies go stale. A new machine starts from zero. A new teammate starts from zero. The knowledge lives in someone's chat history.

This repo is the opposite: **one clone, wired into every harness, and every session inherits the same standard.** Skills are plain markdown, so they're readable, diffable, and portable. Nothing is compiled, nothing is hosted.

---

## What you get

| | |
| --- | --- |
| **Skills** | 79, covering planning, engineering, design, data, infra, testing, review, documents, and business |
| **Method docs** | 9 structured guides — prompt engineering through to the 19-phase development lifecycle |
| **Design reference** | A 44-style frontend taxonomy and a 29-link component/inspiration index |
| **Format** | Markdown only. No build step, no runtime, no vendor lock-in |
| **Harnesses** | OpenCode, Claude Code, Codex CLI, Cursor, Cline, and `AGENTS.md`-native tools |
| **Attribution** | Full third-party inventory in [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md) |

---

## Install

### 1. Clone once

```bash
git clone https://github.com/ahmedume/Spec-driven-development.git ~/.config/harness
```

Windows (PowerShell):

```powershell
git clone https://github.com/ahmedume/Spec-driven-development.git "$env:USERPROFILE\.config\harness"
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

`AGENTS.md` is the cross-agent instruction convention, read by Codex, Cursor, Copilot, Cline, OpenCode, and most others. If a tool has no native skill support, this is the whole install — commit [`AGENTS.md`](AGENTS.md) to your project or home directory.

### 4. Verify

Restart the harness, then ask for a skill by name:

```
> use the the-fool skill on this plan
```

If it loads, the wiring is correct.

---

### Why point at the clone instead of copying

Copying 79 directories into each harness's folder looks simpler and isn't:

- **Updates** mean re-copying every file, per harness, forever.
- **Copies drift**, and you can no longer tell which version is live.
- **Windows symlinks** for directories need elevation; junctions don't, which is why the commands above use `mklink /J`.

One clone plus pointers means `git pull` is the upgrade path for every tool on the machine.

---

## Layout

```
spec-driv-dev/
├── README.md                  ← you are here
├── AGENTS.md                  ← harness default rules (copy into any project)
├── LICENSE                    ← MIT, for the harness's own content
├── THIRD_PARTY_NOTICES.md     ← third-party attribution for everything else
├── docs/                      ← the written method (9 guides)
│   ├── 01-prompt-engineering.md
│   ├── 02-context-engineering.md
│   ├── 03-skill-files.md
│   ├── 04-product-documents.md
│   ├── 05-prompt-anti-patterns.md
│   ├── 06-development-workflow.md
│   ├── 07-harness-rules.md
│   ├── 08-architecture-review.md
│   ├── 09-spec-interrogation.md
│   ├── frontend-design-styles.md  ← raw styles A–Z listing
│   └── source-material/          ← raw sources the guides were structured from
└── skills/                    ← 79 skill directories
    └── <skill-id>/
        ├── SKILL.md           ← the instructions
        └── references/        ← loaded on demand, not always-on
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
| **Build the UI** | bring your own frontend skill (see below) or upstream marketplace | This repo ships no third-party frontier-site skill — see [Notices](THIRD_PARTY_NOTICES.md) |
| **Build the frontend** | upstream marketplace for award-level builds | e.g. `npx skills add <owner/repo> --skill <name>` — see [Notices](THIRD_PARTY_NOTICES.md) for what is deliberately not vendored here |
| **Build the API** | `backend-mastery` · `fastapi-expert` / `nestjs-expert` / … | Correctness-first, tested |
| **Build the data layer** | `postgres-pro` · `sql-pro` | Schema, indexes, query plans |
| **Test it** | `test-master` · `playwright-expert` | Unit through E2E |
| **Review it** | `code-reviewer` | Prioritised findings, not a list of nits |
| **Secure it** | `security-reviewer` · `secure-code-guardian` | Severity-rated audit; OWASP prevention |
| **Ship it** | `devops-engineer` | CI/CD, containers, deploy |
| **Document it** | `code-documenter` | Docs and docstrings |

For upstream frontend/design skills that are not vendored here, install from the marketplace: `npx skills add <owner/repo> --skill <name>`, or from [skills.sh](https://skills.sh).

### The quality gate

Per feature, no exceptions:

```
implemented → types pass → lint passes → tests pass → manual test → commit
```

---

## Skill catalog

All 79 skills, grouped by what you'd reach for them to do.

### Spec, planning & architecture — 9

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

### Frontend & design — 6

| Skill | What it does |
| --- | --- |
| `ui-ux-pro-max` | UI/UX design intelligence backed by a searchable pattern database (MIT — Next Level Builder). |
| `artifacts-builder` | Builds large multi-component HTML artifacts with React, Tailwind, shadcn/ui, routing, and state. |
| `canvas-design` | Static visual art as PNG/PDF — posters, illustrations, print-grade design. |
| `brand-guidelines` | Applies a brand's official colours, typography, and voice to any artifact. |
| `slack-gif-creator` | Creates Slack-optimised animated GIFs and emoji animations within platform size limits. |
| `theme-factory` | Applies or generates a theme (10 presets) across slides, docs, reports, and landing pages. |

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
| `react-expert` | React 18/19: components, hooks, Server Components, Suspense, performance. |
| `nextjs-developer` | Next.js 14+ App Router: RSC, server actions, route handlers, middleware, metadata, streaming, Vercel. |
| `vue-expert` | Vue 3 Composition API, Nuxt 3 SSR/SSG, Pinia, Quasar/Capacitor, PWA, Vite tuning. |
| `vue-expert-js` | The same Vue toolkit in JavaScript only, with JSDoc types instead of a TS compiler. |
| `angular-architect` | Angular 17+ standalone components, lazy routing, guards, NgRx, RxJS, bundle optimisation. |
| `react-native-expert` | React Native + Expo: navigation hierarchies, native modules, FlatList performance, SafeArea. |
| `flutter-expert` | Flutter 3 and Dart: widgets, Riverpod/Bloc, GoRouter, platform channels, performance. |
| `nestjs-expert` | NestJS modules, controllers, services, DTOs, guards, interceptors, DI, Swagger, e2e tests. |
| `backend-mastery` | Backend architecture plus test discipline: correctness-first reasoning, rigorous tests, actually run them. |
| `fastapi-expert` | FastAPI and Pydantic v2: async endpoints, SQLAlchemy async, JWT, WebSockets, OpenAPI. |
| `django-expert` | Django and DRF: models and indexes, `select_related`, serializers, viewsets, JWT. |
| `laravel-specialist` | Laravel 10+: Eloquent, Sanctum, Horizon queues, API resources, Livewire, Pest tests. |
| `rails-expert` | Rails 7+: Active Record tuning, Turbo Frames/Streams, Action Cable, Sidekiq, RSpec. |
| `spring-boot-engineer` | Spring Boot 3.x: REST controllers, Security 6, JPA, WebFlux, Spring Cloud. |
| `dotnet-core-expert` | .NET 8 minimal APIs, clean architecture, EF Core, MediatR CQRS, JWT, AOT compilation. |
| `fullstack-guardian` | End-to-end features with layered security: database → API → UI, auth, validation, encoding. |
| `shopify-expert` | Liquid themes, Storefront API, Shopify apps, checkout extensions. |
| `wordpress-pro` | Custom themes and plugins, Gutenberg blocks, WooCommerce, REST endpoints, hardening. |
| `salesforce-developer` | Apex, Lightning Web Components, SOQL tuning, triggers, batch jobs, DevOps. |
| `embedded-systems` | Firmware: STM32/ESP32, FreeRTOS, interrupt handlers, DMA, power optimisation. |
| `game-developer` | Unity/Unreal: ECS, physics, netcode with lag compensation, shaders, 60+ FPS budgets. |
| `cli-developer` | CLI tools: argument parsing, subcommands, progress UI, shell completions. |
| `websocket-engineer` | Bidirectional messaging with WebSockets and Socket.IO, Redis scaling, presence, rooms. |

### Data — 7

| Skill | What it does |
| --- | --- |
| `postgres-pro` | EXPLAIN analysis, JSONB, extensions, replication, VACUUM tuning, performance monitoring. |
| `sql-pro` | Complex queries, window functions, CTEs, indexing strategy, dialect migration. |
| `database-optimizer` | Slow queries across Postgres and MySQL: rewrites, index design, config tuning. |
| `pandas-pro` | DataFrame manipulation: joins, pivots, resampling, NaN handling, groupby. |
| `spark-engineer` | DataFrame and RDD jobs, shuffle tuning, partitioning, structured streaming, Parquet. |
| `ml-pipeline` | MLflow/W&B tracking, Kubeflow/Airflow DAGs, Feast feature stores, model registry. |
| `rag-architect` | Chunking, embeddings, vector stores, hybrid search, reranking, retrieval evaluation. |

### Infrastructure & operations — 7

| Skill | What it does |
| --- | --- |
| `devops-engineer` | Dockerfiles, CI/CD pipelines, Kubernetes, Terraform/Pulumi, GitOps, incident runbooks. |
| `cloud-architect` | AWS/Azure/GCP architecture, migration plans, cost optimisation, disaster recovery. |
| `terraform-engineer` | Reusable modules, state migration and imports, provider config, IaC testing. |
| `kubernetes-specialist` | Deployments, RBAC, NetworkPolicies, Helm, GitOps, multi-cluster. |
| `monitoring-expert` | Prometheus/Grafana dashboards, alert rules, structured logging, tracing, load tests. |
| `sre-engineer` | SLIs and SLOs, error budgets, incident response, capacity models, toil reduction. |
| `chaos-engineer` | Chaos experiments, failure injection, game days, blast-radius control, rollback runbooks. |

### Testing & QA — 4

| Skill | What it does |
| --- | --- |
| `test-master` | Test files, mocking strategies, coverage analysis, test architecture plans. |
| `playwright-expert` | E2E suites: page objects, fixtures, reporters, CI integration, visual regression. |
| `webapp-testing` | Drives a local web app with Playwright to verify behaviour and catch console errors. |
| `debugging-wizard` | Hypothesis-driven debugging: errors, stack traces, logs, root-cause isolation. |

### Review & security — 3

| Skill | What it does |
| --- | --- |
| `code-reviewer` | Structured review of a diff: bugs, injection, smells, N+1 queries, prioritised report. |
| `security-reviewer` | Security audit with severity ratings and remediation guidance; SAST, dependencies, cloud. |
| `secure-code-guardian` | Implements auth, input validation, hashing, parameterised queries, CORS/CSP, JWTs. |

### AI, prompts & agents — 6

| Skill | What it does |
| --- | --- |
| `prompt-engineer` | Writes, refactors, evaluates prompts — templates, output schemas, rubrics, eval suites. |
| `mcp-builder` | Guide for building high-quality MCP servers in Python (FastMCP) or TypeScript. |
| `mcp-developer` | Implements MCP tool handlers, resource providers, transports, schema validation. |
| `fine-tuning-expert` | LoRA/QLoRA, JSONL dataset prep, hyperparameters, DPO/RLHF, quantised deployment. |
| `skill-creator` | Creates and updates skill files that extend agent capabilities. |
| `template-skill` | Starter template for writing a new skill. |

### Documents & content — 1

| Skill | What it does |
| --- | --- |
| `code-documenter` | Docstrings, JSDoc, OpenAPI specs, documentation portals, user guides. |

### Workflow & team — 1

| Skill | What it does |
| --- | --- |
| `internal-comms` | Status reports, leadership updates, newsletters, FAQs, incident comms in company format. |

### Utilities — 1

| Skill | What it does |
| --- | --- |
| `atlassian-mcp` | Jira (JQL), Confluence (CQL), sprints and backlogs, via MCP. |

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
| [Interrogating a spec](docs/09-spec-interrogation.md) | A feature claims to compute a score or prediction. Interrogate it first. |
| [Instructions for the AI](docs/source-material/instructions.md) | You want the paste-as-is prompt that boots an agent into this workflow. |
| [Questions after the build](docs/source-material/questions.md) | The agent says "done". Get the concept of what it actually built. |

---

## Design resources

The design material ships in-repo — no local folders, no machine-specific paths:

| File | Contents |
| --- | --- |
| [`docs/frontend-design-styles.md`](docs/frontend-design-styles.md) | All 44 frontend visual styles, A–Z listing and families |

The URLs below are public design resources anyone can open; licences and ownership belong to their owners.

### The 29 design links

**Component libraries — code you copy into a project**

| Site | URL | What it gives you | Free / Paid |
| --- | --- | --- | --- |
| 21st | [21st.dev](https://21st.dev) | Community React + Tailwind components and templates | Freemium |
| Animmaster Lib | [animmasterlib.dev](https://animmasterlib.dev) | 300 pro-grade animated components: scroll, mouse, page transitions, WebGL | Paid |
| Lightswind UI | [lightswind.com](https://lightswind.com) | 300+ animated React components, 550+ blocks; MIT core, paid pro assets | Freemium |
| Origin UI | [originui.com](https://originui.com) | Clean, unopinionated shadcn-compatible components | Free |
| React Bits | [reactbits.dev](https://reactbits.dev) | Free animated React components: text effects, backgrounds, carousels | Free |
| Skiper UI | [skiper-ui.com](https://skiper-ui.com) | 100+ uncommon shadcn/ui components as one-file installs | Freemium |
| Vengeance UI | [vengeanceui.com](https://vengeanceui.com) | Animated, motion-led components with a darker default | Freemium |
| UI8 | [ui8.net](https://ui8.net) | UI kits, design systems, Figma files, 3D/illustration assets | Paid |
| Vanta.js | [vantajs.com](https://vantajs.com) | Drop-in animated 3D backgrounds via WebGL | Free |
| Lenis | [lenis.darkroom.engineering](https://lenis.darkroom.engineering) | Smooth-scroll library for expensive-feeling scroll | Free |
| GSAP | [gsap.com](https://gsap.com) | The animation platform: ScrollTrigger, SplitText, timelines | Free |
| Remotion | [remotion.dev](https://remotion.dev) | Frame-accurate video from React code | Freemium |

**Inspiration galleries**

| Site | URL | What it gives you | Free / Paid |
| --- | --- | --- | --- |
| Awwwards | [awwwards.com](https://awwwards.com) | Jury-scored Site of the Day bar for ambition | Free |
| Dribbble | [dribbble.com](https://dribbble.com) | Shot-level UI, brand and colour ideas | Freemium |
| Landbook | [land-book.com](https://land-book.com) | Landing-page gallery filtered by industry, style, section | Free |
| SEESAW | [seesaw.website](https://seesaw.website) | Hand-picked, daily, category-filtered | Free |
| Framer Community | [framer.com/community](https://framer.com/community) | Live sites, templates, remixable projects | Free |
| Webflow — animation sites | [webflow.com/animation-websites](https://webflow.com/animation-websites) | Curated animation-led sites; reference when the brief is "make it move" | Free |

**AI design and generation**

| Site | URL | What it gives you | Free / Paid |
| --- | --- | --- | --- |
| Lovable | [lovable.dev](https://lovable.dev) | Full-stack apps from conversation, live preview, GitHub sync | Freemium |
| Stitch | [stitch.withgoogle.com](https://stitch.withgoogle.com) | Google's UI generator: prompt → editable UI, export to code/Figma | Free |
| Superdesign | [superdesign.dev](https://superdesign.dev) | Design-agent IDE generating multiple UI variants side by side | Freemium |
| Design Extractor | [design-extractor.com](https://design-extractor.com) | Paste a URL, get a `DESIGN.md` — YAML tokens + rationale | Freemium |
| DesignPrompts | [designprompts.dev](https://designprompts.dev) | Pick a page template + one of ~48 styles, preview, copy prompt | Free |
| MotionSites | [motionsites.ai](https://motionsites.ai) | Ready-to-use hero prompts by industry | Freemium |
| MotionVideo | [motionvideo.xyz](https://motionvideo.xyz) | Agent skills for motion: scene starters and motion tokens rendering via Remotion | Free |
| Open Design | no public URL | Desktop design application; install separately from its official source | Local install |

**Prompt and code references**

| Site | URL | What it gives you | Free / Paid |
| --- | --- | --- | --- |
| God of Prompt | [godofprompt.ai](https://godofprompt.ai) | 30,000+ prompts across marketing, design, business | Freemium |
| liquidGL | [github.com/naughtyduk/liquidGL](https://github.com/naughtyduk/liquidGL) | Open-source WebGPU liquid-glass effect library | Free |
| awesome-opus5-5-videos | [github.com/yihui-dev/awesome-opus5-5-videos](https://github.com/yihui-dev/awesome-opus5-5-videos) | Viral coding-agent videos with the prompts that produced them | Free |

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

**Skills are vendored as-is.** This repo is not a fork. Where a skill has a flaw, the fix belongs upstream — otherwise every clone diverges. Run `git pull` to update.

**`SKILL.md` is the entry point.** Every skill has one. Everything in `references/` is loaded on demand and never sits in the always-on context. That's intentional: dozens of skills × full reference material would consume the context window before your first message.

**Descriptions are routing signals.** The `description:` field decides whether a skill loads. It's written as *when to use this*, not *what this is*.

**Docs over code.** If a decision isn't written down, it didn't happen. The `docs/` directory is canonical; `docs/source-material/` holds the sources.

**Markdown only.** No build step, no dependencies, no CI. A skill is a file.

---

## Known quirks

Honest list, so nobody spends an afternoon on them.

| Quirk | Impact | Workaround |
| --- | --- | --- |
| **`spec-driven-dev` references two files that may not exist in every checkout** (`references/examples.md`, `references/package-template.md` — this repo ships both at `skills/spec-driven-dev/`) | Broken reference, not a broken skill | The skill still runs; see [`docs/04-product-documents.md`](docs/04-product-documents.md) for the 9-file structure |
| **Folder names differ from frontmatter `name:`** — `backend-mastery` → `backend-testing-mastery` | Cosmetic only | The skill ID comes from the path, so both names work |
| **`ui-ux-pro-max` hardcodes `~/.opencode/skills/...` paths** and asserts React Native is the stack | Wrong paths outside that layout | Use it for the pattern database, not the file paths; override explicitly in your prompt |
| **`spec-driven-dev/SKILL-coding-assist.md` has no frontmatter** | Undiscoverable by the model | Read it directly; it's a strong engineering-rules doc |
| **`template-skill` description is placeholder text** | It is a template, by design | Ignore it in normal use — it exists to spawn new skills |
| **Third-party skills with no license were removed** (24 uncredited, 4 Anthropic ToS doc skills) | This repo is public; unlicensed skills may not be redistributed | Fetch them at install time from upstream — see [Notices](THIRD_PARTY_NOTICES.md) |

---

## Maintenance

```bash
cd ~/.config/harness   # wherever you cloned it
git pull
```

Restart the harness — skills are re-read at startup. Because every tool points at the same clone, one pull updates all of them.

To add a skill: create `skills/<new-skill>/SKILL.md` with `name`, `description`, and `version` frontmatter, then follow [`docs/03-skill-files.md`](docs/03-skill-files.md). Keep the description trigger-first, and keep the body short — every rule dilutes the others.

When you add or remove a skill, update [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md) in the same commit.

---

## Licence

This repository's own content (README, docs, AGENTS.md, the method, and the harness) is MIT-licensed — see [`LICENSE`](LICENSE).

Third-party skills and fonts retain their own licences, all inventoried in [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md):

- 9 Apache License 2.0 skills
- 67 MIT-licensed skills (66 from [Jeffallan's claude-skills](https://github.com/Jeffallan/claude-skills), 1 from [ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill))
- 26 SIL Open Font License 1.1 fonts bundled inside `canvas-design`
- 3 harness-authored skills (`spec-driven-dev`, `backend-mastery`, `template-skill`)
- **Removed:** 24 uncredited skills and 4 Anthropic ToS-governed document skills — not removed because they are bad, but because this repo is public and none of them granted redistribution rights. Install them from upstream instead: `npx skills add <owner/repo> --skill <name>` or via [skills.sh](https://skills.sh).

Keep `LICENSE`, the `*-OFL.txt` files, and `THIRD_PARTY_NOTICES.md` unchanged in forks.

---

## Contributing

Pull requests welcome for: docs corrections, new first-party skills (with sources noted in their frontmatter), and attribution fixes.

Not welcome: re-adding unlicensed third-party skills to this repository. Vendoring unlicensed content into a public harness is the one thing that is never fixable retroactively.
