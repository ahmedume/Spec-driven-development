# AI-Assisted Production Development Workflow

The full lifecycle, end to end. Nineteen phases, plus the quality gate that runs after every feature.

The skills column is the harness mapping — which skill to load at each phase.

---

## 1. Project initialization

- [ ] Create the git repository
- [ ] Initialize the project
- [ ] Create `.gitignore`
- [ ] Configure Prettier
- [ ] Configure ESLint
- [ ] Configure TypeScript
- [ ] Set up Husky / lint-staged *(optional)*
- [ ] Create `.env.example`
- [ ] Initial commit

**Skills:** `devops-engineer`, `typescript-pro`

---

## 2. Requirements & planning

- [ ] Run the spec-driven process
- [ ] Generate the PRD
- [ ] Generate the SRS
- [ ] Generate the SDS / architecture
- [ ] Define the MVP
- [ ] Define user stories
- [ ] Define acceptance criteria
- [ ] Break into milestones
- [ ] Create the development roadmap

**Skills:** `spec-driven-dev`, `feature-forge`

---

## 3. Architecture review

**Do this before writing code.** It's cheaper to argue on a diagram than in a diff.

- [ ] Folder structure
- [ ] Tech stack validation
- [ ] Component architecture
- [ ] API design
- [ ] Database design
- [ ] Authentication flow
- [ ] Authorization flow
- [ ] State management plan
- [ ] Caching strategy
- [ ] Deployment strategy

**Skills:** `architecture-designer`, `api-designer`, `the-fool` (challenge the design)

See [`08-architecture-review.md`](08-architecture-review.md) for the question set.

---

## 4. Development plan

- [ ] Break the project into small features
- [ ] Prioritize by dependency
- [ ] Estimate complexity
- [ ] Decide implementation order

**One feature at a time. Not one layer at a time, not everything at once.**

---

## 5. Frontend development

- [ ] Layout
- [ ] Reusable components
- [ ] Responsive design
- [ ] Accessibility
- [ ] Animations
- [ ] Forms
- [ ] Error states
- [ ] Loading states
- [ ] Empty states

**Skills:** `elite-frontend-dev`, `high-front-end`, `awwwards`, `canvas-design`

> Error, loading, and empty states are not polish. They are the product for anyone on a slow connection, a bad network, or an empty account.

---

## 6. Backend development

- [ ] Server setup
- [ ] Middleware
- [ ] API routes
- [ ] Authentication
- [ ] Validation
- [ ] Business logic
- [ ] Error handling
- [ ] Logging

**Skills:** `backend-mastery`, `secure-code-guardian`, `nestjs-expert` / `fastapi-expert` / `django-expert` / `laravel-specialist` / `spring-boot-engineer` / `rails-expert`

---

## 7. Database

- [ ] Schema
- [ ] Migrations
- [ ] Indexes
- [ ] Seed data
- [ ] Relationships
- [ ] Constraints

**Skills:** `postgres-pro`, `sql-pro`, `database-optimizer`

---

## 8. Integration

- [ ] Connect frontend, backend, and database
- [ ] Configure environment variables
- [ ] API integration
- [ ] Authentication integration

**Skills:** `fullstack-guardian`

---

## 9. Testing

- [ ] Unit tests
- [ ] Integration tests
- [ ] End-to-end tests
- [ ] Manual testing
- [ ] Edge cases
- [ ] Cross-browser testing
- [ ] Mobile testing

**Skills:** `test-master`, `playwright-expert`, `webapp-testing`

---

## 10. Debugging

- [ ] Fix runtime errors
- [ ] Fix type errors
- [ ] Fix lint warnings
- [ ] Fix console warnings
- [ ] Remove dead code

**Skills:** `debugging-wizard`

---

## 11. Code review

- [ ] Read every changed file
- [ ] Simplify complex code
- [ ] Remove duplication
- [ ] Improve naming
- [ ] Verify architectural consistency

**Skills:** `code-reviewer`, `staff-engineer-review`, `karpathy-guidelines`

---

## 12. Performance optimization

- [ ] Code splitting
- [ ] Lazy loading
- [ ] Image optimization
- [ ] Reduce bundle size
- [ ] Memoization
- [ ] Query optimization
- [ ] Caching
- [ ] Lighthouse audit

**Skills:** `monitoring-expert`, `database-optimizer`, `javascript-pro`

---

## 13. Security review

- [ ] Remove hardcoded secrets
- [ ] Validate inputs
- [ ] Prevent SQL injection
- [ ] Prevent XSS
- [ ] Prevent CSRF where applicable
- [ ] Secure authentication
- [ ] Secure cookies and tokens
- [ ] Rate limiting
- [ ] Environment variable check
- [ ] Dependency vulnerability scan

**Skills:** `security-reviewer`, `code-security-auditor`, `secure-code-guardian`

---

## 14. Cleanup

- [ ] Remove unused files
- [ ] Remove unused packages
- [ ] Remove debug logs
- [ ] Remove TODOs
- [ ] Organize imports
- [ ] Format code
- [ ] Final lint

**Skills:** `code-reviewer`, `karpathy-guidelines`

---

## 15. Documentation

- [ ] README
- [ ] Installation guide
- [ ] Environment setup
- [ ] API documentation
- [ ] Architecture overview
- [ ] Deployment instructions

**Skills:** `readme-generator`, `code-documenter`

---

## 16. Version control

- [ ] Commit by feature
- [ ] Meaningful commit messages
- [ ] Tag releases
- [ ] Merge cleanly

**Skills:** `changelog-generator`

---

## 17. Deployment

- [ ] Production build
- [ ] Environment configuration
- [ ] CI/CD verification
- [ ] Deploy frontend
- [ ] Deploy backend
- [ ] Deploy database
- [ ] Smoke test production

**Skills:** `devops-engineer`, `terraform-engineer`, `kubernetes-specialist`

---

## 18. Post-deployment

- [ ] Monitor logs
- [ ] Check analytics
- [ ] Monitor performance
- [ ] Monitor errors
- [ ] Verify backups
- [ ] Patch issues

**Skills:** `monitoring-expert`, `sre-engineer`

---

## 19. Retrospective

- [ ] What went well?
- [ ] What caused bugs?
- [ ] What can be automated?
- [ ] Update skills and prompts

> The last item is the one that compounds. A retrospective that doesn't change a skill file is a meeting.

---

## The quality gate

Nothing moves forward until all five pass, **per feature**:

```
Feature implemented
  → Type check passes
    → Lint passes
      → Tests pass
        → Manual test passes
          → Commit to git
```

This is deliberately boring. Boring gates are the only ones that get run.

---

## How this relates to the spec-driven pipeline

This workflow is the **full** lifecycle. The `spec-driven-dev` skill covers phases 2–4 in depth — the part where most projects actually fail — then hands off to phase 5 onward.

For the short version of the same pipeline, see the [repo README](../README.md#the-pipeline).

---

*Structured from the original `AI_Assisted_Production_Development_Workflow` source.*
