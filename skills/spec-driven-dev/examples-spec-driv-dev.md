# Worked Example: To-Do App Spec Package

This is what the output looks like when a user says "Build me a to-do app."
Use this to calibrate depth, specificity, and style.

---

# ===== FILE: ENHANCED-PROMPT.md =====

You are a senior full-stack engineer with deep expertise in Next.js 15, TypeScript, FastAPI, 
PostgreSQL, and Tailwind CSS. You are building **TaskFlow**, a minimal but production-ready 
personal task management application.

## Project Context

TaskFlow is a single-user to-do application that allows a user to create, organize, and track 
personal tasks. It solves the problem of needing a clean, fast, self-hostable alternative to 
bloated task apps. Target users are developers and students who want a simple, no-noise tool 
they fully control.

## Your Task

Implement the complete TaskFlow application from scratch. This includes:
- User authentication (registration + login)
- Full CRUD for tasks (create, read, update, delete)
- Task filtering by status (all, active, completed)
- Mark tasks as complete / incomplete
- A clean, responsive UI with dark mode support

## Tech Stack
- **Frontend:** Next.js 15 (App Router), TypeScript, Tailwind CSS, shadcn/ui
- **Backend:** FastAPI, Python 3.12, SQLAlchemy 2.0 (async)
- **Database:** PostgreSQL 16
- **Auth:** JWT (python-jose), password hashing via bcrypt
- **Package Manager:** uv (Python), pnpm (Node)
- **Deployment:** Docker Compose (local), Vercel (frontend) + Railway (backend)

## Output Requirements

After implementation, the following must exist and work:
- `/register` page — user can create an account
- `/login` page — user can authenticate
- `/` (dashboard) — user sees their task list with filters
- POST /auth/register + POST /auth/login API routes
- GET/POST/PUT/DELETE /tasks API routes (all authenticated)
- Tasks persist in PostgreSQL across sessions

## Constraints
- NEVER use `any` TypeScript type
- NEVER hardcode credentials — all via `.env`
- NEVER use pip directly — use `uv pip` or `uv run`
- Do NOT implement team/collaboration features (out of scope)
- Do NOT use Redux — use React Query for server state

## Success Criteria
- A new user can register, log in, create tasks, mark them done, and delete them
- The page loads in under 2 seconds on localhost
- All API endpoints return `{ success, data, error }` structure
- No TypeScript errors in strict mode
- Environment variables documented in `.env.example`

---

# ===== FILE: SPEC.md =====

# Project Specification: TaskFlow

## 1. Overview
- **Project Name:** TaskFlow
- **One-Line Description:** A minimal, fast, personal task manager for developers and students
- **Goal:** Let a single user manage their tasks with zero friction
- **Target Users:** Individual developers, students — non-collaborative use only
- **Version:** 1.0.0 (MVP)

---

## 2. Problem Statement

Existing task apps (Todoist, Notion, etc.) are bloated, slow, and require cloud accounts. 
Developers want a self-hostable, clean tool they own. TaskFlow solves this with a simple CRUD 
app that is fast, private, and easy to deploy.

---

## 3. Core Features (MVP — MUST HAVE)

### Feature 1: User Authentication
- **Description:** Users register and log in with email + password
- **User Flow:**
  1. User visits /register → fills form → submits
  2. System creates account → redirects to /login
  3. User fills login form → submits
  4. System returns JWT → stores in cookie → redirects to /
- **Inputs:** email (string, valid email), password (string, min 8 chars)
- **Outputs:** JWT token on success, error message on failure
- **Rules:**
  - Email must be unique across users
  - Passwords hashed before storage — never stored in plaintext
  - JWT expires in 24 hours

### Feature 2: Task CRUD
- **Description:** Users can create, view, edit, and delete tasks
- **User Flow:**
  1. User is on dashboard → sees task list
  2. User clicks "Add Task" → types title → presses Enter or clicks Add
  3. Task appears in list instantly
  4. User clicks task to mark complete (toggle)
  5. User clicks delete icon → task removed with confirmation
- **Inputs:** title (string, required, max 255 chars), description (optional, max 1000 chars)
- **Outputs:** Task object with id, title, description, status, created_at
- **Rules:**
  - Title is required — empty tasks cannot be saved
  - Tasks belong to the authenticated user — users cannot see other users' tasks

### Feature 3: Task Filtering
- **Description:** User can filter task list by status
- **User Flow:** User clicks filter tab (All / Active / Completed) → list updates
- **Inputs:** Filter value: "all" | "active" | "completed"
- **Outputs:** Filtered task list
- **Rules:** Filter state persists via URL query param (?filter=active)

---

## 4. End-to-End User Flow

1. User visits the app for the first time → redirected to /login
2. User clicks "Create Account" → goes to /register
3. User fills registration form → submits → account created
4. User logs in → lands on dashboard /
5. Dashboard shows empty state: "No tasks yet. Add one above."
6. User types task title in top input → presses Enter → task appears in list
7. User adds more tasks
8. User clicks checkbox on a task → task marked complete, visually struck through
9. User clicks "Completed" filter → sees only done tasks
10. User clicks delete icon → confirmation dialog → task deleted

---

## 5. System Behavior (Logic Rules)

- Only authenticated users can access the dashboard — others redirect to /login
- A user can only see and modify their own tasks
- Deleting a task is permanent — no recycle bin in V1
- Completing a task does not delete it — it changes status to "completed"
- Filtering happens client-side if < 200 tasks, server-side if > 200 tasks

**Edge Cases:**
- If title is empty string on submit: show inline error "Title is required", do not call API
- If API returns 401: clear stored JWT, redirect to /login
- If API returns 500: show toast "Something went wrong. Try again." — do not crash

---

## 6. Data Model

### Entity: User
| Field | Type | Required | Notes |
|-------|------|----------|-------|
| id | UUID | Yes | Primary key |
| email | VARCHAR(255) | Yes | Unique |
| password_hash | VARCHAR(255) | Yes | bcrypt hash |
| created_at | TIMESTAMP | Yes | Auto-set |

### Entity: Task
| Field | Type | Required | Notes |
|-------|------|----------|-------|
| id | UUID | Yes | Primary key |
| user_id | UUID | Yes | FK → users.id |
| title | VARCHAR(255) | Yes | |
| description | TEXT | No | |
| status | ENUM | Yes | 'active' or 'completed' |
| created_at | TIMESTAMP | Yes | Auto-set |
| updated_at | TIMESTAMP | Yes | Auto-updated |

---

## 7. UI Screens

### Screen 1: Login (/login)
- **Purpose:** Authenticate existing user
- **Components:** EmailInput, PasswordInput, SubmitButton, "Create Account" link
- **States:** idle / submitting / error

### Screen 2: Register (/register)
- **Purpose:** Create new account
- **Components:** EmailInput, PasswordInput, ConfirmPasswordInput, SubmitButton
- **States:** idle / submitting / error (email taken, passwords don't match)

### Screen 3: Dashboard (/)
- **Purpose:** View and manage tasks
- **Components:** AddTaskInput, FilterTabs, TaskList, TaskItem (checkbox + title + delete)
- **States:** loading / empty / populated / error

---

## 8. Constraints
- No team features (invites, shared lists, comments)
- No file attachments
- No due dates in V1
- No mobile app — web only

---

## 9. Out of Scope (V1)
- Due dates and reminders
- Labels / tags
- Subtasks
- Collaboration or sharing
- Mobile app

---

## 10. Future Improvements (V2)
- Due dates with calendar view
- Task labels
- Drag-and-drop ordering
- Email reminders

---

# ===== FILE: PRD.md =====

# Product Requirements Document: TaskFlow

## Executive Summary

TaskFlow is a minimal personal task management web application built for developers and students 
who want a clean, fast, self-hostable alternative to bloated SaaS tools. V1 focuses entirely on 
single-user task CRUD with filtering, delivering core value with no unnecessary complexity.

---

## Problem Statement

Most task management tools are over-engineered for teams, slow to load, or require cloud accounts. 
Individual users — especially developers — want something simple, fast, and fully in their control.

---

## Goals & Success Metrics

| Goal | Metric | Target |
|------|--------|--------|
| Core usability | User can complete full flow (register → add task → complete task) | Without errors |
| Performance | Dashboard load time | < 2s on localhost |
| Reliability | API error rate | < 1% |
| Adoption | User stays active after day 1 | 70% (test metric) |

---

## User Personas

### Persona 1: Dev Student (Primary)
- **Who they are:** CS/AI student, comfort with terminal and self-hosting
- **What they need:** A fast task tracker for study sessions and projects
- **What frustrates them:** Slow apps, required sign-ups to cloud services, ads

### Persona 2: Freelance Developer
- **Who they are:** Solo dev managing personal projects
- **What they need:** Simple to-do without project management overhead
- **What frustrates them:** Feature bloat, team-centric UX

---

## User Stories

- As a user, I want to register with email and password so that I can create an account.
- As a user, I want to add tasks so that I can track what I need to do.
- As a user, I want to mark tasks as complete so that I can see progress.
- As a user, I want to filter tasks by status so that I can focus on active work.
- As a user, I want to delete tasks so that I can remove irrelevant items.

---

## Functional Requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-01 | System shall allow user registration with email and password | Must Have |
| FR-02 | System shall authenticate users and return a JWT | Must Have |
| FR-03 | System shall allow authenticated users to create tasks with title | Must Have |
| FR-04 | System shall allow users to view their own tasks | Must Have |
| FR-05 | System shall allow users to mark tasks complete/incomplete | Must Have |
| FR-06 | System shall allow users to delete tasks | Must Have |
| FR-07 | System shall allow filtering tasks by all/active/completed | Must Have |
| FR-08 | System shall support optional task description | Should Have |
| FR-09 | System shall support dark mode | Nice to Have |

---

## Non-Functional Requirements

| Category | Requirement |
|----------|-------------|
| Performance | Dashboard loads in < 2s; API responds in < 300ms |
| Security | Passwords hashed with bcrypt; JWT required for all task endpoints |
| Scalability | Handles single user with up to 10,000 tasks without degradation |
| Accessibility | Keyboard navigable forms, ARIA labels on interactive elements |

---

## Out of Scope
- Team features, sharing, collaboration
- Due dates, reminders, labels (V2)
- Mobile native app

---

## Dependencies & Risks

| Item | Type | Impact | Mitigation |
|------|------|--------|------------|
| PostgreSQL | Dependency | High | Use Docker for local; Railway for prod |
| JWT expiry management | Risk | Medium | Implement refresh token in V1.1 |

---

# ===== FILE: SRS.md =====

# Software Requirements Specification: TaskFlow

**Version:** 1.0.0 | **Standard:** IEEE 830 | **Status:** Draft

---

## 1. Introduction

### 1.1 Purpose
This document specifies all functional and non-functional requirements for TaskFlow V1. 
It is the engineering contract between design and implementation.

### 1.2 Scope
Covers: user auth, task CRUD, task filtering. Excludes: team features, due dates, labels.

### 1.3 Definitions
| Term | Definition |
|------|------------|
| JWT | JSON Web Token — used for stateless authentication |
| Task Status | Enum value: "active" or "completed" |
| Authenticated Route | API endpoint that requires a valid JWT in Authorization header |

---

## 2. Overall Description

### 2.1 Assumptions
- One user per account; no sharing
- Frontend and backend run as separate services
- PostgreSQL is the only supported database

---

## 3. Functional Requirements

### FR-01: User Registration
- **Input:** `{ email: string, password: string }` via POST /auth/register
- **Processing:**
  1. Validate email format (RFC 5322)
  2. Validate password length >= 8 chars
  3. Check email not already registered
  4. Hash password with bcrypt (cost 12)
  5. Insert user record
  6. Return user object (without password_hash)
- **Output (201):** `{ success: true, data: { id, email, created_at } }`
- **Error Cases:**
  - Invalid email format → 422 `VALIDATION_ERROR`
  - Password < 8 chars → 422 `VALIDATION_ERROR`
  - Email already exists → 409 `EMAIL_TAKEN`

### FR-02: User Login
- **Input:** `{ email: string, password: string }` via POST /auth/login
- **Processing:**
  1. Look up user by email
  2. Compare password with bcrypt hash
  3. Generate JWT (payload: user_id, exp: now + 24h)
  4. Return token
- **Output (200):** `{ success: true, data: { token: string } }`
- **Error Cases:**
  - User not found → 401 `INVALID_CREDENTIALS`
  - Password mismatch → 401 `INVALID_CREDENTIALS` (same message — no enumeration)

### FR-03: Create Task
- **Input:** `{ title: string, description?: string }` via POST /tasks (authenticated)
- **Processing:**
  1. Extract user_id from JWT
  2. Validate title not empty, max 255 chars
  3. Validate description max 1000 chars (if provided)
  4. Insert task with status = "active"
  5. Return created task
- **Output (201):** `{ success: true, data: { id, title, description, status, created_at } }`
- **Error Cases:**
  - Empty title → 422 `VALIDATION_ERROR: title is required`
  - No JWT → 401 `UNAUTHORIZED`

### FR-04: List Tasks
- **Input:** Query param `?filter=all|active|completed` via GET /tasks (authenticated)
- **Processing:**
  1. Extract user_id from JWT
  2. Query tasks WHERE user_id = current_user AND (status = filter OR filter = "all")
  3. Order by created_at DESC
- **Output (200):** `{ success: true, data: [{ id, title, description, status, created_at }] }`
- **Error Cases:**
  - No JWT → 401 `UNAUTHORIZED`

### FR-05: Update Task
- **Input:** `{ title?: string, description?: string, status?: "active"|"completed" }` via PUT /tasks/{id}
- **Processing:**
  1. Verify task exists AND belongs to current user
  2. Apply provided field updates
  3. Update updated_at timestamp
- **Output (200):** `{ success: true, data: { ...updated task } }`
- **Error Cases:**
  - Task not found → 404 `NOT_FOUND`
  - Task belongs to different user → 403 `FORBIDDEN`

### FR-06: Delete Task
- **Input:** Task ID via DELETE /tasks/{id} (authenticated)
- **Processing:**
  1. Verify task exists AND belongs to current user
  2. Hard delete the record
- **Output (204):** No content
- **Error Cases:**
  - Task not found → 404 `NOT_FOUND`
  - Task belongs to different user → 403 `FORBIDDEN`

---

## 4. External Interface Requirements

### 4.1 API Interface
- All responses (except 204): `{ success: boolean, data?: any, error?: { code, message } }`
- All timestamps: ISO 8601
- All IDs: UUID v4
- Auth header format: `Authorization: Bearer <token>`

### 4.2 Security
- All /tasks/* routes require valid JWT
- /auth/* routes are public
- CORS: allow only frontend origin

---

## 5. System Attributes

### 5.1 Performance
- List endpoint returns max 200 tasks per request (no pagination in V1 for simplicity)

### 5.2 Reliability
- All DB operations in try/except; return 500 with `INTERNAL_ERROR` code on failure
- Never expose raw exception messages to client

---

## 6. Validation & Testing Criteria

| Test | Input | Expected |
|------|-------|----------|
| Register valid user | valid email + 8-char pw | 201 + user object |
| Register duplicate email | existing email | 409 EMAIL_TAKEN |
| Register short password | password "abc" | 422 VALIDATION_ERROR |
| Login valid | correct credentials | 200 + token |
| Login wrong password | wrong password | 401 INVALID_CREDENTIALS |
| Create task | valid title, authenticated | 201 + task object |
| Create task empty title | "", authenticated | 422 VALIDATION_ERROR |
| Create task no auth | no JWT | 401 UNAUTHORIZED |
| List tasks with filter | ?filter=completed | 200 + only completed tasks |
| Delete own task | own task ID | 204 |
| Delete other user's task | other user's task ID | 403 FORBIDDEN |

---

# ===== FILE: SDS.md =====

# Software Design Specification: TaskFlow

**Version:** 1.0.0

---

## 1. System Architecture

### Pattern
REST API (FastAPI) + SPA (Next.js). Separate deployments communicating over HTTP.

### Components
```
[Browser]
  → [Next.js 15 Frontend — Vercel]
      → [FastAPI Backend — Railway]
          → [PostgreSQL — Railway]
```

---

## 2. Folder Structure

### Frontend
```
taskflow-web/
├── src/
│   ├── app/
│   │   ├── (auth)/
│   │   │   ├── login/
│   │   │   │   └── page.tsx
│   │   │   └── register/
│   │   │       └── page.tsx
│   │   ├── (dashboard)/
│   │   │   └── page.tsx          # Main task dashboard
│   │   ├── layout.tsx
│   │   └── providers.tsx         # React Query + theme providers
│   ├── components/
│   │   ├── ui/                   # shadcn/ui components
│   │   ├── auth/
│   │   │   ├── LoginForm.tsx
│   │   │   └── RegisterForm.tsx
│   │   └── tasks/
│   │       ├── TaskList.tsx
│   │       ├── TaskItem.tsx
│   │       ├── AddTaskInput.tsx
│   │       └── FilterTabs.tsx
│   ├── lib/
│   │   ├── api.ts               # Axios instance + interceptors
│   │   ├── auth.ts              # JWT helpers (store/read/clear)
│   │   └── utils.ts
│   └── types/
│       └── index.ts             # Task, User, ApiResponse types
├── .env.local
├── .env.example
└── package.json
```

### Backend
```
taskflow-api/
├── app/
│   ├── main.py                  # FastAPI app init, CORS, router registration
│   ├── core/
│   │   ├── config.py            # Settings via pydantic-settings
│   │   ├── database.py          # Async SQLAlchemy engine + session
│   │   └── security.py          # JWT create/verify, bcrypt helpers
│   ├── models/
│   │   ├── user_model.py
│   │   └── task_model.py
│   ├── schemas/
│   │   ├── user_schema.py       # Pydantic in/out schemas
│   │   └── task_schema.py
│   ├── routers/
│   │   ├── auth_router.py
│   │   └── task_router.py
│   └── services/
│       ├── auth_service.py
│       └── task_service.py
├── alembic/                     # DB migrations
├── .env
├── .env.example
└── pyproject.toml               # uv project config
```

---

## 3. Database Schema

```sql
CREATE EXTENSION IF NOT EXISTS "pgcrypto";

CREATE TABLE users (
  id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email         VARCHAR(255) NOT NULL UNIQUE,
  password_hash VARCHAR(255) NOT NULL,
  created_at    TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_users_email ON users(email);

CREATE TYPE task_status AS ENUM ('active', 'completed');

CREATE TABLE tasks (
  id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id       UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  title         VARCHAR(255) NOT NULL,
  description   TEXT,
  status        task_status NOT NULL DEFAULT 'active',
  created_at    TIMESTAMP NOT NULL DEFAULT NOW(),
  updated_at    TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_tasks_user_id ON tasks(user_id);
CREATE INDEX idx_tasks_status ON tasks(status);
```

---

## 4. API Routes

### Auth Routes (Public)
| Method | Route | Description |
|--------|-------|-------------|
| POST | /auth/register | Create user account |
| POST | /auth/login | Authenticate + return JWT |

### Task Routes (Authenticated)
| Method | Route | Description |
|--------|-------|-------------|
| GET | /tasks | List user's tasks (filterable) |
| POST | /tasks | Create new task |
| PUT | /tasks/{id} | Update task |
| DELETE | /tasks/{id} | Delete task |

### Utility
| Method | Route | Description |
|--------|-------|-------------|
| GET | /health | Health check (returns 200 OK) |

---

## 5. Environment Variables

```env
# Backend (.env)
APP_ENV=development
SECRET_KEY=your-secret-key-here
DATABASE_URL=postgresql+asyncpg://user:pass@localhost:5432/taskflow
JWT_ALGORITHM=HS256
JWT_EXPIRY_HOURS=24
FRONTEND_ORIGIN=http://localhost:3000

# Frontend (.env.local)
NEXT_PUBLIC_API_URL=http://localhost:8000
```

---

## 6. Security Design

- **Auth:** JWT HS256, 24h expiry, stored in localStorage (upgrade to httpOnly cookie in V1.1)
- **Password:** bcrypt with cost factor 12 via passlib
- **SQL:** SQLAlchemy ORM only — no raw queries
- **CORS:** Restrict to FRONTEND_ORIGIN env var
- **Error messages:** Generic for auth failures (no user enumeration)

---

# ===== FILE: SKILL.md =====

# AI Coding Agent Rules: TaskFlow

**Version:** 1.0.0  
**Stack:** Next.js 15 + FastAPI + PostgreSQL + Tailwind CSS + shadcn/ui

---

## Role

You are a senior full-stack engineer building TaskFlow. You write clean, typed, production-ready code 
following these rules without exception.

---

## ALWAYS

1. Use `uv` for Python — NEVER run `pip install` directly
2. Use `pnpm` for Node — NEVER use `npm install` or `yarn`
3. All env vars via `.env` / `.env.local` — NEVER hardcode values
4. Follow the folder structure in SDS.md exactly — do not create new top-level folders
5. All API responses must be `{ success, data?, error? }` — no exceptions
6. Use Pydantic schemas for all FastAPI request/response bodies
7. Use Zod for all Next.js form validation
8. Use React Query (`@tanstack/react-query`) for all server state — no useState for API data
9. Every component handles loading, error, and empty states
10. Write TypeScript with `strict: true` — no `any` types, no `// @ts-ignore`

---

## NEVER

1. NEVER use raw SQL — SQLAlchemy ORM only
2. NEVER store password_hash or token in API response (unless it IS the login endpoint)
3. NEVER implement features not in SPEC.md — ask before adding
4. NEVER use Redux, MobX, or Context for server state — React Query only
5. NEVER leave TODO comments in completed features
6. NEVER create files > 300 lines — split into focused modules
7. NEVER mix DB queries directly in routers — use service layer
8. NEVER skip error handling for any async operation
9. NEVER use `console.log` for debugging — use Python `logging` / browser `console.error`
10. NEVER commit `.env` files — `.env.example` only

---

## Code Patterns

### FastAPI Route
```python
@router.post("/tasks", response_model=TaskResponse, status_code=201)
async def create_task(
    payload: TaskCreate,
    db: AsyncSession = Depends(get_db),
    current_user: User = Depends(get_current_user)
) -> TaskResponse:
    task = await task_service.create(db, payload, current_user.id)
    return TaskResponse(success=True, data=task)
```

### React Query Hook
```typescript
export function useTasks(filter: "all" | "active" | "completed") {
  return useQuery({
    queryKey: ["tasks", filter],
    queryFn: () => api.get<Task[]>(`/tasks?filter=${filter}`),
  });
}
```

---

# ===== FILE: PROMPTS.md =====

# Execution Prompts: TaskFlow

Always ensure SPEC.md, SRS.md, SDS.md, and SKILL.md are loaded in context before using these.

---

## SETUP PROMPT
```
Read all .md files: SPEC.md, PRD.md, SRS.md, SDS.md, SKILL.md.
Summarize: (1) what TaskFlow is, (2) the tech stack, (3) the folder structure.
Do not write code yet.
```

---

## PROMPT 1: Scaffold Project
```
Scaffold both the taskflow-api (FastAPI) and taskflow-web (Next.js 15) projects 
using the exact folder structure in SDS.md section 2.
Use uv for the Python project (uv init taskflow-api).
Use pnpm for the Node project (pnpm create next-app taskflow-web --typescript --tailwind --app).
Create all files with comment blocks at the top.
No logic yet — structure only.
```

---

## PROMPT 2: Database Models
```
Implement the PostgreSQL schema from SDS.md section 3.
Create SQLAlchemy async models in:
- app/models/user_model.py
- app/models/task_model.py
Set up Alembic for migrations.
Follow SKILL.md rules: ORM only, no raw SQL.
```

---

## PROMPT 3: Auth Backend
```
Implement the auth system per SRS.md FR-01 and FR-02.
Create:
- app/core/security.py (bcrypt + JWT)
- app/schemas/user_schema.py (UserCreate, UserResponse, LoginRequest, TokenResponse)
- app/services/auth_service.py (register_user, login_user)
- app/routers/auth_router.py (POST /auth/register, POST /auth/login)

Follow all error cases in SRS.md FR-01 and FR-02.
All responses follow { success, data, error } format.
```

---

## PROMPT 4: Task Backend
```
Implement task CRUD per SRS.md FR-03 through FR-06.
Create:
- app/schemas/task_schema.py (TaskCreate, TaskUpdate, TaskResponse)
- app/services/task_service.py (create, list, update, delete)
- app/routers/task_router.py (GET/POST/PUT/DELETE /tasks)

All routes require authentication (get_current_user dependency).
Enforce user ownership on update and delete (403 if wrong user).
```

---

## PROMPT 5: Auth Frontend
```
Implement /login and /register pages per SPEC.md Screen 1 and Screen 2.
Create:
- src/components/auth/LoginForm.tsx
- src/components/auth/RegisterForm.tsx
- src/app/(auth)/login/page.tsx
- src/app/(auth)/register/page.tsx

Use Zod for validation. Use React Query mutations to call the API.
Store returned JWT in localStorage (key: "taskflow_token").
On success: redirect to /.
On error: show inline error message.
```

---

## PROMPT 6: Dashboard Frontend
```
Implement the main dashboard per SPEC.md Screen 3.
Create:
- src/components/tasks/AddTaskInput.tsx
- src/components/tasks/FilterTabs.tsx
- src/components/tasks/TaskItem.tsx
- src/components/tasks/TaskList.tsx
- src/app/(dashboard)/page.tsx

Filter state via URL query param (?filter=active).
Use useTasks() React Query hook.
TaskItem: checkbox (toggle status) + title + delete button.
Handle: loading spinner, empty state message, error toast.
```
