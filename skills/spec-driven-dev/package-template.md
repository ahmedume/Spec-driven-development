# Spec Package Template

This file defines the exact structure and writing guidance for all 7 sections of the spec-driven 
context package. Read this before generating any output.

---

## How to Output the Package

Each section begins with a clearly visible file separator:

```
# ===== FILE: FILENAME.md =====
```

Then the full content of that file follows. The user will manually copy each section.

---

## Section 1: Enhanced Prompt

**Tag:** `# ===== FILE: ENHANCED-PROMPT.md =====`

**Purpose:** A rewritten, production-grade version of the user's original request.  
**Based on:** The CRISP Framework — Context, Role, Intent, Specifics, Parameters

**Writing Guide:**

- Start with a **Role Assignment** paragraph ("You are a senior software engineer...")
- State the **Project Context** — what it is, who uses it, why it exists
- Define the **Exact Task** with action verbs
- List the **Tech Stack** explicitly
- Specify **Output Format** expectations
- Set **Constraints** (what NOT to do, what libraries NOT to use)
- End with **Success Criteria** — how we know it is done correctly

**Length:** 300–600 words. This is the most important section. Do not make it short.

**Template:**
```
You are a senior [role: e.g., full-stack engineer, backend architect, AI engineer] with deep expertise 
in [tech stack]. You are building [project name], a [one-line description].

## Project Context
[2–3 sentences on what this is, who uses it, the problem it solves]

## Your Task
[Precise description of what to build in this session. Use action verbs: implement, design, create, wire up]

## Tech Stack
- Frontend: [e.g., Next.js 15, TypeScript, Tailwind CSS]
- Backend: [e.g., FastAPI, Python 3.12]
- Database: [e.g., PostgreSQL via Prisma / SQLAlchemy]
- Auth: [e.g., Clerk / NextAuth / JWT]
- AI Layer: [if applicable]
- Package Manager: [uv for Python / pnpm for Node]
- Deployment: [e.g., Vercel + Railway]

## Output Requirements
[What files should be created, what endpoints should exist, what UI screens should work]

## Constraints
- NEVER use [forbidden library/pattern]
- ALWAYS follow [specific convention]
- Do not implement [out-of-scope feature]

## Success Criteria
- [Specific, testable condition 1]
- [Specific, testable condition 2]
- [Specific, testable condition 3]
```

---

## Section 2: SPEC.md

**Tag:** `# ===== FILE: SPEC.md =====`

**Purpose:** The blueprint — WHAT we are building. No implementation logic here.

**Template:**
```markdown
# Project Specification: [Project Name]

## 1. Overview
- **Project Name:** 
- **One-Line Description:** 
- **Goal:** 
- **Target Users:** 
- **Version:** 1.0.0 (MVP)

---

## 2. Problem Statement
[What problem does this solve? Why does it need to exist? What is broken without it?]

---

## 3. Core Features (MVP — MUST HAVE)

### Feature 1: [Name]
- **Description:** 
- **User Flow:** [step-by-step how user interacts]
- **Inputs:** 
- **Outputs:** 
- **Business Rules:** 

### Feature 2: [Name]
[repeat structure]

### Feature 3: [Name]
[repeat structure]

---

## 4. End-to-End User Flow

1. User arrives at [entry point]
2. User [action]
3. System [response]
4. User [next action]
5. [continue until task complete]

---

## 5. System Behavior (Logic Rules)

- Rule 1: [concrete behavior rule]
- Rule 2: 
- Rule 3: 

**Edge Cases:**
- If [invalid input]: [what happens]
- If [API failure]: [what happens]
- If [unauthenticated user]: [what happens]

---

## 6. Data Model (Entities)

### Entity: [Name]
| Field | Type | Required | Notes |
|-------|------|----------|-------|
| id | UUID | Yes | Primary key |
| [field] | [type] | [yes/no] | [notes] |

### Entity: [Name]
[repeat]

---

## 7. UI Screens (if frontend)

### Screen 1: [Name]
- **Route:** 
- **Purpose:** 
- **Key Components:** 
- **Actions available:** 
- **States:** loading / error / empty / populated

### Screen 2: [Name]
[repeat]

---

## 8. Constraints
- Tech constraints: 
- Performance limits: 
- Security rules: 
- Libraries NOT allowed: 

---

## 9. Out of Scope (this version)
- [Feature we are NOT building]
- [Feature we are NOT building]

---

## 10. Future Improvements (V2)
- [Nice-to-have for later]
- [Scaling ideas]
```

---

## Section 3: TECH_STACK.md

**Tag:** `# ===== FILE: TECH_STACK.md =====`

**Purpose:** All technologies, frameworks, versions, and rationale for each choice. A quick-reference for the AI agent to know exactly what tools are available.

**Template:**
```markdown
# Technology Stack: [Project Name]

| Technology | Version | Purpose |
|------------|---------|---------|
| **[Framework/Lang]** | [version] | [why this is used] |
| **[Library]** | [version] | [what it handles] |
| **[Database]** | [version] | [what it stores] |

## Runtime Requirements

| Resource | Minimum | Recommended |
|----------|---------|-------------|
| RAM | [size] | [size] |
| Disk | [size] | [size] |

## Dependency Graph
```
[ASCII diagram showing how technologies connect]
```

## Why These Choices

| Decision | Rationale |
|----------|-----------|
| [Technology] | [Why chosen over alternatives] |
```

---

## Section 4: BUILD_PLAN.md

**Tag:** `# ===== FILE: BUILD_PLAN.md =====`

**Purpose:** The divide-and-conquer build strategy. This file breaks the entire project into sequential, independent phases. Each phase has clear boundaries, dependencies, a concrete deliverable, and a verification step. The AI agent follows this plan phase by phase, never jumping ahead.

**Writing Guide:**
- Split the build into 4–8 phases
- Each phase must produce something testable/verifiable
- Phase 1 is always scaffolding (structure, config, empty files)
- The last phase is always integration + validation
- Phases should be ordered by dependency (do not build what depends on X until X exists)
- State explicitly what is OUT OF SCOPE for each phase

**Template:**
```markdown
# Build Plan: [Project Name]

## Strategy

Divide and conquer. Each phase is self-contained with a clear deliverable. 
Complete each phase fully before moving to the next. Never jump ahead.

---

## Phase 1: [Name]
**Goal:** [one sentence]
**Depends on:** Nothing

**Tasks:**
1. [Specific action 1]
2. [Specific action 2]
3. [Specific action 3]

**Deliverable:** [What exists and is verifiable after this phase]

**Out of scope:** [What this phase explicitly does NOT do]

---

## Phase 2: [Name]
**Goal:** [one sentence]
**Depends on:** Phase 1

**Tasks:**
1. [Specific action 1]
2. [Specific action 2]

**Deliverable:** [What exists and is verifiable]

**Out of scope:** [What this phase explicitly does NOT do]

---

## Phase 3: [Name]
**Goal:** [one sentence]
**Depends on:** Phase 2

**Tasks:**
1. [Specific action 1]
2. [Specific action 2]

**Deliverable:** [What exists and is verifiable]

---

## Phase 4: [Name]
**Goal:** [one sentence]
**Depends on:** Phase 3

**Tasks:**
1. [Specific action 1]
2. [Specific action 2]

**Deliverable:** [What exists and is verifiable]

---

## Phase 5: [Integration & Polish]
**Goal:** Wire everything together, handle edge cases, validate.
**Depends on:** All previous phases

**Tasks:**
1. Integration testing across all phases
2. Edge case handling
3. Final validation against SRS.md

**Deliverable:** Complete, working [project] that meets all SRS requirements.
```

---

## Section 5: PRD.md

**Tag:** `# ===== FILE: PRD.md =====`

**Purpose:** Product requirements — who, why, what success looks like. The PM's contract.

**Template:**
```markdown
# Product Requirements Document: [Project Name]

## Executive Summary
[One paragraph: what this product is, who it serves, what it does, and why it matters]

---

## Problem Statement
[The specific user problem. Include evidence or pain points where possible.]

---

## Goals & Success Metrics

| Goal | Metric | Target |
|------|--------|--------|
| [User goal] | [How measured] | [Threshold] |
| [Business goal] | [How measured] | [Threshold] |

---

## User Personas

### Persona 1: [Name / Role]
- **Who they are:** 
- **What they need:** 
- **What frustrates them:** 
- **Technical level:** 

### Persona 2: [Name / Role]
[repeat]

---

## User Stories

- As a [user type], I want to [action] so that [benefit].
- As a [user type], I want to [action] so that [benefit].
- As a [user type], I want to [action] so that [benefit].

---

## Functional Requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-01 | [The system shall...] | Must Have |
| FR-02 | [The system shall...] | Must Have |
| FR-03 | [The system shall...] | Should Have |
| FR-04 | [The system shall...] | Nice to Have |

---

## Non-Functional Requirements

| Category | Requirement |
|----------|-------------|
| Performance | [e.g., Page load < 2s, API response < 500ms] |
| Security | [e.g., All endpoints require auth, passwords hashed with bcrypt] |
| Scalability | [e.g., Must handle 1000 concurrent users] |
| Accessibility | [e.g., WCAG 2.1 AA compliance] |
| Reliability | [e.g., 99.5% uptime] |

---

## Out of Scope
- [Feature explicitly excluded from this version]
- [Feature explicitly excluded from this version]

---

## Dependencies & Risks

| Item | Type | Impact | Mitigation |
|------|------|--------|------------|
| [External API] | Dependency | High | [Fallback plan] |
| [Third-party service] | Risk | Medium | [Alternative] |

---

## Timeline & Milestones

| Milestone | Deliverable | Target |
|-----------|-------------|--------|
| M1 | [e.g., Auth + DB setup] | Week 1 |
| M2 | [e.g., Core features] | Week 2 |
| M3 | [e.g., UI polish + testing] | Week 3 |
```

---

## Section 6: SRS.md

**Tag:** `# ===== FILE: SRS.md =====`

**Purpose:** The engineering contract — exact, testable behavioral rules. Based on IEEE 830.

**Template:**
```markdown
# Software Requirements Specification: [Project Name]

**Version:** 1.0.0  
**Standard:** IEEE 830  
**Status:** Draft

---

## 1. Introduction

### 1.1 Purpose
[What this document specifies and who should read it]

### 1.2 Scope
[Boundaries of the system being specified]

### 1.3 Definitions & Acronyms
| Term | Definition |
|------|------------|
| [Term] | [Definition] |

---

## 2. Overall Description

### 2.1 Product Perspective
[How this system fits into a larger context or replaces an existing system]

### 2.2 User Characteristics
[Technical level, expected usage patterns]

### 2.3 Assumptions & Dependencies
- [Assumption 1]
- [Assumption 2]

---

## 3. Functional Requirements

### FR-01: [Feature Name]
- **Input:** [Exact input — field names, types, validation rules]
- **Processing:** [What the system does with that input, step by step]
- **Output:** [Exact output — response shape, status codes]
- **Validation Rules:**
  - [Rule 1: e.g., email must be valid format]
  - [Rule 2: e.g., password must be min 8 chars]
- **Error Cases:**
  - If [condition]: return [error code] with message "[message]"
  - If [condition]: return [error code] with message "[message]"

### FR-02: [Feature Name]
[repeat structure]

---

## 4. External Interface Requirements

### 4.1 User Interface
- [UI behavior rules — e.g., form must show inline validation errors]
- [Accessibility requirements]

### 4.2 API Interface
- All API responses shall follow the structure: `{ success, data, error }`
- All timestamps shall be ISO 8601 format
- All IDs shall be UUIDs

### 4.3 Database Interface
- [ORM rules, transaction requirements]

---

## 5. System Attributes

### 5.1 Security
- [Authentication rule: e.g., all routes except /auth/* require valid JWT]
- [Data rule: e.g., passwords hashed with bcrypt, cost factor 12]
- [Input rule: e.g., all user inputs sanitized before DB write]

### 5.2 Performance
- [e.g., List endpoints must paginate, max 50 records per page]
- [e.g., Image uploads max 5MB]

### 5.3 Reliability
- [e.g., All DB operations wrapped in try/except with rollback]
- [e.g., Failed background tasks logged and retried once]

### 5.4 Maintainability
- [e.g., All environment variables via .env, never hardcoded]
- [e.g., Each module has a single responsibility]

---

## 6. Validation & Testing Criteria

| Requirement | Test Case | Expected Result |
|-------------|-----------|-----------------|
| FR-01 | [Submit valid form] | [Returns 201 + data] |
| FR-01 | [Submit empty form] | [Returns 422 + validation errors] |
| FR-02 | [Unauthenticated request] | [Returns 401] |
```

---

## Section 7: SDS.md

**Tag:** `# ===== FILE: SDS.md =====`

**Purpose:** Architecture + implementation plan. HOW it is built. Devs code from this.

**Template:**
```markdown
# Software Design Specification: [Project Name]

**Version:** 1.0.0

---

## 1. System Architecture

### Architecture Pattern
[e.g., REST API + SPA, Monolithic, Microservices — and rationale]

### High-Level Architecture
```
[ASCII diagram or description of components and how they connect]
e.g.:
[Next.js Frontend] → [FastAPI Backend] → [PostgreSQL]
                                       → [Redis Cache]
                   → [Clerk Auth]
```

---

## 2. Folder Structure

### Frontend (if applicable)
```
src/
├── app/
│   ├── (auth)/
│   │   ├── login/page.tsx
│   │   └── register/page.tsx
│   ├── (dashboard)/
│   │   └── [feature]/page.tsx
│   └── layout.tsx
├── components/
│   ├── ui/          # shadcn components
│   └── [feature]/   # feature-specific components
├── lib/
│   ├── api.ts       # API client
│   └── utils.ts
└── types/
    └── index.ts
```

### Backend (if applicable)
```
app/
├── main.py
├── core/
│   ├── config.py    # env vars via pydantic-settings
│   └── database.py  # SQLAlchemy setup
├── models/
│   └── [entity].py
├── schemas/
│   └── [entity].py  # Pydantic schemas
├── routers/
│   └── [feature].py
└── services/
    └── [feature].py
```

---

## 3. Database Schema

### Table: [table_name]
```sql
CREATE TABLE [table_name] (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  [field]     [TYPE] NOT NULL,
  [field]     [TYPE],
  created_at  TIMESTAMP DEFAULT NOW(),
  updated_at  TIMESTAMP DEFAULT NOW()
);
```

### Table: [table_name]
[repeat]

### Relationships
- [Entity A] has many [Entity B] via [foreign key]
- [Entity B] belongs to [Entity A]

---

## 4. API Routes

### Authentication Routes
| Method | Route | Auth Required | Description |
|--------|-------|---------------|-------------|
| POST | /auth/register | No | Create new user |
| POST | /auth/login | No | Return JWT |
| POST | /auth/logout | Yes | Invalidate token |

### [Feature] Routes
| Method | Route | Auth Required | Description |
|--------|-------|---------------|-------------|
| GET | /[resource] | Yes | List all (paginated) |
| POST | /[resource] | Yes | Create new |
| GET | /[resource]/{id} | Yes | Get by ID |
| PUT | /[resource]/{id} | Yes | Update |
| DELETE | /[resource]/{id} | Yes | Delete |

---

## 5. Request / Response Schemas

### POST /[resource]
**Request Body:**
```json
{
  "field": "string (required, max 255 chars)",
  "field": "string (optional)"
}
```

**Success Response (201):**
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "field": "value",
    "created_at": "ISO 8601"
  }
}
```

**Error Response (422):**
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "field is required"
  }
}
```

---

## 6. Key Algorithms & Business Logic

### Algorithm: [Name]
```pseudocode
FUNCTION [name](inputs):
  1. Validate inputs
  2. [Step 2]
  3. [Step 3]
  RETURN result
```

---

## 7. Security Design

- **Authentication:** [Method — e.g., JWT with 24h expiry, refresh token in httpOnly cookie]
- **Authorization:** [Role-based? Resource-based? Rules here]
- **Password Hashing:** bcrypt, cost factor 12
- **Input Validation:** Pydantic (backend) + Zod (frontend)
- **SQL Injection:** SQLAlchemy ORM only — no raw queries
- **CORS:** Restrict to [allowed origins]

---

## 8. Environment Variables

```env
# App
APP_ENV=development
SECRET_KEY=

# Database
DATABASE_URL=postgresql://user:pass@localhost:5432/dbname

# Auth
AUTH_SECRET=

# External APIs
[API_NAME]_KEY=
```

---

## 9. Performance Design

- Pagination: All list endpoints use cursor-based or offset pagination
- Caching: [What gets cached, for how long]
- Indexing: [Which DB columns get indexes and why]
```

---

## Section 8: SKILL.md (Project-Level AI Rules)

**Tag:** `# ===== FILE: SKILL.md =====`

**Purpose:** Rules for the AI coding agent on THIS project. Not generic — project-specific.

**Template:**
```markdown
# AI Coding Agent Rules: [Project Name]

**Version:** 1.0.0  
**Stack:** [stack summary]

---

## Role

You are a senior [role] building [project name]. You follow these rules at all times.

---

## ALWAYS

1. Use [package manager] — NEVER use pip install or npm install directly without lock files
2. All environment variables via `.env` — NEVER hardcode credentials or keys
3. Follow the folder structure defined in SDS.md exactly
4. Write TypeScript with strict mode — no `any` types
5. All API responses follow `{ success, data, error }` structure
6. Validate all user input with [Pydantic / Zod] before processing
7. Write meaningful variable and function names — no `x`, `temp`, `data2`
8. Every async operation has error handling — no unhandled promise rejections
9. Add a comment block at the top of every new file: purpose, exports, dependencies
10. Follow existing naming conventions — check nearby files before naming anything

---

## NEVER

1. NEVER write raw SQL — use the ORM
2. NEVER store secrets in source code or commit .env files
3. NEVER implement features not specified in SPEC.md without asking
4. NEVER use `console.log` in production code — use the logger
5. NEVER ignore TypeScript errors with `// @ts-ignore`
6. NEVER create a file larger than 300 lines — split into modules
7. NEVER use `any` as a type — define proper interfaces
8. NEVER skip error handling for DB operations
9. NEVER mix business logic with route handlers — use service layer
10. NEVER use deprecated APIs from the framework

---

## Code Patterns

### API Route Pattern (Backend)
```python
@router.post("/resource", response_model=ResourceResponse, status_code=201)
async def create_resource(
    payload: ResourceCreate,
    db: AsyncSession = Depends(get_db),
    current_user: User = Depends(get_current_user)
):
    result = await resource_service.create(db, payload, current_user.id)
    return {"success": True, "data": result}
```

### Component Pattern (Frontend)
```typescript
// Purpose: [what this component does]
// Props: [what it receives]
interface Props {
  [prop]: [type];
}

export function ComponentName({ prop }: Props) {
  // state at top
  // effects after state
  // handlers before return
  // return JSX last
}
```

---

## File Naming Conventions
- Backend routes: `[feature]_router.py`
- Backend models: `[entity]_model.py`
- Frontend components: `PascalCase.tsx`
- Frontend hooks: `use[Name].ts`
- Frontend pages: `page.tsx` (Next.js App Router)

---

## Commit Message Format
```
[type]: [short description]

Types: feat | fix | refactor | docs | test | chore
Example: feat: add user authentication endpoint
```
```

---

## Section 9: PROMPTS.md

**Tag:** `# ===== FILE: PROMPTS.md =====`

**Purpose:** Reusable execution prompts — one per major feature. Copy-paste into AI coding tool.

**Template:**
```markdown
# Execution Prompts: [Project Name]

These are ready-to-use prompts for each major task. Copy the relevant prompt into your 
AI coding tool. Always ensure SPEC.md, SRS.md, SDS.md, and SKILL.md are in context first.

---

## SETUP PROMPT (Run This First)
```
Read all .md files in this folder: SPEC.md, PRD.md, SRS.md, SDS.md, SKILL.md.
Confirm you have read them by summarizing:
1. What we are building
2. The tech stack
3. The folder structure
4. The 3 most important SKILL.md rules
Do not write any code until I say "proceed".
```

---

## PROMPT 1: Project Scaffolding
```
Using SDS.md as your guide, scaffold the complete project structure.
Create all folders and empty files with the correct names.
For each file, add a comment block at the top with: purpose, exports, dependencies.
Do NOT implement logic yet — structure only.
Follow SKILL.md rules at all times.
```

---

## PROMPT 2: Database & Models
```
Implement the database schema from SDS.md section 3.
Create all SQLAlchemy models (or Prisma schema).
Include all fields, types, constraints, and relationships exactly as specified.
Add indexes to columns marked in SDS.md.
Do not modify any field names or types.
```

---

## PROMPT 3: [Feature Name] — Backend
```
Implement the [feature] backend from SRS.md FR-0X.
Create:
- Router: [route file path]
- Service: [service file path]  
- Schema: [schema file path]

Follow these rules from SRS.md exactly:
- [Specific validation rule]
- [Specific error behavior]
- [Specific response format]

Follow SKILL.md code patterns for route and service structure.
```

---

## PROMPT 4: [Feature Name] — Frontend
```
Implement the [feature] UI from SPEC.md Screen [N].
Create:
- Page: [page path]
- Components: [component paths]

The UI must handle all states: loading, error, empty, populated.
Use [component library] components where possible.
Wire up to the API using [API client path].
```

---

## PROMPT 5: Auth
```
Implement authentication as specified in SRS.md section 3 (FR-0X).
Setup [auth provider / JWT].
Protect all routes that require authentication per SDS.md API Routes table.
Redirect unauthenticated users to /login.
```

---

## PROMPT 6: Testing
```
Write tests for [feature] following SRS.md section 6 (Validation & Testing Criteria).
Cover:
- Happy path (valid input)
- Validation errors (invalid input)
- Auth errors (unauthenticated)
- Not found (invalid ID)
Use [test framework].
```

---

## PROMPT 7: Debug / Fix
```
The following is not working as specified in SRS.md:
[Describe the bug]

Expected behavior per SRS.md FR-0X:
[What SRS says should happen]

Actual behavior:
[What is happening]

Fix this without changing any other behavior.
```

---

## PROMPT 8: Review & Spec Check
```
Review the current implementation against SPEC.md and SRS.md.
List any features that are:
1. Missing (not yet implemented)
2. Incorrectly implemented (behavior doesn't match SRS)
3. Out of scope (implemented but not in SPEC)

Do not fix anything yet — just report.
```
```
