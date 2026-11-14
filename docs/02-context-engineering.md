# Context Engineering

Designing the informational environment around a model, not just the prompt in front of it.

---

## The six-step process

### Step 1 — Define the operating environment

Write a system prompt that establishes:

- **Identity and role** — *"You are a medical research assistant…"*
- **Domain of operation** — biomedical literature, legal contracts, financial analysis.
- **Tone and style** — formal, concise, technical.
- **Absolute rules** — what the model must always or never do.

### Step 2 — Inject relevant knowledge

Provide the documents, schemas, APIs, and references needed to complete the task accurately. In production this is usually retrieval-augmented generation (RAG).

- API documentation the model will code against
- Database schemas, so generated SQL or ORM queries are correct
- Brand guidelines, tone-of-voice documents
- Domain glossaries and knowledge bases

### Step 3 — Create skill files

Skill files encode hard-won best practices for a specific task and stop the model repeating known mistakes. Full treatment in [`03-skill-files.md`](03-skill-files.md).

### Step 4 — Manage memory and history

| Type | Contents |
| --- | --- |
| **Short-term** | Full conversation history within a session |
| **Long-term** | User preferences, past decisions, project state stored externally |
| **Episodic** | Key milestones or prior outputs the model should know about |

### Step 5 — Define tools and capabilities

If the model can act — search, run code, write files — define each tool with its name, purpose, parameters, and expected output shape. Vague tool descriptions are the most common cause of bad tool calls.

### Step 6 — Test, iterate, refine

- Run edge-case scenarios to find where the context breaks down.
- Refine skill files and system prompts against observed failure modes.
- **Version-control your context files** exactly as you would source code.

---

## The five-layer architecture

A production-ready template for any AI product or workflow.

| Layer | Contents | Purpose |
| --- | --- | --- |
| **1. Identity** | System prompt: role, personality, expertise, absolute rules | Who the model is and what it will never do |
| **2. Knowledge** | Documents, APIs, schemas, glossaries | What the model knows about your world |
| **3. Skills** | One `SKILL.md` per task type | How the model does this kind of work here |
| **4. Memory** | Conversation history, preferences, session state | What the model remembers |
| **5. Tools** | Function and tool definitions | What the model can do |

Layers 1–3 are stable and versioned. Layer 4 changes every turn. Layer 5 changes with the product surface.

### System prompt template

```markdown
# IDENTITY
You are [Expert Role] specializing in [Domain].
Your communication style is [formal / technical / concise].
You are operating within [Product or System Name].

# CORE RULES
ALWAYS:
  - [non-negotiable behaviour]
  - [non-negotiable behaviour]
NEVER:
  - [forbidden behaviour]
  - [forbidden behaviour]

# KNOWLEDGE CONTEXT
[injected documents, schemas, APIs]

# ACTIVE SKILL FILES
[which SKILL.md files are currently active]

# CURRENT SESSION STATE
Project: [name]
User: [role / level]
Date: [current date]
Status: [workflow stage]

# OUTPUT EXPECTATIONS
Format: [expected structure]
Length: [guidance on response length]
Language: [human language, and coding language if relevant]
```

---

## Worked example: a biomedical research assistant

An end-to-end illustration of all five layers, for a system that automates PubMed literature reviews.

### Layer 1 — Identity

```
You are a biomedical AI research assistant embedded in a multi-agent pipeline.
Your specialization is automating PubMed literature reviews for medical researchers.
You communicate in precise, academic English for a scientific audience.
You operate as part of a LangGraph multi-agent architecture.

ALWAYS:
  - Cite PubMed IDs (PMIDs) for every referenced paper
  - Flag low-quality evidence with an explicit warning
  - Use PICO format when structuring research questions
NEVER:
  - Generate medical advice or treatment recommendations
  - Claim confidence in findings not supported by cited evidence
```

### Layer 2 — Knowledge

```
# PUBMED API REFERENCE
Base URL: https://eutils.ncbi.nlm.nih.gov/entrez/eutils/
Search: esearch.fcgi?db=pubmed&term=[query]&retmax=20
Fetch:  efetch.fcgi?db=pubmed&id=[pmid]&rettype=abstract

# EVIDENCE GRADING SCHEMA
Grade A: systematic reviews and RCTs with low bias risk
Grade B: cohort and case-control studies
Grade C: case reports, expert opinion, editorials
```

### Layer 3 — Skill file

```
# SKILL: literature-review.md
ALWAYS structure summaries as: Background, Methods, Results, Conclusion
NEVER summarize papers not retrieved from the PubMed API call
Use PRISMA reporting standards for systematic reviews
Flag papers older than 5 years as "potentially outdated"
```

### Layer 4 — Session state

```
Research Query: "Efficacy of mRNA vaccines in immunocompromised patients"
Papers Retrieved: 12
Current Agent: Evidence Builder
Next Agent: HITL Email Collector
User: Immunology Research Team
```

### Result

Every agent in the pipeline operates with full awareness of the domain, the data, the rules, and the current workflow state — producing consistent, citation-backed output rather than plausible-sounding prose.

---

## Why this matters more than prompt wording

A better-worded prompt buys you one good response. A designed context buys you a thousand consistent ones, across sessions, across team members, across models.

The failure mode to watch for: **context rot**. As layers accumulate, conflicting instructions and stale documents quietly degrade output. Re-read your identity and knowledge layers quarterly. Delete what's no longer true.

---

*Structured from `a-guide-for-prompts.md` (Sections 2–3).*
