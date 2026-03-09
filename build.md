# Build Skill

Go from plan to working code systematically. This skill covers how to set up, structure,
and implement software in any language or framework — including when building with AI agents.

## Build Principles

1. **Work task by task** — One task from the plan at a time. Finish it before starting the next.
2. **Tests alongside code** — Write/run tests as you build, not at the end.
3. **Small commits** — Commit after each working task. Never commit broken code.
4. **Build vertically** — Complete one thin slice end-to-end before widening.
5. **Defer optimization** — Make it work, then make it right, then make it fast.

## Before You Write a Single Line

Confirm you have:
- [ ] A spec (what to build)
- [ ] A plan (tasks in order)
- [ ] Tests written for the first task (TDD)
- [ ] Development environment running
- [ ] A clean branch / starting point

If any of these are missing, go back. Code written without a spec is debt from line one.

## Project Setup Checklist

### New Project
- [ ] Initialize version control (git init or equivalent)
- [ ] Create a `.gitignore` appropriate for your stack
- [ ] Set up dependency management (package.json, requirements.txt, go.mod, etc.)
- [ ] Install a linter and formatter — configure it before writing code
- [ ] Set up a test runner and confirm it can discover and run tests
- [ ] Create the folder structure (see below)
- [ ] Write a README with: purpose, how to run, how to test
- [ ] Commit this skeleton before adding any features

### Folder Structure (Technology-Agnostic)
```
/
├── src/              ← application source code
│   ├── [domain]/     ← group by feature/domain, not by type
│   │   ├── [module].js
│   │   └── [module].test.js
├── tests/
│   ├── integration/  ← integration tests
│   └── e2e/          ← end-to-end tests
├── docs/
│   ├── specs/        ← spec files
│   └── plans/        ← plan files
├── scripts/          ← build, migration, utility scripts
├── .env.example      ← env variable template (never commit .env)
└── README.md
```

Organize by **domain/feature**, not by technical layer. Prefer:
```
/src/users/           ← user model, service, routes, tests together
/src/payments/        ← payment model, service, routes, tests together
```
Over:
```
/models/              ← all models
/services/            ← all services
/routes/              ← all routes
```

## Implementation Workflow (Per Task)

```
1. READ   the task and its acceptance criteria
2. WRITE  failing tests (if not already done in TDD step)
3. CODE   minimum implementation to pass tests
4. RUN    tests — confirm green
5. REVIEW your own code (see code review skill)
6. COMMIT with a descriptive message
7. NEXT   task
```

### Writing the Code

**Functions**
- One function, one responsibility. If you can't name it without "and", split it.
- Functions should fit on one screen (~25 lines max as a guide).
- Parameters: 0–3 is clean; 4+ is a smell — consider a config object.
- Return early. Avoid deeply nested conditionals.

```
// ❌ Deeply nested
function processOrder(order) {
  if (order) {
    if (order.items.length > 0) {
      if (order.isPaid) {
        // ... actual logic buried 3 levels deep
      }
    }
  }
}

// ✅ Early returns (guard clauses)
function processOrder(order) {
  if (!order) throw new Error("Order is required")
  if (order.items.length === 0) throw new Error("Order has no items")
  if (!order.isPaid) throw new Error("Order is not paid")
  // ... actual logic at top level
}
```

**Naming**
- Variables: nouns (`userCount`, `isValid`, `emailAddress`)
- Functions: verb + noun (`createUser`, `validateEmail`, `fetchOrders`)
- Booleans: is/has/can prefix (`isActive`, `hasPermission`, `canEdit`)
- Constants: SCREAMING_SNAKE_CASE (`MAX_RETRY_COUNT`)
- Be precise: `getUserById()` not `getUser()` when ID is the key

**Error Handling**
- Every external call (API, DB, file system) must handle failure.
- Distinguish between: user errors (return a message), system errors (log + throw), programming errors (throw + fix).
- Never silently swallow errors (`catch(e) {}`).
- Log errors with enough context to debug: what operation, what inputs, what failed.

**Configuration & Secrets**
- Never hardcode secrets, URLs, or environment-specific values.
- Use environment variables. Provide a `.env.example` with all keys (no values).
- Config should be loaded once at startup, not scattered throughout code.

## Handling External Dependencies

Before integrating any third-party library or service:
1. Check it's actively maintained and widely used.
2. Understand its failure modes.
3. Wrap it behind an interface you control — never scatter direct SDK calls across the codebase.
4. Write a fake/stub for testing (see TDD skill).

## Working with AI Agents (Replit Agent)

When delegating implementation to an AI agent:

**Good prompts include:**
- The spec section or acceptance criteria
- The failing tests to make pass
- The specific file(s) to create or modify
- Constraints: "Don't change the function signature", "Use only stdlib"

**Template prompt for Replit Agent:**
```
Implement [TASK-XXX]: [Task title]

Context:
[Paste relevant spec section]

Acceptance criteria:
[Paste ACs from spec]

Failing tests to pass:
[Paste test file or tests]

Constraints:
- [Any constraints]
- Do not modify [files that should stay unchanged]
```

**After the agent writes code:**
- Run tests — confirm they pass.
- Read the code — understand what was written.
- Check for: hardcoded values, missing error handling, no tests.
- Commit only if you understand and agree with what was generated.

## Commit Message Format

```
[type]: [short description in imperative mood]

[optional body: what and why, not how]

Refs: TASK-XXX
```

Types: `feat`, `fix`, `test`, `refactor`, `docs`, `chore`

Examples:
```
feat: add email validation to user signup
fix: return 404 when user not found instead of 500
test: add edge cases for empty cart checkout
refactor: extract payment logic into PaymentService
```

## Definition of Done (Build Phase)

- [ ] All tests for this task are passing
- [ ] No new linting errors or warnings
- [ ] Error cases are handled (not just happy path)
- [ ] No hardcoded secrets or environment-specific values
- [ ] Code is self-reviewed (see code review skill)
- [ ] Committed with a descriptive message

## Replit-Specific Notes
- Use the Replit Shell for running tests, git commands, and scripts.
- Keep `.env` in `.gitignore` — use Replit Secrets for environment variables in deployment.
- The Replit file tree auto-saves — still commit frequently to preserve history.
- When using Replit Agent: give it one task at a time with clear context; review before moving on.
- Use Replit's "Run" button for the main entry point, Shell for everything else.
