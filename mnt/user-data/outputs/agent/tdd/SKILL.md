---
name: tdd
description: >
  Write failing tests before implementation to drive code design. Use when practicing TDD, writing tests first, defining behavior with tests, or test-driving a function or module. Triggered by: write tests first, TDD, failing tests, test-drive this, red-green-refactor.
---

# TDD Skill

Write tests first. Let failing tests drive the design and implementation of code.
This skill is technology-agnostic — the patterns apply regardless of language or framework.

## The TDD Cycle

```
RED   → Write a failing test that describes ONE desired behavior
GREEN → Write the minimum code to make it pass
REFACTOR → Clean up code and tests without changing behavior
REPEAT
```

Never skip RED. A test you didn't see fail is a test you can't trust.

## Test Anatomy

Every good test has three parts:

```
ARRANGE — Set up the conditions / inputs
ACT     — Call the code under test
ASSERT  — Verify the outcome
```

Also known as: **Given / When / Then** (from BDD) — maps directly to spec acceptance criteria.

## Test Types and When to Write Them

| Type | What it tests | When to write |
|------|--------------|---------------|
| **Unit** | A single function, method, or class in isolation | First — drive design |
| **Integration** | How two+ components work together | After units are stable |
| **Contract** | API inputs/outputs match agreed interface | When APIs cross team/service boundaries |
| **End-to-End** | Full user journey through the system | After integration; before shipping |

In TDD, write **unit tests first** for every task. Integration/E2E come later in the plan.

## TDD Workflow

### Step 1: Identify Behaviors to Test

Before writing a single test, list the behaviors from your spec:
- Happy path(s)
- Edge cases (empty input, boundary values, nulls)
- Error cases (invalid input, external failure, permission denied)
- State changes (what persists after the action?)

Example — for a `createUser(email, password)` function:
```
✅ Creates a user with valid email and password
✅ Returns the new user object with an ID
✅ Hashes the password before storing
✅ Throws if email is already taken
✅ Throws if email format is invalid
✅ Throws if password is too short (< 8 chars)
✅ Does not store plaintext password
```

### Step 2: Write the First Failing Test (RED)

Write ONE test. Run it. Confirm it fails for the right reason.

```
// Pseudocode — translate to your language/framework
test("createUser returns user object with ID when given valid inputs") {
  // Arrange
  email = "test@example.com"
  password = "securepassword123"

  // Act
  user = createUser(email, password)

  // Assert
  expect(user.id).toBeDefined()
  expect(user.email).toEqual(email)
}
```

**Failing for the right reason** means:
- ❌ Bad fail: "Function not found" (you haven't created the file yet — that's OK to start)
- ✅ Good fail: "Expected user.id to be defined, received undefined"

### Step 3: Write Minimum Code to Pass (GREEN)

Write only enough code to make this ONE test pass. No more.
Resist the urge to write the full implementation — that comes test by test.

```
// Minimum implementation — just enough to pass
function createUser(email, password) {
  return { id: generateId(), email: email }
}
```

Run tests. See GREEN.

### Step 4: Refactor

Now that tests pass, clean up:
- Remove duplication
- Improve naming
- Extract helpers
- Improve structure

Run tests again after refactoring. Still GREEN? Good. Move on.

### Step 5: Repeat for Next Behavior

Pick the next behavior from your list. Write a failing test. Make it pass. Refactor.

## Test Quality Rules

### Name Tests as Sentences
```
❌ test("createUser works")
✅ test("createUser throws ValidationError when email is already taken")
✅ test("createUser hashes the password before persisting")
```
A good test name is documentation. You should understand what broke just from the name.

### One Assertion Per Test (Default Rule)
Each test should verify one thing. Multiple assertions are OK only when they describe the
same logical outcome (e.g., checking all fields of a returned object).

### Tests Must Be Independent
- Tests must not depend on execution order.
- Each test sets up its own state (Arrange).
- Tear down any side effects in cleanup hooks.

### Test the Behavior, Not the Implementation
```
❌ Assert that hashingLibrary.hash() was called (testing HOW)
✅ Assert that stored password !== plaintext password (testing WHAT)
```
Tests tied to implementation break when you refactor. Tests tied to behavior survive refactors.

### Use Fakes for External Dependencies
Replace real databases, APIs, file systems, and clocks with fakes/mocks/stubs in unit tests.
This keeps tests fast, deterministic, and independent of infrastructure.

```
// Instead of hitting a real database:
userRepository = FakeUserRepository()  // in-memory, controlled by test
service = UserService(userRepository)
```

## Test File Organization

```
/src
  /users
    user-service.js        ← implementation
    user-service.test.js   ← tests live next to the code
/tests
  /integration             ← integration tests in separate folder
  /e2e                     ← end-to-end tests
```

Prefer co-locating unit tests with the code they test.

## Mapping Spec → Tests

Acceptance criteria from specs are your test cases. Map them directly:

**Spec AC**: "Given a valid email and password, when createUser is called, then a user record is persisted and a user object with ID is returned."

**Test**:
```
test("createUser persists user and returns object with ID given valid inputs")
```

Every acceptance criterion = at least one test. If an AC has no test, it's untested by definition.

## TDD Checklist

Before writing any implementation code:
- [ ] Listed all behaviors to test (happy paths, edge cases, errors)
- [ ] Wrote first failing test
- [ ] Confirmed test fails for the right reason
- [ ] Implementation written to pass the test
- [ ] All tests still passing after refactor
- [ ] Test names read as sentences describing behavior
- [ ] External dependencies are faked/mocked in unit tests
- [ ] Every spec acceptance criterion has a corresponding test

## IDE / Environment Notes
- Run tests in the terminal tab with your framework's test command.
- Keep a terminal split open: one pane for editing, one for running tests continuously.
- When using AI agent to implement code, prefix your prompt with the test file and say
  "Make these failing tests pass" — this is an extremely effective prompting pattern.
- Store test utilities and shared fakes in a `/tests/helpers/` or `/tests/fixtures/` directory.
