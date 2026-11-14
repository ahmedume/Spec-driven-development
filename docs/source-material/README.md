# Source material

The raw files this method was built from, converted to markdown. Two of them have been rewritten for public use:

- **`instructions.md`** — now *Instructions for the AI*: a paste-as-is instruction block for any coding agent.
- **`questions.md`** — now *Questions to Ask After the Build*: a categorised question bank for debriefing the agent.

The remainder are kept as auto-extracted text so the original wording stays traceable. Tables and multi-column layout land as flat lines with `|` separators; that is expected.

**The structured guides in [`../`](../README.md) are canonical.** Edit those, not these files.

---

## Where each one went

| Raw file | Structured output | What it became |
| --- | --- | --- |
| `a-guide-for-prompts.md` | [`../01-prompt-engineering.md`](../01-prompt-engineering.md) – [`../05-prompt-anti-patterns.md`](../05-prompt-anti-patterns.md) | The core method: prompt engineering, context engineering, skill files, product documents, anti-patterns |
| `ai-assisted-production-development-workflow.md` | [`../06-development-workflow.md`](../06-development-workflow.md) | The 19-phase lifecycle, with a skill mapped to each phase |
| `instructions.md` | [`../07-harness-rules.md`](../07-harness-rules.md) · now a public *Instructions for the AI* prompt pack | The 19 operating rules, generalised; personal details removed |
| `questions.md` | [`../09-spec-interrogation.md`](../09-spec-interrogation.md) · now a post-build *question bank* | The seven-question inquiry, expanded and sorted by theme |
| `sdd-ask-model-imp.txt` | [`../08-architecture-review.md`](../08-architecture-review.md) | The architecture-review checklist and the six questions people skip |

The styles A–Z listing lives at [`../frontend-design-styles.md`](../frontend-design-styles.md) — the source for the design taxonomy referenced in the repo README.

---

## Notes on the originals

- `instructions.md` and `questions.md` were rewritten in-place for public release: the original documents carried a private voice and concrete personal contact details. The current files are generic, audience-neutral, and keep no personal identifiers; the earlier drafts are removed.
- `A guide for prompts.docx` carried a personal byline on its title page. Removed before the markdown conversion.
- A superseded early draft existed as an `.rtf` file carrying a personal name in its Word metadata. It is not distributed.

Everything else in this folder is verbatim source text.

---

## Regenerating a structured file

The mapping above is one-way. The files here are inputs; the structured guides are the deliverable. If the source changes, edit the corresponding guide by hand and note it in the commit message.
