# Source material

Auto-extracted markdown from the original unstructured documents. Unedited text, kept for provenance — whitespace and layout differ from the originals.

**The structured versions in [`../`](.) are canonical** (`../01-prompt-engineering.md` … `../09-spec-interrogation.md`). Edit those, not these files.

---

## Where each one went

| Raw file | Structured output | What it became |
| --- | --- | --- |
| `A guide for prompts.md` | [`../01-prompt-engineering.md`](../01-prompt-engineering.md) – [`../05-prompt-anti-patterns.md`](../05-prompt-anti-patterns.md) | The core method: prompt engineering, context engineering, skill files, product documents, anti-patterns |
| `AI_Assisted_Production_Development_Workflow.md` | [`../06-development-workflow.md`](../06-development-workflow.md) | The 19-phase lifecycle, with a skill mapped to each phase |
| `Instructions.md` | [`../07-harness-rules.md`](../07-harness-rules.md) | The 19 operating rules, generalised; personal details removed |
| `questions.md` | [`../09-spec-interrogation.md`](../09-spec-interrogation.md) | The seven questions for interrogating a score-producing spec |
| `sdd-ask-model-imp.txt` | [`../08-architecture-review.md`](../08-architecture-review.md) | The architecture-review checklist and the six questions people skip |

The style listing in [`../frontend-design-styles.md`](../frontend-design-styles.md) is the source for the design taxonomy referenced in the repo README.

---

## Notes on the originals

- `Instructions.docx` originally listed the author's email, GitHub, and LinkedIn in its last rule. That text is removed in [`../07-harness-rules.md`](../07-harness-rules.md); the canonical rule keeps no contact details.
- `A guide for prompts.docx` carried a personal byline on its title page. Removed before the markdown conversion, and the `.docx` binaries are not kept in this repo.
- A superseded early draft of this material existed as an `.rtf` file carrying a personal name in its Word metadata. It is not distributed.

Everything else in this folder is verbatim source text.

---

## Regenerating a structured file

The mapping above is one-way. The `.md` files in this folder are inputs; the structured guides are the deliverable. If the source changes, edit the corresponding guide by hand and note it in the commit message.
