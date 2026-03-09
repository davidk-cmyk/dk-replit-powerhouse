---
name: code-review
description: >
  Review code for quality, correctness, security, and maintainability. Use this skill whenever
  an engineer wants to review code — their own or someone else's — perform a pull request
  review, audit code for issues, check code quality, identify bugs or security issues, give
  feedback on implementation, or self-review before committing. Trigger when the user says
  "review this code", "check my code", "what's wrong with this", "PR review", "code review",
  "is this code good", "find bugs in", "audit this", "give me feedback on my implementation",
  or "self-review checklist". Use this skill after building and before merging or shipping.
---

# Code Review Skill

Review code systematically to catch bugs, security issues, and quality problems before they
reach production. This skill covers both self-review and reviewing others' code.

## Review Mindset

- **Goal**: Improve the code and the author — not to find fault.
- **Be specific**: "This function does X when Y is null, which will cause Z" beats "this is bad".
- **Ask questions**: "Could this be simplified by..." not "You should have...".
- **Prioritize**: Distinguish blockers (must fix) from suggestions (nice to have).
- **Review the spec**: Code that works but doesn't match requirements is still wrong.

## Review Checklist

Work through this checklist top to bottom. Note every finding with line number and severity.

### 1. Correctness
- [ ] Does the code do what the spec / task says it should?
- [ ] Are all acceptance criteria met?
- [ ] Does the happy path work?
- [ ] Are edge cases handled? (empty inputs, null/undefined, boundary values)
- [ ] Are error cases handled? (what happens when things go wrong?)
- [ ] Are there off-by-one errors in loops or ranges?
- [ ] Are async operations awaited correctly? Are race conditions possible?
- [ ] Is state mutated when it shouldn't be (or vice versa)?

### 2. Tests
- [ ] Are there tests for this code?
- [ ] Do tests cover happy path, edge cases, and error cases?
- [ ] Do tests actually test behavior, not just assert that mocks were called?
- [ ] Would a failing test name tell you what broke without reading the code?
- [ ] Could the tests pass even if the code were wrong? (false confidence)
- [ ] Are tests isolated? (no cross-test state dependency)

### 3. Security
- [ ] Is any user input validated and sanitized before use?
- [ ] Is any user input ever interpolated into a query, command, or HTML without escaping?
- [ ] Are secrets or credentials hardcoded anywhere?
- [ ] Are authentication and authorization checks in place where needed?
- [ ] Are error messages exposing stack traces or internal details to end users?
- [ ] Are there any overly permissive access controls?
- [ ] Is sensitive data logged?
- [ ] Are dependencies known to be safe (no known vulnerabilities)?

### 4. Readability & Maintainability
- [ ] Can you understand what the code does without asking the author?
- [ ] Are variable and function names clear and precise?
- [ ] Is there dead code (commented-out code, unused variables, unreachable branches)?
- [ ] Are functions small and focused (single responsibility)?
- [ ] Is there duplicated logic that should be extracted?
- [ ] Are complex sections explained with a comment? (the WHY, not the WHAT)
- [ ] Would someone unfamiliar with the codebase understand this in 6 months?

### 5. Error Handling
- [ ] Are errors caught where they can occur (external calls, parsing, I/O)?
- [ ] Are errors handled or re-thrown with context — never silently swallowed?
- [ ] Are user-facing error messages helpful and free of internal details?
- [ ] Is the system left in a consistent state after an error?

### 6. Performance
- [ ] Are there obvious performance issues? (N+1 queries, unnecessary loops, blocking I/O)
- [ ] Are large objects or collections copied unnecessarily?
- [ ] If this code runs in a hot path — is it fast enough?
- [ ] Are resources (connections, file handles, timers) properly released?

### 7. Design & Architecture
- [ ] Does this change fit within the existing architecture or break patterns?
- [ ] Is there a simpler way to accomplish the same thing?
- [ ] Does this introduce a new dependency? Is it justified?
- [ ] Does this create tight coupling that will be painful to change later?
- [ ] Are new abstractions added only when there's genuine duplication (Rule of Three)?

### 8. Spec Compliance
- [ ] Does this match the spec exactly, or did implementation drift?
- [ ] Are there any features built that weren't in the spec (scope creep)?
- [ ] Are all non-functional requirements met (performance, security, accessibility)?

## Severity Labels

When leaving feedback, use consistent severity levels:

| Label | Meaning | Action Required |
|-------|---------|----------------|
| 🔴 **BLOCKER** | Bug, security issue, or spec mismatch | Must fix before merge |
| 🟡 **ISSUE** | Real problem but not a blocker | Should fix in this PR |
| 🔵 **SUGGESTION** | Improvement opportunity | Nice to have, author's call |
| ⬜ **NIT** | Minor style/naming thing | Optional, don't block on it |

## Giving Feedback

### Good Feedback
```
🔴 BLOCKER [line 42]: getUserById() doesn't handle the case where the database returns null.
Calling user.email on line 45 will throw a TypeError when the user is not found.
Suggestion: Add a null check and return a 404 response.
```

```
🟡 ISSUE [line 78]: The API key is read from a hardcoded string instead of process.env.
This will be visible in version control and break in other environments.
```

```
🔵 SUGGESTION [line 31]: This loop iterates the full array twice (once for filter, once for map).
These could be combined into a single reduce to improve readability and performance.
```

### Avoid
- ❌ "This is wrong" (no explanation)
- ❌ "Why did you do it this way?" (sounds accusatory — ask genuinely)
- ❌ "I would have done X" (unless X is clearly better, this is noise)
- ❌ Blocking PRs on style issues that a formatter should handle automatically

## Self-Review Protocol

Before requesting any review from another human or AI:

1. **Step away** — Let the code sit for 10+ minutes if possible. Return with fresh eyes.
2. **Read it line by line** — Not skim. Read it as if you're seeing it for the first time.
3. **Run the full checklist above** — Don't skip sections.
4. **Run tests** — Confirm all passing before submitting for review.
5. **Review the diff** — Look at what you've changed in aggregate. Does it make sense?
6. **Read it aloud (optional)** — Awkward names and logic become obvious when spoken.

## Code Review Output Format

When writing a review, structure it as:

```markdown
## Code Review: [Task / PR / File Name]

**Reviewer**: [You / Claude]
**Date**: [Date]
**Verdict**: ✅ Approved | 🔄 Approve with minor fixes | 🔴 Needs rework

### Summary
1–2 sentences on overall quality and what the code does.

### Findings

#### 🔴 Blockers
- [file.js:42] ...

#### 🟡 Issues
- [file.js:78] ...

#### 🔵 Suggestions
- [file.js:31] ...

#### ⬜ Nits
- ...

### Positives
What was done well? (always include something genuine)
```

## Replit-Specific Notes
- Use Replit's built-in diff view when reviewing changes before committing.
- For AI-assisted reviews: paste the code and this checklist into Claude with "Review this code
  against the checklist below."
- When reviewing Replit Agent output: always review generated code before running or committing —
  agents can produce working but insecure or poorly structured code.
- Pay extra attention to secrets/environment variables in Agent-generated code.
