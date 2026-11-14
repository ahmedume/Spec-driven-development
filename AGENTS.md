# AGENTS.md

This repository is a portable AI coding harness: **107 skills** plus a written method.

Copy this file to any project that should inherit the standard, or point your harness's skills directory at `skills/`. Setup is in [`README.md`](README.md#install).

---

## Always

- Read [`README.md`](README.md) before changing anything in this repo.
- Skills live in `skills/<skill-id>/SKILL.md`, with optional `references/`, `scripts/`, and `assets/`.
- Before starting a task, check whether a skill's `description` matches it. If one does, read that `SKILL.md` first and follow it.
- The written method is in [`docs/`](docs/README.md). Start there, not in the skills.
- Agent behaviour rules: [`docs/07-harness-rules.md`](docs/07-harness-rules.md).

## When building a product with this harness

Follow [`docs/06-development-workflow.md`](docs/06-development-workflow.md). The order is fixed:

```
questions → spec package → architecture review → build in phases → review and ship
```

The spec is the product. Never start coding before the spec exists and the user has approved it.

## Never

- **Do not edit files under `skills/` to fix them.** They are vendored as-is. Known defects are documented in [`README.md`](README.md#known-quirks); fixes belong upstream.
- **Do not hardcode secrets or personal information** — no API keys, tokens, emails, or profile URLs in source. Use `.env`, and commit `.env.example`.
- **Do not add a skill whose `description` duplicates another.** Overlapping descriptions make the model pick arbitrarily.

## Conventions

- Markdown only. No build step, no runtime.
- Keep `SKILL.md` short. References live beside it in `references/`, loaded on demand.
- Write the `description` as *when to use this*, not *what this is*. It's the routing signal.
- Docs over code: if a decision isn't written down, it didn't happen.
- Third-party attribution lives in [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md). If you add, remove, or update a skill, update that file in the same commit.
