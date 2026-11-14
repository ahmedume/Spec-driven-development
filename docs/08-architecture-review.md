# Architecture Review

The phase between planning and coding where a design is interrogated until it either holds or breaks, on paper, while that is still cheap.

---

## Why this is a phase, not a vibe

Every project has an architecture review. The only question is when it happens — before the first file is written, or in the diff of the pull request that finally exposes the assumption nobody checked.

An architecture review that lives as "we'll talk about it as we go" is not a review. It is a series of late discoveries, each one wearing the costume of a bug. A missing index, an undefined error body, a permission check that only exists on the happy path — none of these are mistakes in code. They are questions that were never asked.

> It is cheaper to argue on a diagram than in a diff.

Three properties make this a phase rather than a meeting:

| Property | What it means in practice |
| --- | --- |
| **It has an input** | A written design — components, contracts, schema, flows. Not a verbal vision. |
| **It has an exit condition** | Every row of the baseline checklist answered, and every gap turned into an owned action item. |
| **It gates something** | Scaffolding does not begin until it closes. A review with no gate is an opinion. |

The gate is the whole point. Without it, the review produces a document nobody reads and the same questions resurface mid-build, when the answer requires a rewrite.

---

## The baseline checklist

Every design must answer these, in writing, before coding starts. "In writing" matters: an answer that only exists in someone's head cannot be reviewed, contradicted, or checked later.

| Area | The question to ask |
| --- | --- |
| **Requirements** | Which requirement does this component satisfy, and where is it written? |
| **API contracts** | What are the exact request and response shapes, status codes, and error bodies? |
| **Database schema** | What are the tables, columns, types, relationships, indexes, and constraints? |
| **Data flow** | Where does data enter, where is it transformed, where does it land? |
| **Security considerations** | Who can call this, with what token, and what happens when that token is forged, expired, or absent? |
| **Edge cases** | Empty state, one item, ten thousand items, concurrent writes, partial failure. |
| **Failure modes** | What happens when the database is down, the queue is backed up, or the third party times out? |
| **Testing strategy** | What is unit-tested, what is integration-tested, what is only ever verified by hand? |

Two rules make the table useful rather than decorative:

1. **A row with no answer is a finding, not a formality.** Write down "unknown" and attach an owner. Silence is the failure mode.
2. **The schema row is not optional for a project that stores anything.** "We'll let the ORM figure it out" means nobody has decided what the data is, which means nobody can decide what the code does.

### Supporting areas

The eight rows above are the floor. Larger systems add:

- **Authorisation**, separate from authentication — who may do this, not merely who is asking.
- **Caching** — what is cached, for how long, and how it is invalidated.
- **State management** — where the client's source of truth lives and what may mutate it.
- **Deployment and environment parity** — what differs between local, staging, and production.
- **Observability** — what you will be able to see when it breaks at 3am.

---

## The questions most people skip

*"Why did you use this?"* is the easy question, and it is the one most reviews stop at. Anyone can answer it, because the answer is a restatement of the choice. The questions below cannot be answered by restating the choice. They require the author to have considered the alternative and paid for the decision.

### 1. What alternatives did you reject, and why?

**Why it is better:** it tests whether a decision was made or merely inherited. A choice with no rejected alternative is not a decision — it is a default, a habit, or something copied from the last project.

**Weak answer:** "It's the most popular option." "It's what I know." "It's what the tutorial used." Popularity is not a reason; it is a summary of other people's reasons, applied to their problem.

**Strong answer:** names one or two real candidates, states the property that separated them, and admits what the chosen option gives up.

### 2. What are the trade-offs?

**Why it is better:** every architecture decision costs something. Naming the cost proves the author looked past the benefit. A design presented as purely positive has not been examined.

**Weak answer:** "There aren't really any downsides." Or a list of trade-offs that are actually benefits in disguise ("it's slightly more scalable").

**Strong answer:** states what got worse — latency, complexity, operational burden, hiring pool, migration cost — and why that was an acceptable price here.

### 3. What breaks first under ten thousand users?

**Why it is better:** it forces the abstraction to meet a number. Vague architectures survive vague questions; they do not survive arithmetic. This is the highest-value question in the set.

**Weak answer:** "It'll scale fine." "We can always add caching later." Both are predictions about a system nobody has measured, offered without a named bottleneck.

**Strong answer:** names the specific component that fails first — the unindexed query, the synchronous third-party call, the single worker, the connection pool — and says roughly at what load and what the first mitigation would be.

> A design that cannot name its own bottleneck does not know where it is thin.

### 4. What is the weakest part of this architecture?

**Why it is better:** it asks the author to grade their own work. Reviewers trust a designer who knows the soft spots far more than one who claims there are none, because the second designer is either inexperienced or not being honest.

**Weak answer:** "Nothing, really." The review is not finished. There is always a weakest part.

**Strong answer:** points at a specific seam — a shared table, an unowned service, a hand-rolled auth flow — and says why it is thin and what would need to be true for it to become the incident.

### 5. What technical debt did we introduce, and when does it come due?

**Why it is better:** it converts an informal feeling into a tracked liability. Debt taken deliberately is a tool. Debt taken accidentally is a surprise with interest.

**Weak answer:** "None — we did it properly." Every design that ships under a deadline carries debt; claiming otherwise usually means it is unexamined rather than absent.

**Strong answer:** lists the specific shortcuts, the reason each was taken, and the trigger that forces repayment — "when we add a second tenant", "before the first paying customer", "when the queue exceeds a day of backlog".

### 6. If this were an interview, how would I justify every major decision?

**Why it is better:** it is the coherence test. Individual answers can each sound reasonable while contradicting one another. Recounting the whole design as a defence forces those contradictions into the open.

**Weak answer:** a narrative that skips decisions, or one that justifies each choice by appealing to a different authority ("industry standard", "best practice", "the client asked") without connecting them.

**Strong answer:** a short, continuous account in which each decision follows from the one before it and from the stated constraints. If the story has a jump cut, that is where the design is unresolved.

---

## How to run it

1. **Circulate the design document at least a day ahead.** The review is not the first time the room sees the architecture. Reviewers who read on the spot ask clarifying questions instead of hard ones.
2. **The author presents without the code.** Code anchors the discussion to what exists and forecloses the question of what should exist. Present the diagram, the contracts, and the schema.
3. **Work the baseline table top to bottom.** Do not skip rows because they seem boring. The requirements row and the schema row are where most designs are thinnest.
4. **Ask the six skipped questions in order.** They build on each other: alternatives feed trade-offs, trade-offs expose the bottleneck, the bottleneck is usually the weak point, the weak point is usually the debt.
5. **Record every unanswered question as an action item with an owner and a date.** An unowned action item is a note. A note is not a gate.
6. **Have a second person write the minutes.** The author is defending; they will not hear the gaps accurately.
7. **Close the gate explicitly.** Either the design passes, passes with conditions, or returns. Say which, out loud, and write it down.

> If the author cannot answer question 4 — the weakest part — in under a minute, the design is not understood well enough to build.

### Recording outcomes

A review that produces only conversation produces nothing. Keep a short artefact with four fields:

| Field | Content |
| --- | --- |
| **Decision** | Pass, pass with conditions, or return for revision. |
| **Findings** | Each unanswered baseline row and each weak answer, stated as a question, not a complaint. |
| **Actions** | Owner, date, and the condition that marks it done. |
| **Deferred** | What was raised, judged acceptable, and consciously left — with the trigger that reopens it. |

The deferred list is what separates a real review from a wish list. It records consent, not omission.

---

## Red flags

Answers that should stop the build until they change:

- **"Nothing breaks — it'll scale."** No bottleneck named means no capacity thinking has happened.
- **"There are no trade-offs."** The author has not looked past the benefit.
- **"Nothing is weak, it's a clean design."** Either inexperience or dishonesty. Both are expensive.
- **"No technical debt."** The debt exists; it is just untracked, which is worse.
- **"We'll figure out the schema when we start coding."** The data model is the architecture. Coding it by accident is how migrations are born.
- **"The API will be REST-ish, we'll define the shapes later."** Undefined contracts become incompatible clients.
- **"Security is handled by the framework."** A framework authenticates. It does not know who is allowed to read this row.
- **"Testing will be added at the end."** A design with no test strategy has no definition of done.
- **"Just copy what the last project did."** There is no rejected alternative, so there is no decision.
- **A design that only exists verbally.** If it cannot be circulated, it cannot be reviewed.

Any one of these is a single question away from being fixable. The point of the gate is that it gets fixed before scaffolding, not after.

---

## Relationship to the other documents

- [`06-development-workflow.md`](06-development-workflow.md) places this phase third, between requirements and the development plan.
- [`09-spec-interrogation.md`](09-spec-interrogation.md) holds the companion question set for interrogating a feature spec.
- `architecture-designer`, `api-designer`, and `the-fool` are the skills to load: design, contract, challenge.

---

*Structured from `sdd-ask-model-imp.txt`.*
