# Product Documents: MVP, PRD, SRS, SDS

Four documents, one chain of accountability. Each answers a different question, and skipping one creates a hidden risk that surfaces later as an expensive bug.

| Document | Question it answers | Written by | Written when |
| --- | --- | --- | --- |
| **MVP** | Should we build this at all? | Founder / product | Before anything else |
| **PRD** | What are we building, for whom, and how do we know it worked? | Product | After the MVP hypothesis is chosen |
| **SRS** | What must the system do, precisely and testably? | Product + engineering | Before design |
| **SDS** | How will it be designed? | Architecture + senior engineers | After SRS approval, before code |

---

## 1. MVP — Minimum Viable Product

The smallest version of a product that delivers enough value to satisfy early adopters, validate a core hypothesis, and generate real feedback — for minimum investment of time, money, and engineering.

Popularised by Eric Ries in *The Lean Startup*. The insight: most product assumptions are wrong, and building the full product before validating them burns resources for no learning.

### MVP vs full product

| Aspect | MVP | Full product |
| --- | --- | --- |
| **Features** | Only the single most critical user journey | Everything the market expects |
| **Quality** | Good enough to use and learn from | Polished |
| **Time** | Days to weeks | Months to years |
| **Purpose** | Validate a hypothesis, gather real feedback | Serve a validated market |
| **Audience** | Early adopters who tolerate rough edges | Mainstream users |
| **Success metric** | Did users engage? Would they pay? Would they return? | Retention, revenue, growth |

### Why it matters more for AI products

- Model capabilities can be prototyped in hours against existing APIs.
- The core value proposition can be tested without a full backend.
- Feedback on AI output quality is immediate and unusually informative.
- Prompts, context, and skill files can be refined against real usage.

> **MVP rule:** if you build it and nobody uses it, the investment is gone. Build the smallest thing that tests whether people want what you think they want.

---

## 2. PRD — Product Requirements Document

A formal definition of what a product or feature must do, from the user's and business's perspective. It is the contract between the people who define what to build and the people who build it.

A PRD answers: *What are we building? Who is it for? What problem does it solve? What must it do? What does success look like?*

### Standard structure

1. **Executive summary** — one paragraph on the product and its purpose.
2. **Problem statement** — the specific user problem, with evidence.
3. **Goals and success metrics** — quantified outcomes, KPIs, OKRs.
4. **User personas** — who they are, what they need, what frustrates them.
5. **Use cases and user stories** — *"As a [user], I want to [action] so that [benefit]."*
6. **Functional requirements** — what the system must do.
7. **Non-functional requirements** — performance, security, accessibility, scalability.
8. **Out of scope** — the explicit list of what is *not* in this version.
9. **Dependencies and risks** — external dependencies, technical risks, open questions.
10. **Timeline** — milestones, release plan, prioritised roadmap.

> "Out of scope" is the most valuable section. It is the only thing that actually stops scope creep.

### Additional sections for AI products

- Which model(s) will be used, and why.
- Acceptable accuracy and quality thresholds.
- Human-in-the-loop requirements — when a person must review output.
- Failure modes — what happens when the model is wrong or low-confidence.
- Data requirements — what training or fine-tuning data is needed.

---

## 3. SRS — Software Requirements Specification

A precise technical specification of every system requirement, written from the software engineering perspective.

If the PRD says **what** the product does, the SRS says **exactly how** those requirements are expressed in testable, engineering-actionable terms.

### Standard structure (IEEE 830)

1. **Introduction** — purpose, scope, definitions, acronyms, references.
2. **Overall description** — product perspective, functions, user characteristics, constraints.
3. **Specific requirements** — detailed functional requirements with input/output specs.
4. **External interface requirements** — UI, hardware, software, communication.
5. **System attributes** — reliability, availability, security, maintainability.
6. **Other requirements** — legal, regulatory, compliance.

---

## 4. SDS — Software Design Specification

Describes **how** the software will be built to meet the SRS. Also called a Software Design Document (SDD).

Where the SRS says *what*, the SDS says *how it will be designed*.

### Standard structure

1. **System architecture** — high-level diagram and rationale.
2. **Component design** — each module, class, or service.
3. **Data design** — schema, entity-relationship diagrams, data flow.
4. **Interface design** — API contracts, UI wireframes, integration specs.
5. **Algorithm design** — pseudocode or flowcharts for complex logic.
6. **Security design** — authentication, authorisation, encryption, threat model.
7. **Performance design** — caching, load handling, optimisation decisions.

### SRS vs SDS at a glance

| | SRS | SDS |
| --- | --- | --- |
| **Focus** | What the system must do | How it will be designed |
| **Audience** | All stakeholders | Engineering |
| **Written by** | Analysts, PMs, engineers | Architects, senior engineers |
| **Created** | Before design | After SRS approval, before code |
| **Content** | Requirements, constraints, behaviour | Diagrams, schemas, APIs, algorithms |

---

## 5. How the documents chain

```
MVP proves the idea
  → PRD defines the vision
    → SRS specifies the requirements
      → SDS designs the solution
        → Code implements it
          → Tests verify it
```

Each document is an accountability checkpoint:

| Document | What it prevents |
| --- | --- |
| **MVP** | Wasted investment on unvalidated demand |
| **PRD** | Scope creep; misalignment between business and engineering |
| **SRS** | Ambiguous interpretation; untestable requirements; audit failures |
| **SDS** | Architectural drift; blocked parallel work; six-month onboarding |

Skipping a layer doesn't remove the work — it moves the work to the most expensive place to do it.

---

## 6. How this harness implements it

The `spec-driven-dev` skill turns a conversation into a spec package rather than a single PRD. In practice:

| Harness file | Maps to |
| --- | --- |
| `PRD.md` | PRD — problem, users, success metrics, scope |
| `SPEC.md` | SRS — testable requirements, EARS-format where useful |
| `SDS.md` | SDS — architecture, data model, interfaces |
| `TECH_STACK.md` | SDS — the decisions and rejected alternatives |
| `BUILD_PLAN.md` | Milestones, dependency order, phases |
| `DESIGN.md` | Visual system: tokens, type, spacing, motion |
| `SKILL.md` | Project-local rules the agent must follow |

Nine sections, written before scaffolding, reviewed by the user, then built in phases. See [`06-development-workflow.md`](06-development-workflow.md) for the full sequence.

---

*Structured from the original `A guide for prompts` document (Section 4).*
