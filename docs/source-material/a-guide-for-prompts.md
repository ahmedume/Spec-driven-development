# a-guide-for-prompts

> Auto-extracted from the original `A guide for prompts` document. Unstructured source text preserved as-is; tables and multi-column layout land as flat lines with ` | ` separators.
> The structured, canonical version lives in `01-prompt-engineering.md` at ../docs/01-prompt-engineering.md.

---

PROFESSIONAL DEVELOPER GUIDE
Prompt Engineering, Context Engineering
& AI-Assisted Product Development
A complete reference covering Skills.md, MVP, PRD, SRS, SDS,
high-quality prompt writing, and effective AI-driven development workflows
2026
For Developers, AI Engineers & Product Teams
Section 1 Prompt Engineering
|
1. What Is Prompt Engineering?
Prompt engineering is the practice of designing, crafting, and refining the input text called a "prompt" that you provide to an AI language model in order to elicit a desired output. It is a discipline that sits at the intersection of linguistics, human-computer interaction, and AI systems design.
When you interact with an AI model, the model has no inherent understanding of your goals; it responds based on patterns learned during training and the context you give it in the moment. Prompt engineering is the art of shaping that context so the model behaves exactly as you intend.
Why It Matters
The quality of an AI output is almost entirely determined by the quality of the prompt. A vague, ambiguous prompt yields vague, generic results. A precisely crafted prompt yields focused, professional, actionable results. This is true whether you are asking an AI to write code, draft a document, answer a research question, or build a software feature.
Core Concepts
Role Assignment: Telling the AI what expert persona it should adopt (e.g., 'You are a senior software architect...')
Task Definition: Explicitly stating what you want the model to produce
Format Specification: Describing the output format JSON, markdown, numbered list, prose, etc.
Constraint Setting: Defining what the model should avoid or limit
Examples (Few-Shot): Providing sample input-output pairs to guide the model's behavior
Chain-of-Thought: Instructing the model to reason step-by-step before giving a final answer
Key Insight: Prompt engineering is not about "tricking" the AI. It is about communicating your intent as precisely as possible much like writing a clear requirements document for a developer.
|
2. What Is Context Engineering?
Context engineering is the advanced practice of systematically designing and managing everything that goes into the AI's context window not just the immediate prompt, but the entire informational environment the AI operates within. While prompt engineering focuses on a single input message, context engineering is a holistic strategy.
Think of it this way: if prompt engineering is writing one good email, context engineering is designing the entire communication system the templates, the knowledge base, the workflow rules, the memory that makes every email consistently excellent.
What Goes into Context Engineering?
System Prompts: Persistent instructions that define the AI's role, personality, and operational rules
Injected Knowledge: Documents, code files, database schemas, API references anything the AI needs to know
Memory & History: Previous conversation turns, user preferences, past decisions
Skill Files (e.g., SKILL.md): Structured reference files that encode domain-specific best practices
Tools & Functions: Definitions of tools the AI can invoke to take real-world actions
Examples & Templates: Pre-built patterns the AI should follow for consistency
Metadata & State: Current date, user profile, project status, environment variables
Context engineering is what separates a "toy" AI demo from a production-grade AI system. It is the engineering discipline behind building reliable, scalable, intelligent software.
|
Section 2 Differences, Techniques & Best Practices
|
3. The Main Difference Between Prompt and Context Engineering
Both disciplines are complementary and often used together, but they operate at different levels of abstraction.
Dimension
| Prompt Engineering vs Context Engineering
|
Scope
| Prompt: Single message or request. Context: The entire AI environment.
|
Level
| Prompt: Tactical (per-task). Context: Strategic (system-wide).
|
Who Does It
| Prompt: Any user interacting with AI. Context: Engineers building AI systems.
|
Persistence
| Prompt: One-off, ephemeral. Context: Designed once, reused across sessions.
|
Output Goal
| Prompt: Get a good single answer. Context: Build a reliable AI product.
|
Complexity
| Prompt: Low-to-medium. Context: Medium-to-high (requires architecture thinking).
|
Tools Used
| Prompt: Just plain text. Context: System prompts, SKILL.md, RAG, memory systems.
|
In practice: you use prompt engineering every day when you interact with AI. You use context engineering when you are building an AI-powered product, tool, or automated workflow that others will use or that needs to run reliably without human hand-holding every step.
4. How to Write High-Level Prompts
A high-level prompt is one that gives the AI the right amount of context, role definition, and instruction to produce expert-level output on the first attempt (or with minimal iteration).
The CRISP Framework
Remember: C-R-I-S-P a memorable structure for every high-quality prompt.
CRISP Element
| What It Means & Example
|
C Context
| Background the AI needs. 'I am building a SaaS web app using Next.js 14 with a PostgreSQL database.'
|
R Role
| Expert persona to adopt. 'You are a senior full-stack engineer with 10+ years of SaaS experience.'
|
I Instruction
| The specific task. 'Design a secure authentication flow using JWT and refresh tokens.'
|
S Specifics
| Format, length, constraints. 'Provide the solution as TypeScript code with inline comments. No external auth libraries.'
|
P Purpose
| Why you need this. 'This will be the core auth system for a B2B product with 1000+ enterprise users.'
|
Step-by-Step: Writing a High-Level Prompt
Start with the Role: Define the AI's expert identity before anything else
State the Context: Give relevant background tech stack, constraints, audience
Define the Task Precisely: Use action verbs (create, analyze, refactor, write, design)
Specify the Output Format: Tell the AI how you want the answer structured
Set Constraints & Boundaries: What to include, what to avoid, what assumptions to make
Add Examples if Needed: For complex or ambiguous tasks, include a sample input/output
State the Success Criteria: What does a 'perfect' answer look like?
Example: Basic vs High-Level Prompt
Basic (Weak) Prompt:
Write a login function.
|
High-Level (Strong) Prompt:
You are a senior backend engineer specializing in Node.js and security.
Context: I am building a REST API with Express.js, using PostgreSQL via Prisma ORM.
Authentication uses JWT access tokens (15-min expiry) and refresh tokens (7-day expiry).
Task: Write a secure login function that:
1. Accepts email and password
2. Validates credentials against the database using bcrypt
3. Returns both an access token and a refresh token
4. Handles errors with descriptive HTTP status codes
Format: TypeScript, async/await, with JSDoc comments on each function.
Constraints: Do not use Passport.js. Follow OWASP authentication best practices.
Success: The function should be production-ready and secure against timing attacks.
|
The second prompt gives the AI everything it needs: role, stack, task breakdown, format, and constraints. The output will be dramatically more useful with zero back-and-forth.
|
5. How to Do Context Engineering
Context engineering is a multi-step process of designing the informational architecture that surrounds your AI model.
Step 1 Define the AI's Operating Environment
Write a system prompt that establishes:
The AI's role and identity (e.g., 'You are a medical research assistant...')
The domain of operation (e.g., biomedical literature, legal contracts, financial analysis)
The tone and communication style (e.g., formal, concise, technical)
Absolute rules (things the AI must always or never do)
Step 2 Inject Relevant Knowledge
Provide all documents, schemas, APIs, and reference material the AI needs to complete tasks accurately. This is often called Retrieval-Augmented Generation (RAG) in production systems.
API documentation the AI will use to generate code
Database schemas so the AI writes correct SQL or ORM queries
Brand guidelines, writing style guides, or tone-of-voice documents
Domain glossaries or knowledge bases
Step 3 Create Skill Files
Skill files (like SKILL.md) are structured reference documents that encode hard-won best practices for specific tasks. They prevent the AI from repeating common mistakes. (See Section 3 for full details.)
Step 4 Manage Memory & History
Decide what the AI should remember across turns:
Short-term: Full conversation history within a session
Long-term: User preferences, past decisions, project state stored externally
Episodic: Key milestones or previous outputs the AI should be aware of
Step 5 Define Tools & Capabilities
If the AI can take actions (search the web, run code, write files), provide clear tool definitions including their name, purpose, parameters, and expected output format.
Step 6 Test, Iterate, and Refine
Run edge-case scenarios to find where the context breaks down
Refine skill files and system prompts based on failure modes
Version-control your context files as you would any source code
Section 3 Skills.md & Context Structure
|
6. What Is SKILL.md?
A SKILL.md file is a structured markdown document that encodes domain-specific knowledge, constraints, best practices, and procedural instructions for a specific task or technology area. It is designed to be injected into an AI's context window before it performs tasks in that domain.
Think of SKILL.md as a training manual for the AI not for general knowledge (the AI already has that), but for your specific environment, your specific constraints, your specific stack, and the lessons your team has learned through trial and error.
Why SKILL.md Exists
AI models are trained on general knowledge. They don't know:
Your company's specific coding conventions
The quirks of your particular tech stack version
The business rules that govern your domain
The mistakes that have been made and should not be repeated
The preferred output formats for your toolchain
SKILL.md fills this gap. It is the bridge between the AI's general intelligence and your specific operational context.
Real-world analogy: SKILL.md is like the internal wiki page your most experienced developer wrote before going on vacation it tells the AI exactly what it needs to know to do the job correctly without making the same old mistakes.
|
7. How to Write SKILL.md
A well-written SKILL.md is concise, scannable, unambiguous, and actionable. Follow this process:
SKILL.md Writing Principles
Write for an expert reader: The AI already knows the fundamentals; focus on your specifics
Be prescriptive, not descriptive: Say 'Always do X' not 'X is generally considered good'
Include negative rules: 'NEVER do Y' is as important as 'Always do X'
Use concrete examples: Abstract rules are harder for the AI to apply consistently
Organize with headers: The AI should be able to find relevant sections quickly
Keep it DRY: Don't repeat information the AI already knows from training
Version it: Add a version header so you know which version of the skill file is in use
SKILL.md Structure Template
---
name: [skill-name]
version: 1.0.0
description: [One-line description of what this skill covers]
---
# [Skill Name] Quick Reference
## Overview
[2-3 sentences describing what this skill handles and when to use it]
## When to Use This Skill
- [Trigger condition 1]
- [Trigger condition 2]
- Do NOT use for: [anti-patterns]
## Setup & Prerequisites
[Installation commands, dependencies, environment setup]
## Core Patterns
### [Pattern 1 Name]
[Explanation + code example]
### [Pattern 2 Name]
[Explanation + code example]
## Critical Rules
- ALWAYS: [Rule that must always be followed]
- NEVER: [Rule that must never be violated]
- IMPORTANT: [High-priority consideration]
## Common Mistakes to Avoid
- [Mistake 1]: [Why it's wrong + correct alternative]
- [Mistake 2]: [Why it's wrong + correct alternative]
## Output Format
[Specify exactly how output should be structured]
## Examples
### Good Example
[Correct output example]
### Bad Example (Do Not Do This)
[Incorrect example + explanation of why it's wrong]
|
8. A Structured Format for Context Engineering
Below is a production-ready template for structuring a complete context engineering architecture. This can be used as a starting point for any AI-powered product or workflow.
The Five-Layer Context Architecture
Layer
| Contents & Purpose
|
Layer 1: Identity
| System prompt defining role, personality, expertise domain, and absolute behavioral rules
|
Layer 2: Knowledge
| Injected documents, APIs, schemas, glossaries, and domain-specific reference material
|
Layer 3: Skills
| SKILL.md files for each task type the AI will perform
|
Layer 4: Memory
| Conversation history, user preferences, session state, and cross-session memory
|
Layer 5: Tools
| Function/tool definitions for actions the AI can take in the real world
|
Context Engineering System Prompt Template
# IDENTITY
You are [Expert Role] specializing in [Domain].
Your communication style is [tone: formal/technical/concise].
You are operating within [Product/System Name].
# CORE RULES
ALWAYS:
- [Non-negotiable behavior 1]
- [Non-negotiable behavior 2]
NEVER:
- [Forbidden behavior 1]
- [Forbidden behavior 2]
# KNOWLEDGE CONTEXT
[Inject relevant documents, schemas, APIs here]
# ACTIVE SKILL FILES
[Reference which SKILL.md files are currently active]
# CURRENT SESSION STATE
Project: [Project Name]
User: [User role/level]
Date: [Current date]
Status: [Current workflow stage]
# OUTPUT EXPECTATIONS
Format: [Expected output structure]
Length: [Guidance on response length]
Language: [Human language + coding language if relevant]
|
9. An Example of Context Engineering (End-to-End)
This example demonstrates how to apply context engineering for a real project: a Biomedical Multi-Agent Research Assistant an AI system that automates literature reviews on PubMed.
Project: Biomedical Research Assistant
Layer 1: System Prompt (Identity)
You are a biomedical AI research assistant embedded in a multi-agent pipeline.
Your specialization is automating PubMed literature reviews for medical researchers.
You communicate in precise, academic English appropriate for a scientific audience.
You operate as part of a LangGraph multi-agent architecture.
ALWAYS:
- Cite PubMed IDs (PMIDs) for every referenced paper
- Flag low-quality evidence with an explicit warning
- Use PICO format when structuring research questions
NEVER:
- Generate medical advice or treatment recommendations
- Claim confidence in findings not supported by cited evidence
|
Layer 2: Injected Knowledge
# PUBMED API REFERENCE
Base URL: https://eutils.ncbi.nlm.nih.gov/entrez/eutils/
Search endpoint: esearch.fcgi?db=pubmed&term=[query]&retmax=20
Fetch endpoint: efetch.fcgi?db=pubmed&id=[pmid]&rettype=abstract
# EVIDENCE GRADING SCHEMA
Grade A: Systematic reviews and RCTs with low bias risk
Grade B: Cohort studies and case-control studies
Grade C: Case reports, expert opinion, editorials
|
Layer 3: Active Skill File
# SKILL: literature-review.md
ALWAYS structure summaries with: Background, Methods, Results, Conclusion
NEVER summarize papers not retrieved from the PubMed API call
Use PRISMA reporting standards for systematic reviews
Flag papers older than 5 years as 'potentially outdated'
|
Layer 4: Session State
Research Query: 'Efficacy of mRNA vaccines in immunocompromised patients'
Papers Retrieved: 12
Current Agent: Evidence Builder
Next Agent: HITL Email Collector
User: Dr. Sarah Ahmed, Immunology Research Team
|
Result: With this context architecture, every AI agent in the pipeline operates with full awareness of the domain, the data, the rules, and the current workflow state producing consistent, reliable, citation-backed research summaries every time.
|
Section 4 Product Documentation: MVP, PRD, SRS, SDS
|
10. What Is MVP (Minimum Viable Product)?
A Minimum Viable Product (MVP) is the smallest, simplest version of a product that delivers enough value to satisfy early adopters, validate a core hypothesis, and generate actionable feedback with the minimum investment of time, money, and engineering resources.
The MVP concept was popularized by Eric Ries in The Lean Startup. The key insight is that most product assumptions are wrong. Building a full product before validating assumptions wastes enormous resources. An MVP lets you test your riskiest assumptions quickly and cheaply.
MVP vs Full Product
Aspect
| MVP vs Full Product
|
Features
| Only the single most critical user journey nothing else
|
Quality
| Good enough to use and learn from, not polished to perfection
|
Time to Build
| Days to weeks, not months to years
|
Purpose
| Validate a hypothesis, gather real user feedback
|
Audience
| Early adopters who tolerate rough edges in exchange for early access
|
Success Metric
| Did users engage with the core feature? Would they pay? Would they return?
|
MVP in AI Product Development
When building AI-powered applications, the MVP approach is especially powerful because:
AI capabilities can be rapidly prototyped with existing APIs (OpenAI, Anthropic, Google Gemini)
The core value proposition can be tested without building a full production backend
User feedback on AI outputs is immediately available and highly informative
The context engineering (system prompts, skill files) can be iteratively refined based on real usage
MVP Rule: If you build it and users don't use it, you have wasted the investment. Build the smallest thing that tests whether users actually want what you think they want.
|
11. What Is PRD (Product Requirements Document)?
A Product Requirements Document (PRD) is a formal document that defines what a product (or a specific feature) must do, from the user's and business's perspective. It is the "contract" between the product team (who defines what to build) and the engineering team (who builds it).
The PRD answers: What are we building? Who is it for? What problems does it solve? What must it do? What does success look like?
Standard PRD Structure
Executive Summary: One-paragraph overview of the product and its purpose
Problem Statement: The specific user problem being solved, with evidence
Goals & Success Metrics: Quantified business and user outcomes (KPIs, OKRs)
User Personas: Who the users are, what they need, what frustrates them
Use Cases & User Stories: 'As a [user], I want to [action] so that [benefit]'
Functional Requirements: What the system must do (features, behaviors, workflows)
Non-Functional Requirements: Performance, security, accessibility, scalability
Out of Scope: Explicit list of features NOT included in this version
Dependencies & Risks: External dependencies, technical risks, open questions
Timeline: Milestones, release plan, and prioritized feature roadmap
PRD in the Context of AI Products
For AI-powered products, the PRD should additionally specify:
Which AI model(s) will be used and why
Acceptable accuracy/quality thresholds for AI outputs
Human-in-the-loop (HITL) requirements when a human must review AI outputs
Failure modes what happens when the AI gives a wrong or low-confidence answer
Data requirements what training or fine-tuning data is needed
12. What Is SRS and SDS?
SRS Software Requirements Specification
An SRS is a comprehensive technical document that precisely specifies all the requirements of a software system from the software engineering perspective. If a PRD says 'what' the product does, the SRS says 'exactly how' those requirements are expressed in testable, engineering-actionable terms.
SRS Standard Structure (IEEE 830)
Introduction: Purpose, scope, definitions, acronyms, references
Overall Description: Product perspective, functions, user characteristics, constraints
Specific Requirements: Detailed functional requirements with input/output specifications
External Interface Requirements: UI, hardware, software, and communication interfaces
System Attributes: Reliability, availability, security, maintainability requirements
Other Requirements: Legal, regulatory, and compliance requirements
SDS Software Design Specification
An SDS (also called Software Design Document / SDD) describes how the software will be built to meet the requirements stated in the SRS. It bridges the gap between requirements and implementation. Where the SRS says 'what', the SDS says 'how it will be designed'.
SDS Standard Structure
System Architecture: High-level architecture diagram and rationale
Component Design: Detailed specification of each module, class, or service
Data Design: Database schema, entity-relationship diagrams, data flow
Interface Design: API contracts, UI wireframes, integration specifications
Algorithm Design: Pseudocode or flowcharts for complex business logic
Security Design: Authentication, authorization, encryption, threat model
Performance Design: Caching strategy, load handling, optimization decisions
Document
| SRS vs SDS at a Glance
|
Focus
| SRS: What the system must do. SDS: How it will be designed.
|
Audience
| SRS: All stakeholders (business, product, engineering, QA). SDS: Engineering team.
|
Written By
| SRS: Business analysts + product managers + engineers. SDS: Software architects + senior engineers.
|
When Created
| SRS: Before design begins. SDS: After SRS is approved, before coding.
|
Content Type
| SRS: Requirements, constraints, behaviors. SDS: Diagrams, schemas, APIs, algorithms.
|
13. How These Documents Help Build a Product
MVP, PRD, SRS, and SDS are not bureaucratic formalities they are engineering tools that dramatically reduce waste, miscommunication, and rework. Here is how each one contributes to a successful product build.
The Product Development Workflow
Idea Validation (MVP): Test the core hypothesis with real users before investing in a full build
Requirements Definition (PRD): Align the entire team on what success looks like and what will be built
Technical Requirements (SRS): Translate product requirements into precise, testable engineering specifications
System Design (SDS): Design the architecture, database, and APIs before writing a single line of code
Implementation: Developers build against the SDS, QA tests against the SRS
Validation: Verify the delivered product matches the PRD's success metrics
Benefits by Document
Document
| Key Benefits for the Development Team
|
MVP
| Reduces wasted investment; validates real user demand; generates feedback for PRD
|
PRD
| Prevents scope creep; aligns business and engineering; defines 'done'
|
SRS
| Enables precise QA testing; prevents ambiguous interpretations; supports compliance/audits
|
SDS
| Enables parallel development; documents architectural decisions; onboards new engineers
|
The documents form a chain of accountability: MVP proves the idea → PRD defines the vision → SRS specifies the requirements → SDS designs the solution → Code implements it → Tests verify it. Skipping any layer creates hidden risks that surface as expensive bugs or missed requirements later.
|
Section 5 AI-Assisted Prompt Generation & Anti-Patterns
|
14. How to Generate High-Level Prompts Using AI
One of the most powerful meta-skills in AI-assisted development is using AI to help you write better prompts. This is called "meta-prompting" or "prompt generation". The idea is simple: describe your goal to the AI, and ask it to generate the optimal prompt for achieving that goal.
The Meta-Prompting Workflow
Describe your goal at a high level to the AI
Ask the AI to generate the best possible prompt for achieving that goal
Review and refine the generated prompt
Use the refined prompt for your actual task
Meta-Prompt Template
I need your help generating a high-quality prompt for the following task:
TASK: [Describe what you want to accomplish in plain language]
CONTEXT: [Your tech stack, domain, constraints, audience]
OUTPUT FORMAT: [How you want the final output structured]
CONSTRAINTS: [Things the prompt must include or avoid]
Please generate a complete, production-quality prompt that:
1. Assigns the AI an appropriate expert role
2. Provides sufficient context for accurate output
3. Specifies the exact output format required
4. Includes relevant constraints and success criteria
5. Is clear enough that no clarification questions are needed
|
AI Tools for Prompt Generation
Claude (Anthropic): Excellent for generating structured, context-rich prompts for complex tasks
ChatGPT (OpenAI): Strong at generating few-shot examples and conversational prompts
Gemini (Google): Effective for technical prompts involving code and structured data
PromptPerfect: Specialized tool for prompt optimization and testing
LangChain Hub: Community repository of production-tested prompt templates
Prompt Chain Generation
For complex multi-step tasks, ask the AI to generate a chain of prompts rather than a single one:
Generate a prompt chain for: 'Automated code review of a Node.js REST API'
The chain should include:
1. A prompt for static analysis of code style and conventions
2. A prompt for security vulnerability identification
3. A prompt for performance bottleneck detection
4. A prompt for generating a structured review report
Each prompt should be self-contained and reference outputs from the previous step.
|
Pro Tip: Once you have a high-quality prompt that consistently produces excellent output, save it as a SKILL.md entry. It becomes a reusable asset in your context engineering library.
|
15. What NOT to Do When Writing Prompts
Understanding common prompt anti-patterns is just as important as knowing best practices. These mistakes consistently produce low-quality AI outputs, wasted iterations, and frustrating experiences.
The Critical Anti-Patterns
1. Being Vague or Abstract
WRONG: 'Help me with my code.'  |  RIGHT: 'Review this Express.js route handler for SQL injection vulnerabilities and suggest fixes using parameterized queries.'
|
2. Overloading a Single Prompt
Asking the AI to do too many things at once leads to shallow treatment of all of them. Break complex tasks into smaller, focused prompts.
WRONG: 'Write my app, design the UI, create the database, write the API, and deploy it.'  |  RIGHT: One prompt per layer, in the correct sequence.
|
3. Omitting Format Instructions
Without format instructions, the AI chooses its own format which may not match your toolchain or workflow.
WRONG: 'List the pros and cons.'  |  RIGHT: 'List the pros and cons in a markdown table with columns: Aspect, Pros, Cons.'
|
4. No Role Definition
Generic prompts produce generic answers. Always give the AI an expert identity relevant to your task.
WRONG: 'Explain database indexing.'  |  RIGHT: 'You are a database architect. Explain indexing strategies for a PostgreSQL table with 50M rows to a mid-level developer.'
|
5. Assuming Context the AI Doesn't Have
The AI only knows what you tell it. Referencing 'our system', 'the existing code', or 'as we discussed' without providing that information leads to hallucinated answers.
Always include relevant code, schemas, or documents in the prompt
Do not use pronouns that refer to context outside the conversation
Be explicit never assume the AI knows your project setup
6. Not Specifying Constraints
Unconstrained prompts give the AI too much freedom, often leading to answers that are correct in general but wrong for your specific situation.
WRONG: 'Write a search function.'  |  RIGHT: 'Write a search function using Elasticsearch not SQL LIKE queries that handles partial matches and is case-insensitive.'
|
7. Asking Leading Questions
Leading questions bias the AI toward confirming your assumptions rather than providing honest analysis.
WRONG: 'React is better than Vue, right?'  |  RIGHT: 'Compare React and Vue.js objectively for a team of 3 engineers building a dashboard app with complex state management.'
|
8. Ignoring the Token Budget
Very long prompts consume large portions of the context window, leaving less room for the AI's response. And very long prompts with irrelevant content reduce the AI's attention on what matters.
Include only the most relevant context not everything you have
Summarize lengthy documents rather than pasting them in full
For very long tasks, use multiple focused prompts rather than one massive one
9. Not Iterating on Failures
When a prompt produces a poor result, most users just try again with a slightly different wording. Instead, analyze why it failed:
Was the role definition missing or wrong?
Was there ambiguous language in the task description?
Was the output format not specified?
Was the context incomplete?
Document your failures. Every time a prompt produces a bad output, ask why then add the fix to your SKILL.md. Over time, your SKILL.md files become highly effective because they capture all the edge cases that tripped you up.
|
Prompt Anti-Pattern Summary
Anti-Pattern
| Why It Fails & How to Fix It
|
Too vague
| AI fills gaps with generic content. Fix: Add specific context, tech stack, exact task.
|
Overloaded
| AI gives shallow coverage of all tasks. Fix: One clear task per prompt.
|
No format spec
| Output doesn't match your workflow. Fix: Always specify format (JSON, table, code, prose).
|
No role defined
| Generic expert-level output missing. Fix: Always assign an expert persona.
|
Assumed context
| AI hallucinates missing information. Fix: Include all relevant context explicitly.
|
No constraints
| Output is correct but not for your case. Fix: Specify libraries, versions, and patterns to exclude.
|
Leading questions
| Biases AI toward confirming your assumptions. Fix: Ask objective, open-ended comparisons.
|
Ignoring token budget
| Reduces response quality and attention. Fix: Keep context relevant, summarize docs, split tasks.
|
Not iterating on failures
| Wastes time on repeated bad inputs. Fix: Analyze the failure point and update instructions/SKILL.md.
|
