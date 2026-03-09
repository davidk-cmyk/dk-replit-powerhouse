---
name: test
description: >
  Write integration and E2E tests and validate a feature before shipping. Use when writing integration tests, end-to-end tests, defining a test strategy, or running a pre-ship checklist. Triggered by: integration tests, E2E tests, test strategy, test coverage, validate before shipping.
---

# Test Skill

Design and execute a complete test strategy that validates software actually works — not just
that its individual pieces work, but that they work together and deliver the expected user value.

## Test Strategy Overview

A complete test strategy has layers. Each layer serves a different purpose:

```
        /\
       /E2E\         ← Few, slow, high confidence in user journeys
      /------\
     / Integr \      ← Some, medium speed, validates component wiring
    /----------\
   /   Unit     \    ← Many, fast, drives design (covered in TDD skill)
  /--------------\
```

The TDD skill covers **Unit Tests**. This skill covers everything above it.

## Integration Tests

Integration tests verify that two or more components work correctly together.
They use real (or realistic) implementations — no fakes for the things being integrated.

### What to Integration Test

- **Service + Database**: Does the service correctly read/write data with a real DB?
- **Service + External API**: Does the integration with a third-party service work?
- **API Layer + Service**: Does the HTTP endpoint correctly invoke the service and return the right response?
- **Message Queue + Consumer**: Does the consumer correctly process messages?

### Integration Test Template

```
// Pseudocode
integration test: "[Component A] + [Component B] — [Scenario]"

setup:
  - Start real or in-memory instance of dependency (DB, cache, queue)
  - Seed any required state
  - Initialize the components under test with real dependencies

test:
  ARRANGE: set up input state
  ACT:     invoke the operation that crosses component boundaries
  ASSERT:  verify outcome in the real dependency (not just return value)

teardown:
  - Clean up seeded data
  - Close connections
```

### Integration Test Checklist
- [ ] Tests use a real or realistic implementation of each integrated component
- [ ] Test database/service is separate from production (test environment)
- [ ] Each test cleans up its own data (no cross-test pollution)
- [ ] Tests cover: success path, not-found, duplicate/conflict, permission denied
- [ ] Tests do not depend on execution order

## End-to-End (E2E) Tests

E2E tests simulate a real user journey through the entire system — from input to final output.
They are the most expensive to write and maintain but give the highest confidence.

### What to E2E Test
- The 2–3 most critical user journeys (from your spec's Key Flows)
- Anything that if it breaks, would be immediately visible to users
- New features before their first production release

### E2E Test Selection Criteria
Write an E2E test when:
- The scenario spans multiple services or layers
- It maps to a spec acceptance criterion that integration tests can't fully verify
- It's a business-critical flow (login, checkout, core data creation)

Do NOT write E2E tests for:
- Every edge case (unit tests handle these)
- Internal implementation details
- Things already fully covered by integration tests

### E2E Test Template

```
E2E test: "[User Persona] can [complete journey]"

preconditions:
  - System is in known starting state
  - Test user account exists (or is created in setup)

steps:
  1. [User action] → [Expected system response]
  2. [User action] → [Expected system response]
  ...
  N. [Final state verification]

assertions:
  - UI/API shows expected final state
  - Data persisted correctly in the system
  - Side effects occurred (email sent, event fired, etc.)

cleanup:
  - Remove test data created during the test
```

### E2E Test Best Practices
- Use a dedicated test environment — never run E2Es against production.
- Create test data programmatically in setup, don't rely on manual seeding.
- Test data should have a unique prefix/ID so it can be cleaned up reliably.
- E2E tests are inherently slower — run them less frequently (pre-deploy, not per-commit).
- When an E2E test breaks, don't just re-run it — find the root cause.

## Test Environment Setup

Before running any tests beyond unit tests:

### Environment Checklist
- [ ] Separate test environment (database, services) — isolated from dev and prod
- [ ] Environment variables for test configuration (`.env.test` or equivalent)
- [ ] Test data seeding mechanism (scripts or fixtures)
- [ ] Test data cleanup mechanism (teardown hooks or isolated transactions)
- [ ] CI pipeline runs tests automatically on every commit or PR
- [ ] Test results are visible and actionable (not buried in CI logs)

### Environment Isolation Rules
- Test DB ≠ Dev DB ≠ Prod DB. Ever.
- No test should permanently modify shared state.
- If tests must share state, seed it in a `beforeAll` and tear it down in `afterAll`.
- External services (email, SMS, payment): use a sandbox/test mode or mock at the network layer.

## Test Coverage

Coverage measures how much of your code is exercised by tests.
Aim for meaningful coverage, not 100% line coverage.

### What to Prioritize
| Priority | Coverage Target | Why |
|----------|----------------|-----|
| Business logic / domain | 90%+ | Core value; must be correct |
| API handlers | 80%+ | Entry points; must handle errors |
| Data layer | 70%+ | Persistence; must be validated |
| Utility functions | 60%+ | Low risk if simple |
| Config / glue code | Low | Hard to test; low value |

### Coverage Anti-Patterns
- ❌ Testing code just to hit a number — tests that assert nothing meaningful
- ❌ Skipping error paths to keep the number up
- ❌ Over-mocking to avoid "hard" tests — this fakes coverage

### Generating a Coverage Report
Most test frameworks support coverage reporting. Run:
```
[test command] --coverage
```
Review the report and focus new tests on:
1. Untested branches in critical paths
2. Error handling code
3. Code that recently changed

## Debugging Failing Tests

When a test fails, work through this systematically:

```
1. READ  the failure message carefully — what was expected vs. what happened?
2. ISOLATE  run only the failing test in isolation
3. CHECK  has the test ever passed? If yes, what changed?
4. PRINT  add temporary logging to observe actual state
5. SIMPLIFY  reduce the test to the minimum that reproduces the failure
6. FIX  fix the code OR the test (not both at once)
7. VERIFY  run full suite to confirm no regressions
```

### Common Failure Patterns
| Symptom | Likely Cause |
|---------|-------------|
| Test passes alone, fails in suite | State pollution from another test |
| Test fails randomly | Race condition or timing issue |
| Test always fails after a refactor | Test was testing implementation, not behavior |
| Test never fails (even when code is broken) | Assertion is wrong or not reached |
| "Expected X, received undefined" | Async operation not awaited |

## Pre-Ship Test Checklist

Before shipping any feature to production:

- [ ] All unit tests passing
- [ ] All integration tests passing
- [ ] E2E tests for critical journeys passing
- [ ] Test coverage meets targets for changed code
- [ ] Tests run in CI pipeline (not just locally)
- [ ] No tests skipped without documented reason
- [ ] Tested in an environment that mirrors production
- [ ] Spec acceptance criteria manually verified (spot check)
- [ ] Edge cases from the spec have test coverage
- [ ] Error scenarios produce expected user-facing messages

## IDE / Environment Notes
- Run tests in the Shell tab: keep a split with your test command running in watch mode.
- Store test fixtures and seed data in `/tests/fixtures/` as JSON or code files.
- For E2E tests in your IDE: use a `.env.test` secret or a separate environment for the test
  environment to keep it isolated from your dev environment.
- When using AI agent to write tests: provide the spec acceptance criteria and say
  "Write integration tests that verify these acceptance criteria."
- a persistent environment can serve as a lightweight staging environment for E2E tests.
