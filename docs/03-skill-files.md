# Skill Files (`SKILL.md`)

The bridge between a model's general intelligence and your specific operational context.

---

## What a skill file is

A `SKILL.md` is a structured markdown document encoding domain knowledge, constraints, best practices, and procedural instructions for one task or technology area. It is injected into the context window before the model performs work in that domain.

Think of it as a training manual **not** for general knowledge — the model already has that — but for *your* environment, *your* constraints, *your* stack, and the lessons your team learned the hard way.

**The analogy:** it's the internal wiki page your most senior developer wrote before going on holiday.

## Why it exists

Models are trained on general knowledge. They don't know:

- Your company's coding conventions
- The quirks of your specific stack version
- The business rules that govern your domain
- The mistakes already made and not to be repeated
- The output formats your toolchain expects

A skill file closes that gap, once, for everyone.

---

## Writing principles

| Principle | What it means |
| --- | --- |
| **Write for an expert reader** | The model knows the fundamentals. Focus on your specifics. |
| **Be prescriptive, not descriptive** | "Always do X", not "X is generally considered good". |
| **Include negative rules** | "NEVER do Y" matters as much as "Always do X". |
| **Use concrete examples** | Abstract rules are applied inconsistently. |
| **Organise with headers** | The model must be able to find the relevant section fast. |
| **Keep it DRY** | Don't restate what the model already knows from training. |
| **Version it** | Add a version header so you know which copy is live. |

### The two failure modes

- **Too vague** — reads like a blog post. Produces blog-post output.
- **Too long** — 2,000 lines of context that push out the actual task. Every rule dilutes the others.

The target is the shortest document that changes behaviour.

---

## Structure template

```markdown
---
name: [skill-name]
version: 1.0.0
description: [one line: what this skill covers and when to use it]
---

# [Skill Name] Quick Reference

## Overview
[2–3 sentences: what this skill handles and when to reach for it.]

## When to Use This Skill
- [trigger condition]
- [trigger condition]
- Do NOT use for: [anti-patterns]

## Setup & Prerequisites
[install commands, dependencies, environment setup]

## Core Patterns
### [Pattern 1 Name]
[explanation + code example]

### [Pattern 2 Name]
[explanation + code example]

## Critical Rules
- ALWAYS: [rule]
- NEVER: [rule]
- IMPORTANT: [high-priority consideration]

## Common Mistakes to Avoid
- [mistake]: [why it's wrong + the correct alternative]

## Output Format
[exactly how output should be structured]

## Examples
### Good Example
[correct output]

### Bad Example (Do Not Do This)
[incorrect output + why]
```

---

## What belongs in the frontmatter

| Field | Purpose | Notes |
| --- | --- | --- |
| `name` | Human-readable identifier | In OpenCode the skill ID comes from the **folder path**, not this field. Mismatches are cosmetic. |
| `description` | The routing signal | This is what the model reads to decide whether to load the skill. Write it as *when to use*, not *what it is*. |
| `version` | Change tracking | Cheap, and it stops you shipping a stale copy. |

### Description writing

The description is the most important line in the file, because it determines whether the skill ever loads.

**Weak:** `"Database optimization."`
**Strong:** `"Use when investigating slow queries, analysing execution plans, or tuning PostgreSQL/MySQL performance. Invoke for index design, query rewrites, lock contention."`

Lead with the trigger phrases a user would actually type.

---

## Skill hygiene

1. **One skill, one domain.** A skill that covers "backend" is a skill that fires on everything and helps with nothing.
2. **Skills are code.** Version them, review changes, and treat a silent edit as a production change.
3. **Retire aggressively.** A stale skill is worse than no skill — it confidently teaches the wrong thing.
4. **Don't duplicate.** If two skills both claim the same trigger, the model picks arbitrarily. Fix the descriptions.
5. **Fold in failures.** Every time a task goes wrong, ask *"which rule was missing?"* and add it. That's how a skill file gets good.

---

## Where the skills live

Skills live in `skills/<skill-id>/SKILL.md`. Every harness reads the same folder shape — the open Agent Skills standard — and only the directory it scans differs:

| Harness | Scans |
| --- | --- |
| OpenCode | any path in `opencode.jsonc`, plus `~/.config/opencode/skills` |
| Claude Code | `~/.claude/skills`, `.claude/skills` |
| Codex CLI | `~/.agents/skills`, `.agents/skills` |
| Cursor | `~/.cursor/skills`, `~/.agents/skills`, `.cursor/skills`, `.agents/skills` |

OpenCode is the only one that takes an explicit config path:

```jsonc
{
  "skills": ["~/.config/harness/skills"]
}
```

Everything else just needs the clone's `skills/` directory linked into one of the folders above. Full wiring instructions — including the Windows junction commands — are in the [repo README](../README.md#install).

Reference material that shouldn't be loaded into context lives beside it in `references/` and is read on demand. That keeps the always-on context small, which is the whole point.

See the [repo README](../README.md) for install and the full skill catalog.

---

*Structured from `a-guide-for-prompts.md` (Section 3).*
