# Spec-Driven README Sections

Reference content extracted from project guides.

## Requirements Coverage (from sdd-ask-model-imp.txt)

Every README should address these aspects when applicable:

- Requirements - What must be true for the system?
- API contracts - Endpoints, request/response formats
- Database schema - Entities, relationships, constraints
- Data flow - How data moves through the system
- Security considerations - Auth, encryption, secrets
- Edge cases - Empty states, errors, limits
- Failure modes - What breaks and how it recovers
- Testing strategy - How correctness is verified

## Architecture Review Points (from sdd-ask-model-imp.txt)

Include trade-off analysis in README for architectural decisions:

- What alternatives were rejected and why?
- What are the key trade-offs?
- What breaks first under 10,000 users?
- What is the weakest part of the architecture?
- What technical debt was introduced?

## Spec-Driven Dev Structure (from write skills and spec driv dev.rtf)

Align README sections with spec-driven development layers:

- **SPEC** → Features, user flows, data models, APIs, screens
- **SRS** → Precise requirements, edge cases, validations, system rules, error handling
- **SDS** → Folder structure, database schema, API routes (exact), algorithms, system architecture
