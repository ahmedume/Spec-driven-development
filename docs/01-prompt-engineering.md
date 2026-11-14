# Prompt Engineering

How to write instructions that produce expert-level output on the first attempt.

---

## 1. What prompt engineering is

Prompt engineering is the practice of designing and refining the input text — the **prompt** — that you give an AI model in order to elicit a specific output. It sits at the intersection of linguistics, human-computer interaction, and systems design.

A model has no inherent understanding of your goals. It responds to patterns learned during training plus the context you provide in the moment. Prompt engineering is the discipline of shaping that context so the model behaves as intended.

**Why it matters:** output quality is almost entirely determined by prompt quality. A vague prompt yields generic results. A precise prompt yields focused, actionable results — whether you're asking for code, a document, an analysis, or an entire feature.

### Core concepts

| Concept | What it does |
| --- | --- |
| **Role assignment** | Tells the model which expert persona to adopt: *"You are a senior software architect…"* |
| **Task definition** | States explicitly what you want produced. |
| **Format specification** | Describes the output shape: JSON, markdown, numbered list, prose, table. |
| **Constraint setting** | Defines what to avoid, exclude, or limit. |
| **Few-shot examples** | Sample input/output pairs that anchor the behaviour. |
| **Chain-of-thought** | Instructs the model to reason step by step before answering. |

> Prompt engineering is not about tricking the model. It's about communicating intent as precisely as you would in a requirements document for a developer.

---

## 2. What context engineering is

Context engineering is the systematic design of **everything that enters the context window** — not just the prompt, but the entire informational environment the model operates in.

The distinction: prompt engineering is writing one good email. Context engineering is designing the communication system — the templates, the knowledge base, the workflow rules, the memory — that makes every email consistently good.

### What goes into it

- **System prompts** — persistent instructions defining role, personality, operational rules.
- **Injected knowledge** — documents, code, schemas, API references.
- **Memory and history** — prior turns, preferences, past decisions.
- **Skill files** — structured references encoding domain best practices (`SKILL.md`).
- **Tools and functions** — definitions of actions the model can take.
- **Examples and templates** — pre-built patterns for consistency.
- **Metadata and state** — date, user profile, project status, environment.

Context engineering is what separates a toy demo from a production system.

---

## 3. The difference between them

They are complementary, operating at different levels of abstraction.

| Dimension | Prompt engineering | Context engineering |
| --- | --- | --- |
| **Scope** | A single message or request | The entire AI environment |
| **Level** | Tactical, per task | Strategic, system-wide |
| **Who does it** | Any user interacting with AI | Engineers building AI systems |
| **Persistence** | One-off, ephemeral | Designed once, reused across sessions |
| **Output goal** | One good answer | A reliable AI product |
| **Complexity** | Low to medium | Medium to high — needs architecture thinking |
| **Tools** | Plain text | System prompts, `SKILL.md`, RAG, memory |

In practice: you use prompt engineering every day you talk to an AI. You use context engineering when you're building something that others will use, or that must run reliably without hand-holding at every step.

---

## 4. The CRISP framework

A memorable structure for every high-quality prompt.

| | Element | What it means | Example |
| --- | --- | --- | --- |
| **C** | Context | Background the model needs | *"I am building a SaaS app with Next.js 14 and PostgreSQL."* |
| **R** | Role | Expert persona to adopt | *"You are a senior full-stack engineer with 10+ years of SaaS experience."* |
| **I** | Instruction | The specific task | *"Design a secure authentication flow using JWT and refresh tokens."* |
| **S** | Specifics | Format, length, constraints | *"Provide TypeScript with inline comments. No external auth libraries."* |
| **P** | Purpose | Why you need it | *"This will be the auth system for a B2B product with 1000+ enterprise users."* |

---

## 5. How to write a high-level prompt

A high-level prompt supplies the right amount of context, role, and instruction to produce expert output with minimal iteration.

**The sequence:**

1. **Start with the role.** Define the expert identity before anything else.
2. **State the context.** Relevant stack, constraints, audience.
3. **Define the task precisely.** Use action verbs: create, analyse, refactor, design, audit.
4. **Specify the output format.** Never leave the structure to chance.
5. **Set constraints and boundaries.** What to include, what to avoid, what to assume.
6. **Add examples** for complex or ambiguous tasks.
7. **State the success criteria.** What does a perfect answer look like?

### Worked example

**Weak:**

```
Write a login function.
```

**Strong:**

```
You are a senior backend engineer specializing in Node.js and security.

Context: I am building a REST API with Express.js and PostgreSQL via Prisma.
Authentication uses JWT access tokens (15-min expiry) and refresh tokens
(7-day expiry).

Task: Write a secure login function that:
  1. Accepts email and password
  2. Validates credentials against the database using bcrypt
  3. Returns both an access token and a refresh token
  4. Handles errors with descriptive HTTP status codes

Format: TypeScript, async/await, with JSDoc on each function.
Constraints: Do not use Passport.js. Follow OWASP authentication practices.
Success: Production-ready and secure against timing attacks.
```

The second prompt supplies role, stack, task breakdown, format, and constraints. The output is dramatically more useful with zero back-and-forth.

---

## 6. Meta-prompting: using AI to write prompts

One of the highest-leverage skills is asking the model to write the prompt for you.

**The workflow:**

1. Describe your goal in plain language.
2. Ask the model to generate the optimal prompt for that goal.
3. Review and refine.
4. Use the refined prompt for the real task.

**Template:**

```
I need help generating a high-quality prompt for this task:

TASK: [what you want to accomplish, in plain language]
CONTEXT: [tech stack, domain, constraints, audience]
OUTPUT FORMAT: [how the final output should be structured]
CONSTRAINTS: [what the prompt must include or avoid]

Generate a complete, production-quality prompt that:
1. Assigns an appropriate expert role
2. Provides sufficient context for accurate output
3. Specifies the exact output format
4. Includes relevant constraints and success criteria
5. Is clear enough that no clarification questions are needed
```

### Prompt chains

For multi-step work, ask for a **chain** rather than one giant prompt. Example — automated code review of a Node.js REST API:

1. Static analysis of style and conventions
2. Security vulnerability identification
3. Performance bottleneck detection
4. Structured review report generation

Each prompt should be self-contained and reference the previous step's output.

> Once a prompt consistently produces excellent output, save it as a `SKILL.md` entry. It becomes a reusable asset instead of a good day.

---

## 7. Where this fits in the harness

- Write the prompt with **CRISP** when you need one good answer now.
- Promote it to a **skill file** when you need the same answer every time. See [`03-skill-files.md`](03-skill-files.md).
- Use **context engineering** when the prompt is only one layer of a system. See [`02-context-engineering.md`](02-context-engineering.md).
- Avoid the recurring failure modes in [`05-prompt-anti-patterns.md`](05-prompt-anti-patterns.md).

---

*Structured from `a-guide-for-prompts.md` (Sections 1–2).*
