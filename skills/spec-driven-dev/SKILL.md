---
name: spec-driven-dev
version: 1.0.0
description: >
  Use this skill whenever the user asks to "build", "create", "make", or "design" any app, feature, tool, 
  API, or system — even casually phrased requests like "build me a to-do app", "create a login system", 
  "make a dashboard", or "design a chat feature". This skill MUST trigger before any code is written. 
  It generates a complete, exhaustive spec-driven context package: an enhanced prompt, BUILD_PLAN.md,
  PRD, SRS, SDS, SPEC, TECH_STACK.md, SKILL.md, and PROMPTS.md — all as a single structured output 
  the user can break into markdown files and feed to an agentic coding tool (OpenCode, Cursor, Windsurf, etc.). 
  Do NOT skip this skill and jump straight to code. Spec first. Always.
---

# Spec-Driven Development + Context Engineering Skill

## What This Skill Does

When the user says "build me X", this skill does NOT write code.

Instead, it outputs a **complete spec-driven context package** — a long, structured, copy-paste-ready 
document that the user manually breaks into markdown files and feeds to their AI coding tool.

The package covers:
1. **Enhanced Prompt** — a rewritten, high-quality version of the user's request
2. **SPEC.md** — what we are building (blueprint)
3. **TECH_STACK.md** — all technologies, frameworks, versions, and rationale
4. **BUILD_PLAN.md** — how the project is built: phases, milestones, divide-and-conquer strategy
5. **PRD.md** — product requirements (who, why, what success looks like)
6. **SRS.md** — exact behavioral rules and engineering contract
7. **SDS.md** — architecture, schema, API routes, folder structure
8. **SKILL.md** — how the AI coding agent should behave on this project
9. **PROMPTS.md** — reusable execution prompts for each major task

Each section is clearly tagged with its file name so the user knows exactly what to copy into what file.

---

## When This Skill Triggers

- "Build me a [anything]"
- "Create a [feature / app / tool / API / system]"
- "Make a [anything]"
- "Design a [anything]"
- "I want to build [X]"
- "Help me build [X]"
- "Generate a [app / feature]"
- Any request where software is being constructed from scratch

## When NOT to Use

- User is asking a question, not building something
- User wants to debug existing code
- User wants to refactor a specific function
- User explicitly says "just give me the code" and has already done their own spec

---

## Workflow: What to Do Step by Step

### Step 1: Clarify (if needed)

Before generating, check if the user's request is too vague. If so, ask ONE batch of clarifying questions:

- What is the tech stack? (or should we decide?)
- Who are the target users?
- Any must-have features vs nice-to-have?
- Any constraints? (auth, database type, deployment target)

If the request has enough detail, skip directly to Step 2.

### Step 2: Generate the Full Spec Package

Output the entire package in one response, clearly divided by file tags.

Read `references/package-template.md` for the exact structure and all section templates.

### Step 3: Tell the User What to Do

After the output, give brief instructions:
```
Copy each section above into its own markdown file:
- SPEC.md
- TECH_STACK.md
- BUILD_PLAN.md
- PRD.md  
- SRS.md
- SDS.md
- SKILL.md
- PROMPTS.md

Then open your AI coding tool and say:
"Read all .md files in this folder. Start with SPEC.md and BUILD_PLAN.md. Build feature by feature following the plan. 
Follow SKILL.md rules at all times."
```

---

## Output Rules (NON-NEGOTIABLE)

### ALWAYS
- Output ALL 9 sections every time — no skipping
- Use `# ===== FILE: FILENAME.md =====` headers to separate sections
- Write every section with real, filled-in content — never leave template blanks
- Make the Enhanced Prompt long, detailed, and CRISP-framework-compliant
- Include actual field names, route names, method names based on what is being built
- Include edge cases that are specific to what is being built
- Write SKILL.md rules in the prescriptive "ALWAYS / NEVER" format
- Put the tech stack in its own TECH_STACK.md file with a table of technology / version / purpose
- In BUILD_PLAN.md, break the build into 4-8 sequential phases with clear boundaries, dependencies, and deliverable for each

### NEVER
- Never skip sections because "they don't apply"
- Never write placeholder content like "[add your feature here]"
- Never write the actual implementation code inside the spec package
- Never use vague language — be specific about routes, fields, behaviors
- Never generate a spec that would make a different AI confused about what to build
- Never bury the tech stack inside another file — always put it in its own TECH_STACK.md
- Never skip BUILD_PLAN.md — even simple projects get at least 3 phases

---

## Quality Checks Before Outputting

Before finalizing, verify:
- [ ] Does the SRS have testable requirements? (not vague goals)
- [ ] Does the SDS have a real folder structure and real table schemas?
- [ ] Does the SKILL.md have at least 5 ALWAYS and 5 NEVER rules?
- [ ] Does PROMPTS.md have a prompt for each major feature?
- [ ] Is every section clearly tagged with its filename?
- [ ] Does TECH_STACK.md have a table with technology, version, and purpose columns?
- [ ] Does BUILD_PLAN.md break work into 4-8 sequential phases with specific deliverables per phase?

---

## Reference Files

- `references/package-template.md` — Full template for all 7 sections with writing guidance
- `references/examples.md` — Complete worked example: a To-Do App spec package

Read `references/package-template.md` before generating any output.
Read `references/examples.md` if you need to calibrate depth and style.
