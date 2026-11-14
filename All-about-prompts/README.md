# Source material

The raw originals. Unstructured, unedited, kept for provenance.

**The structured versions in [`../docs/`](../docs/README.md) are canonical.** Edit those, not these.

---

## Where each one went

| Raw file | Structured output | What it became |
| --- | --- | --- |
| `A guide for prompts.docx` (now also `A guide for prompts.md`) | [`../docs/01`](../docs/01-prompt-engineering.md) – [`05`](../docs/05-prompt-anti-patterns.md) | The core method: prompt engineering, context engineering, skill files, product documents, anti-patterns |
| `AI_Assisted_Production_Development_Workflow.docx` (now also `.md`) | [`../docs/06`](../docs/06-development-workflow.md) | The 19-phase lifecycle, with a skill mapped to each phase |
| `Instructions.docx` (now also `.md`) | [`../docs/07`](../docs/07-harness-rules.md) | The 19 operating rules, generalised; personal details removed |
| `questions.docx` (now also `.md`) | [`../docs/09`](../docs/09-spec-interrogation.md) | The seven questions for interrogating a score-producing spec |
| `sdd-ask-model-imp.txt` | [`../docs/08`](../docs/08-architecture-review.md) | The architecture-review checklist and the six questions people skip |
| `write skills and spec driv dev.rtf` | — | **Superseded.** An earlier draft of the same material. Nothing in it that isn't in `docs/`. |

Every `.docx` now has a matching auto-extracted `.md` beside it (tables and columns land as flat lines with `|` separators). The `.docx` files are kept for provenance.

The style taxonomy in `../frontend_design_styles.docx` is the source for `../../Design/frontend-design-styles.md`.

---

## Notes on the originals

- `Instructions.docx` originally listed the author's email, GitHub, and LinkedIn in its last rule. That text was removed; [`../docs/07`](../docs/07-harness-rules.md) keeps the rule and drops the contact details.
- `A guide for prompts.docx` carried a personal byline on its title page. Removed.
- `write skills and spec driv dev.rtf` carried a personal name in its Word metadata (`\operator`). Cleared.

Everything else in this folder is verbatim.

---

## If you want to regenerate a structured file

The mapping above is one-way. The `.docx` and `.rtf` files are inputs, not outputs — the markdown is the deliverable. If a source changes, update the matching `docs/` file by hand and note it in the commit message.
