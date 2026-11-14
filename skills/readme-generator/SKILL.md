---
name: readme-generator
description: Generates high-detailed, comprehensive README.md files for any project. Auto-triggers on every project when README creation or update is requested. Covers all project functions, requirements, tech stack, architecture, API docs, setup instructions, and TLDR summary.
---

# README Generator

Generate exhaustive, production-quality README.md files for any project.

## When to Use This Skill

- User says "generate a README", "create readme", "write readme", "make readme", "add readme", "document the project", "README.md"
- User starts a new project or asks to document an existing one
- ANY project activity where a README.md is missing, incomplete, or needs updating
- ALWAYS auto-trigger this skill when README.md is mentioned

## Do NOT Use For

- Generating code documentation (docstrings, JSDoc, OpenAPI specs) — that is `code-documenter`
- Writing blog posts, articles, or general documentation not tied to a project

## Core Rules (NON-NEGOTIABLE)

### ALWAYS

1. **ALWAYS ask first:** Before generating, ask: "Would you like to include screenshots, a demo video, both, or neither in the README?"
2. **ALWAYS analyze the full project** — scan all files, folders, package.json, pyproject.toml, CMakeLists.txt, etc. to understand the complete codebase before writing.
3. **ALWAYS include EVERY section** listed in the README Template below. No shortcuts.
4. **ALWAYS end with a TLDR summary** — a brief 3-5 sentence overview at the very bottom.
5. **ALWAYS write in clear, professional English** with consistent formatting.
6. **ALWAYS use proper markdown** — tables, code blocks, headings, lists, badges, links.

### NEVER

- Never generate a README without analyzing the project structure first
- Never skip asking about screenshots/video
- Never leave placeholder text like "TODO" or "Description here"
- Never assume tech stack without reading config files
- Never plagiarize or copy from other projects
- Never use emojis unless the README already uses them (match project tone)

## README Template (Must Include ALL Sections)

Generate every section below. Skip only if clearly inapplicable and explain why.

### 0. Abstract (first thing in the file)

A 4-6 sentence overview that is the very first content in the README — no heading, no horizontal rule, just a blockquote. Reads like an academic abstract: what the project is, the problem it solves, key capabilities, and who it's for. The Title & Badges section comes immediately after with no separator.

Example:

> *Booksie is a full-stack web application that transforms PDF documents into an interactive AI-powered study experience. It automatically parses uploaded books into chapters, generates concise summaries per chapter and for the full text, and enables natural-language Q&A grounded in the document content. Built with Next.js 16, React 19, and SQLite, it uses Groq's LLM API for all AI inference. Designed for students, researchers, and lifelong learners who want to extract knowledge from books faster.*

### 1. Title & Badges

```
# Project Name

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-green.svg)]
...
```

Pull version from package.json/pyproject.toml/Cargo.toml etc. Pull license from LICENSE file.

### 2. Brief Description (2-3 sentences)

What does this project do? Who is it for? What problem does it solve?

### 3. Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [API Documentation](#api-documentation)
- [Project Structure](#project-structure)
- [Database Schema](#database-schema)
- [Testing](#testing)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)
- [TLDR](#tldr)

### 4. Features

List every major function/feature of the project with a short description.

| Feature | Description |
|---------|-------------|
| Feature 1 | What it does |
| Feature 2 | What it does |

### 5. Tech Stack

| Category | Technology |
|----------|-----------|
| Frontend | React 18, TypeScript, Tailwind CSS |
| Backend | Node.js, Express |
| Database | PostgreSQL 16 |
| ORM | Prisma |
| Auth | JWT, bcrypt |
| Hosting | Vercel |
| CI/CD | GitHub Actions |

Pull from package.json, requirements.txt, go.mod, Cargo.toml, CMakeLists.txt, Dockerfile, docker-compose.yml, etc.

### 6. Requirements

List all prerequisites with minimum versions:

- Node.js >= 18.x
- PostgreSQL >= 15
- npm >= 9.x
- Docker (optional)

### 7. Installation

Step-by-step, copy-paste ready:

```bash
git clone https://github.com/user/project.git
cd project
npm install
cp .env.example .env
# Edit .env with your values
npm run dev
```

### 8. Configuration

Document every environment variable, config file, and setting:

| Variable | Default | Description |
|----------|---------|-------------|
| `DATABASE_URL` | - | PostgreSQL connection string |
| `JWT_SECRET` | - | Secret key for JWT signing |
| `PORT` | 3000 | Server port |

### 9. Usage

Show common usage patterns:

- Basic usage
- CLI commands (if applicable)
- Screenshots/demo references (based on user's answer about media)
- Example code snippets

### 10. API Documentation

If the project has an API, document all endpoints:

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| GET | `/api/users` | List users | JWT |
| POST | `/api/users` | Create user | JWT |

Include request/response examples for key endpoints.

### 11. Project Structure

```
project/
├── src/
│   ├── components/
│   ├── pages/
│   └── utils/
├── public/
├── tests/
├── package.json
└── README.md
```

### 12. Database Schema

If applicable, document the data model:

```
User {
  id: UUID (PK)
  email: string (unique)
  password_hash: string
  created_at: timestamp
}
```

Or reference an ERD diagram.

### 13. Testing

How to run tests:

```bash
npm test        # unit tests
npm run e2e     # e2e tests
npm run coverage # coverage report
```

### 14. Deployment

Steps to deploy (or reference deployment config):

```bash
npm run build
npm start
```

Docker instructions if applicable.

### 15. Contributing

Standard contributing guidelines / link to CONTRIBUTING.md.

### 16. License

Link to LICENSE file. Name the license type.

### 17. TLDR (Final Section — Always Include)

3-5 sentence summary at the very bottom:

> **TLDR:** Project Name is a [type] built with [tech] that [does what]. It [key features]. Setup takes [X] minutes. See [relevant section] to get started.

## Workflow (Step-by-Step)

1. **Detect trigger** — User asks for README generation or update. Auto-trigger on any project activity where README is mentioned or missing.
2. **Ask about media** — "Would you like to include screenshots, a demo video, both, or neither in the README?"
3. **Analyze project** — Read all config files, source code structure, dependencies, tests, CI/CD config:
   - `package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`, `CMakeLists.txt`, `composer.json`
   - `requirements.txt`, `Pipfile`, `Gemfile`
   - `Dockerfile`, `docker-compose.yml`
   - `.env.example`, `config/` files
   - `tests/`, `__tests__/`, `spec/` directories
   - `LICENSE`, `CONTRIBUTING.md`, `CHANGELOG.md`
   - All source directories (scan top-level structure)
4. **Extract metadata** — Project name, version, description, author from config files.
5. **Determine tech stack** — Map dependencies to categories (frontend, backend, database, etc.).
6. **Identify features** — Scan source code for routes, controllers, commands, exports, main functions.
7. **Generate each section** — Follow the README Template above, section by section.
8. **Validate** — Run through the checklist in `references/readme-quality-checklist.md`.
9. **Render final README** — Write `README.md` to project root.
10. **Report** — Summarize what was generated and note any sections that were skipped with reason.

## Reference Files

Load these for additional guidance when needed:

| File | Purpose |
|------|---------|
| `references/spec-driven-readme-sections.md` | Requirements coverage, architecture review points, spec-driven dev alignment |
| `references/readme-quality-checklist.md` | Pre-delivery verification checklist |
