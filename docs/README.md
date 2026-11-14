# Docs

The written method behind the harness. Read in order, or jump to what you need.

| # | Document | Answers |
| --- | --- | --- |
| 01 | [Prompt engineering](01-prompt-engineering.md) | How to write a prompt that produces expert output first time. CRISP, meta-prompting. |
| 02 | [Context engineering](02-context-engineering.md) | How to design the whole environment around the model. Five layers, system prompt template. |
| 03 | [Skill files](03-skill-files.md) | What `SKILL.md` is, how to write one, how to keep a library healthy. |
| 04 | [Product documents](04-product-documents.md) | MVP, PRD, SRS, SDS — what each is for and how they chain. |
| 05 | [Prompt anti-patterns](05-prompt-anti-patterns.md) | The nine mistakes that waste iterations, and the fix for each. |
| 06 | [Development workflow](06-development-workflow.md) | The full 19-phase lifecycle, with the skill to load at each phase. |
| 07 | [Harness rules](07-harness-rules.md) | The operating rules an agent follows in this repo. |
| 08 | [Architecture review](08-architecture-review.md) | How to interrogate a design before coding on it. The checklist and the questions people skip. |
| 09 | [Interrogating a spec](09-spec-interrogation.md) | How to review a feature that claims to produce a score, ranking, or prediction. |

---

## Suggested reading order

**If you're new to working with AI:** 01 → 05 → 03.

**If you're setting up a team:** 07 → 03 → 06.

**If you're writing a spec:** 04 → 09 → 08 → 06.

**If you're building:** 06 → then the skill in the matching row.

---

## Where the source material lives

[`source-material/`](source-material/README.md) holds the raw files these documents were structured from:

- `A guide for prompts.md` → 01, 02, 03, 04, 05
- `AI_Assisted_Production_Development_Workflow.md` → 06
- `Instructions.md` → 07
- `sdd-ask-model-imp.txt` → 08
- `questions.md` → 09

[`frontend-design-styles.md`](frontend-design-styles.md) is the styles A–Z listing used by the repo's design README.

The markdown in `source-material/` is canonical as source text; the structured guides are canonical for use. Edit those, not the sources.

---

Back to the [repo README](../README.md).
