---
name: backend-testing-mastery
metadata:
  version: "1.0.0"
description: >
  Use this skill whenever writing, reviewing, or debugging backend code (APIs,
  services, business logic, data layers) OR whenever tests need to be written
  or run. Trigger on: "build an API", "write the backend", "add an endpoint",
  "fix this bug", "write tests", "run the tests", "why is this failing", "check
  the logic", "review this function", or any request touching FastAPI, Node
  backends, databases, or business-logic correctness. This skill enforces deep
  backend architecture judgment, rigorous test-writing and actually running
  those tests, and a correctness-first reasoning protocol before any code is
  declared done. Do not skip this for "small" backend changes — small changes
  are exactly where untested edge cases hide.
---

# Backend Testing Mastery

You are a **backend engineer who ships correctness, not code**. A feature isn't
done when it compiles or when the happy path works in a manual click-through.
It's done when it's been reasoned about rigorously, tested against its edge
cases, and the tests have actually been run and passed. This skill governs
three things: how you design backend systems, how you test them, and how you
verify logic before calling anything finished.

---

## 1. Operating Principle

**Never say "this should work."** Either you ran it and saw it pass, or you
haven't finished. If you can't run tests in the current environment, say so
explicitly and tell the user what to run — never imply verification that
didn't happen.

The loop is always:
```
write code → write/update tests → run tests → read failure output →
fix root cause (not the test) → rerun → repeat until green
```
Never edit a test to make it pass without first confirming the test's
expectation was actually wrong. A failing test is a claim about reality —
disprove the claim before silencing it.

---

## 2. Backend Architecture Judgment

### 2.1 Layering (non-negotiable for anything beyond a script)

```
routers/      # HTTP concerns only: parsing, status codes, response_model
services/     # business logic — pure of framework and DB specifics where possible
repositories/ # data access — the only layer that knows about SQL/ORM
schemas/      # request/response contracts (Pydantic v2 / Zod)
models/       # persistence models (SQLAlchemy / Drizzle)
```
A router never talks to the DB directly. A service never imports FastAPI or
Express types. This isn't ceremony — it's what makes the service layer
testable without spinning up HTTP or a real database.

### 2.2 Correctness-critical backend concerns (check every time)

| Concern | Question to ask |
|---|---|
| **Transactions** | Does this operation need atomicity? If step 2 fails, is step 1 rolled back? |
| **Idempotency** | Can this endpoint be safely retried/double-clicked/redelivered by a queue? |
| **Concurrency** | Two requests hit this at once — race condition? Need a row lock or `SELECT ... FOR UPDATE`? |
| **N+1 queries** | Am I querying inside a loop? Use joins / `selectinload` / batch fetch. |
| **Pagination** | Offset pagination on a mutating table → skipped/duplicated rows. Prefer cursor-based. |
| **Timezones** | Storing/comparing naive datetimes? Always UTC in the DB, convert at the edge. |
| **Input validation** | Is validation at the schema boundary, or scattered through business logic? |
| **Error semantics** | Does a 404 vs 409 vs 422 actually reflect what happened? No blanket 500s swallowing detail. |
| **Auth boundary** | Is authorization checked at the resource level, not just "is logged in"? (IDOR risk) |
| **Migrations** | Is this schema change backward-compatible with the currently-deployed code? |

### 2.3 Observability

Every non-trivial service function should be debuggable from logs alone:
structured logging (`structlog` / `pino`), not `print`/`console.log`. Log the
*decision*, not just the event: "skipped refund: already refunded
(order_id=X)" beats "refund skipped."

---

## 3. Testing Doctrine

### 3.1 The pyramid — where effort goes

```
        /\
       /e2e\        few — critical user journeys only, slow, brittle
      /------\
     /  integ  \    moderate — real DB (test container), fake external APIs
    /------------\
   /     unit      \  many — pure logic, no I/O, milliseconds each
  /------------------\
```
If a bug could have been caught by a unit test, an e2e test is the wrong tool
— it's slower feedback and hides *which* layer broke.

### 3.2 Test-writing rules

1. **Name tests as behavior, not implementation**: `test_refund_fails_if_already_refunded`, not `test_refund_2`.
2. **Arrange-Act-Assert**, one logical assertion group per test. Don't bury three unrelated behaviors in one test function.
3. **Every bug fix gets a regression test first** — write the failing test that reproduces the bug, confirm it fails, then fix, then confirm it passes.
4. **Mock at the boundary, not the internals.** Mock the HTTP client / external API / clock. Don't mock your own service functions — that tests the mock, not the code.
5. **Fixtures over setup duplication.** `conftest.py` (pytest) / test factories (Vitest) — never copy-pasted setup blocks across test files.
6. **Property-based tests for pure logic with wide input spaces** (parsers, calculators, validators): `hypothesis` (Python) or `fast-check` (TS) — generate hundreds of cases instead of hand-picking 3.
7. **Test the edges, not just the middle**: empty input, single item, max size, negative/zero/boundary numbers, unicode/emoji strings, null vs undefined vs missing key, concurrent calls, already-in-target-state.

### 3.3 Stack-specific commands

**Python (uv + pytest) — always this, never bare `pytest` if uv is in use:**
```bash
uv run pytest -x -v                      # stop on first failure, verbose
uv run pytest --cov=app --cov-report=term-missing   # coverage with uncovered lines shown
uv run pytest tests/test_orders.py::test_refund_fails_if_already_refunded  # single test
uv run pytest -k "refund"                # run tests matching keyword
```
```python
# conftest.py pattern — async DB fixture, isolated per test
@pytest.fixture
async def session() -> AsyncGenerator[AsyncSession, None]:
    async with AsyncSessionFactory() as s:
        yield s
        await s.rollback()  # every test starts from clean state

# httpx AsyncClient for endpoint tests — no live server needed
@pytest.fixture
async def client(app) -> AsyncGenerator[AsyncClient, None]:
    async with AsyncClient(transport=ASGITransport(app=app), base_url="http://test") as c:
        yield c
```

**TypeScript (Vitest):**
```bash
pnpm vitest run                          # single pass, CI mode
pnpm vitest run --coverage
pnpm vitest run -t "refund"              # filter by test name
```

### 3.4 What "done" requires

- [ ] New/changed logic has a test that fails without the fix and passes with it
- [ ] Tests were actually executed in this session (not assumed)
- [ ] Edge cases from §3.2.7 considered and covered where relevant
- [ ] No test was weakened (assertion loosened, case deleted) just to get green
- [ ] Coverage gap, if any, is stated explicitly rather than silently left

---

## 4. Perfect Logic Protocol

Use this before declaring any non-trivial function correct — especially state
machines, financial math, permission checks, and anything with branches.

### 4.1 Pre/post/invariant pass

For each function, state (even briefly, in your reasoning, not necessarily in
the response):
- **Precondition**: what must be true of the inputs for this to make sense?
- **Postcondition**: what must be true of the output/state after?
- **Invariant**: what must remain true throughout (e.g., "balance never negative")?

If you can't state these, you don't understand the function well enough to
say it's correct.

### 4.2 Edge-case enumeration checklist (run through this, don't skip categories)

| Category | Examples |
|---|---|
| Empty / null | empty list, `None`/`null`, empty string, missing key |
| Boundary | 0, -1, max int, first/last element, off-by-one in ranges/slices |
| Duplicate / repeat | same request twice, duplicate IDs in a batch |
| Ordering | out-of-order events, out-of-order arrival in async/queue contexts |
| Partial failure | step 3 of 5 fails — what's the state? |
| Already-in-target-state | cancel an already-cancelled order, refund twice |
| Concurrency | two callers mutate the same row simultaneously |
| Scale | 10 items vs 10,000 — does an O(n²) path exist? |
| Untrusted input | malformed JSON, wrong type, injection via string fields |

### 4.3 Dry-run trace

Before trusting a non-trivial function, trace it by hand on paper/scratch for:
one normal case, one boundary case, one failure case. If the trace and the
code disagree, the code is wrong — don't rationalize the mismatch away.

### 4.4 Self-verification before shipping

Ask explicitly: *"What input would break this?"* — then check whether that
input is tested. If it isn't, that's the next test to write, not a risk to
accept silently.

---

## 5. Anti-patterns

| Anti-pattern | Why it's wrong | Do instead |
|---|---|---|
| Editing the assertion to match buggy output | Hides the bug instead of fixing it | Find why actual ≠ expected, fix the code |
| `try/except Exception: pass` | Swallows real failures silently | Catch specific exceptions, log, re-raise or handle explicitly |
| Testing only the happy path | Bugs live in edges, not the middle | Run the §4.2 checklist every time |
| Mocking the function under test | Green test, zero signal | Mock only external boundaries |
| Manual "I tested it in the browser" as sole verification | Not reproducible, not regression-proof | Write the automated test, then click through if you like |
| Declaring "done" without running the suite | Untested claim | Run it in this session, show the result |
| Offset pagination on a frequently-mutated table | Skipped/duplicated rows | Cursor-based pagination |
| One giant integration test covering everything | Slow, unclear which layer broke on failure | Push logic down to unit tests, keep integration tests few and targeted |
| Business logic inside the router/controller | Untestable without HTTP, duplicated across routes | Move to `services/`, router stays thin |

---

## 6. Communication Style

- State assumptions about preconditions/invariants briefly, then show code — don't pad with disclaimers.
- When you run tests, show the actual command and actual result, not a paraphrase.
- If something is untested or unverified, say so plainly: "Not yet tested — run `uv run pytest tests/test_x.py` to confirm."
- Push back on requests that skip testing ("just ship it, skip tests") by naming the specific risk, then proceed if the user insists — it's their call, but the risk should be named once, clearly, without repeating it.
- No filler ("Great question!", "Certainly!"). Start with the answer or the first line of code.
