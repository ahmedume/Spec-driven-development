# Harness Rules

Operating rules for an agent working in this harness. Project-agnostic — copy into `AGENTS.md` and adjust.

The order matters: read, then spec, then build, then verify, then document. Most failures come from starting at step three.

---

## At a glance

| # | Rule | Category |
| --- | --- | --- |
| 1 | Read everything before writing anything | Process |
| 2 | Spec first; use the user's spec if they have one | Process |
| 3 | Build one step at a time | Process |
| 4 | Ask when ambiguous — with a recommendation attached | Process |
| 5 | Maximum code compaction | Build |
| 6 | No generic AI slop | Build |
| 7 | Production grade, not prototype grade | Build |
| 8 | Structure the repository | Build |
| 9 | Python uses `uv`, never `pip` | Build |
| 10 | Docker is opt-in, built last | Build |
| 11 | Separate launch scripts per side | Build |
| 12 | `.gitignore` from the first commit | Build |
| 13 | Never hardcode secrets | Safety |
| 14 | Test safety and behaviour, not just the happy path | Quality |
| 15 | Screenshot everything | Quality |
| 16 | Take the user's advice | Conduct |
| 17 | Finish phases; don't drag them | Delivery |
| 18 | Document as you go | Delivery |
| 19 | Report at the end | Delivery |

---

## Process

### 1. Read before acting

Read every provided file completely before writing anything.

Reading is cheap. A wrong rewrite of a 700-line file is not. If a file is long, read it in full anyway — partial reads produce confident, wrong edits.

### 2. Spec first, always

Run the spec-driven process and produce the `.md` structure before scaffolding.

**If the user supplies their own spec files, use theirs.** Do not regenerate what already exists. Fill gaps, flag conflicts, ask about contradictions. Regenerating a document the user wrote is the fastest way to lose their trust in the process.

### 3. One step at a time

Divide and conquer. Build a single file or snippet → hunt bugs in it → move on.

Never generate an entire application in one pass. When three things are wrong at once, you cannot tell which change caused which failure.

### 4. Ask, don't guess

If something is ambiguous, or a decision carries real cost, stop and ask.

Always pair the question with your recommended option and the reason. Never ask an open question you could have answered yourself — that's offloading work, not resolving it.

---

## Build

### 5. Maximum code compaction

**The most important rule.**

- If a component can be a function, it's a function — not a 100-line class.
- No speculative abstraction, no unnecessary dependency, no boilerplate.
- Prefer the standard library, then the platform, then a package.
- Load `karpathy-guidelines` for structural economy and `caveman` for output economy.

Compact is not the same as clever. Small, readable, production-grade beats small and clever.

### 6. No generic AI slop

Build for real use, not for the demo.

- Before designing, look at what competing and AI-generated products look like.
- Actively avoid their markers: default purple gradients, three-feature icon grids, "Empower your workflow" copy, a centred hero with a floating dashboard mockup, testimonial carousels with stock portraits, bento grids of fake metrics.
- If the output could be identified as generated without reading the copy, it isn't finished.

### 7. Production grade, not prototype grade

Scalability, error handling, and observability are part of the definition of done. A feature that only works on the happy path is not done.

### 8. Structure the repository

No flat dumps.

```
frontend/   backend/   infra/   docs/   tests/
```

If a folder has more than ~15 unrelated files, it needs subfolders. A new contributor should be able to guess where a file lives before searching for it.

### 9. Python uses `uv`

`uv` for every Python and FastAPI project. Never `pip`.

```bash
uv sync
uv run python -m app
uv add fastapi
```

### 10. Docker is opt-in, built last

Ask whether a Dockerfile is wanted. If yes, build it **after** the app is stable — not alongside scaffolding, where a broken container hides a real configuration error.

### 11. Separate launch scripts

One script per side, so a new contributor can run the project without reading docs:

- `start-frontend.bat` / `start-backend.bat` on Windows
- `start-frontend.sh` / `start-backend.sh` elsewhere

### 12. `.gitignore` from the first commit

Ignore build output, dependencies, `.env`, editor state, and OS artefacts. A secret committed once is a secret leaked forever, even after the rewrite.

---

## Safety

### 13. Never hardcode secrets

No API keys, JWT secrets, database URLs, tokens, or personal contact details in source.

- Everything environment-dependent goes in `.env`.
- Commit `.env.example` with placeholder values.
- Add `.env` to `.gitignore` before the first commit.
- If a secret is ever committed, rotate it — don't just delete the file.

---

## Quality

### 14. Test safety and behaviour

Cover authentication, authorisation, input validation, business logic, scalability, and **every interactive path**.

A click a user can make is a click that must be tested. Include the unhappy paths: expired token, empty input, double submit, slow network, concurrent write.

### 15. Screenshot everything

Capture every page and key flow into `imgs/`.

This is the review evidence and the README's raw material. Screenshots also catch what tests miss: layout breaks, clipped text, states that render but look broken.

---

## Conduct

### 16. Take the user's advice

The user knows their domain and their constraints. When your recommendation conflicts with theirs, state it once with reasoning, then follow their decision.

Repeating an objection after the decision is made is not diligence.

---

## Delivery

### 17. Finish phases

Don't drag a phase out chasing perfection. Hit its definition of done, ship it, move on.

Optimise for quality and performance over quantity and scope. A finished small thing beats an unfinished large one.

### 18. Document as you go

Update `README.md` continuously — not at the end. It should carry:

- what the project is and who it's for
- setup and run instructions
- architecture overview
- at least one screenshot or a short demo video of the product working

### 19. Report at the end

At the end of a phase or project, produce a full written report with the `docx` skill: what was built, what was decided, what was tested, what remains.

---

## The operating loop

This sequence is not optional:

```
User gives input
  → read everything provided
    → ask questions, take full context
      → advise and recommend
        → user says "ok"
          → spec-driven approach (9 detailed .md files)
            → user evaluates and approves
              → scaffold
                → build in phases, one at a time
                  → screenshot, test, gate
                    → report
```

Never skip from input to code. **The spec is the product; the code is a rendering of it.**

---

## Per-task checklist

Before declaring a task complete:

- [ ] I read every file I was given
- [ ] The spec covers this change
- [ ] It's the smallest implementation that works
- [ ] No secret is hardcoded
- [ ] Types pass
- [ ] Lint passes
- [ ] Tests pass
- [ ] The unhappy paths are handled
- [ ] I looked at it in a browser or terminal
- [ ] Docs/README updated if behaviour changed

---

*Structured and generalised from the original `Instructions` source. Personal contact details have been removed — rule 13 is where your own values belong.*
