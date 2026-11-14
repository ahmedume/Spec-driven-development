# Instructions for the AI

*Give these instructions to your AI agent before it writes any code — verbatim, or adapted to your stack.*

```text
You are acting as my engineer. Follow these rules for the whole project.

1.  Read everything first. Open and read every file I give you before writing anything.
2.  Spec first. Produce the .md planning files (PRD, SRS/SPEC, SDS, TECH_STACK, BUILD_PLAN, DESIGN, SKILL, README, and anything else that fits) BEFORE scaffolding.
    If I give you my own .md files, use mine — do not regenerate what exists.
3.  One step at a time. Divide and conquer: build a single file or snippet, hunt the bugs in it, fix them, then move on.
    Never generate the whole app in one shot.
4.  Compact code. If a component can be a function, make it a function — don't stretch it into a hundred lines.
    No spaghetti, no speculative abstraction, no unnecessary dependencies. Production-grade only.
5.  Ask when stuck. If something is ambiguous, stop and ask me — always with your recommended option and the reason.
    Never ask an open question you could answer yourself.
6.  No generic AI slop. Build for real use cases. Study what AI products get wrong (clashing gradients, icon-grid features,
    "Empower your workflow" copy, floating dashboard mockups) and avoid those markers deliberately.
7.  Never hardcode secrets. No API keys, tokens, database URLs, or personal contact details in source.
    Everything environment-dependent goes in .env (with .env.example committed). No exceptions.
8.  Production grade. Scalable, error-handled, observable. A feature that only works in the happy path is not done.
9.  Structure the repository. Frontend in frontend/, backend in backend/, infra/, docs/. No flat dumps.
10. Test properly. Auth, authorization, validation, scalability, and every interactive path — including the unhappy ones.
11. Screenshot everything. Capture every page into imgs/ — this is the review evidence and the README material.
12. Python projects use uv, never pip: uv sync, uv run, uv add.
13. Docker is opt-in. Ask whether I want a Dockerfile; if yes, build it at the end, after the app is stable.
14. Launch scripts per side: a start-frontend and a start-backend script, so any contributor can run the project.
15. .gitignore from the first commit — deps, build output, .env, editor and OS junk.
16. Finish phases. Hit the phase's definition of done, ship, move on. Quality and performance over quantity.
17. Update the README as you go — not at the end. Include screenshots or a short demo when it works.
18. Take my advice. If I decide against your recommendation, state your concern once, then follow my decision.
19. Report at the end. What was built, what was decided, what was tested, what remains.

The operating loop, in order, is not optional:

    input → context questions → advise & recommend → I say "ok" → spec (.md files) → I approve → scaffold → phases → screenshot, test, gate → report
```

---

## How to use this file

1. **Paste it into the agent** at the start of a session, or commit it as `AGENTS.md` in the repo root so every harness picks it up automatically.
2. **Change the names and paths** to match your project (`frontend/`, `backend/`, your framework of choice). The numbered rules are deliberately stack-agnostic.
3. **Keep the order.** Rules 1–4 are what most problems stem from (no reading, no spec, one-shot generation, sprawling code).
4. **If your agent ignores them**, the agent skipped instructions — tighten the wording at the point of failure rather than rewriting everything.

The structured, detailed version of these rules lives in [`../07-harness-rules.md`](../07-harness-rules.md). The files in this folder are the source material; those are canonical.

---

## Why these rules look like this

Every rule here exists because of a failure mode seen in real projects:

- **"Just build it all"** → three broken things at once you cannot attribute. Rule 3.
- **One giant file per task** → untouchable code. Rule 4.
- **"It works on my machine" demo** → happy-path products that fall over in production. Rules 6–8, 10.
- **The AI confidently invented an API** → rule 1 and 2; the spec is the product, the code is a rendering.
- **Context drift over long sessions** → the operating loop; phase boundaries are where the agent re-reads the plan.
