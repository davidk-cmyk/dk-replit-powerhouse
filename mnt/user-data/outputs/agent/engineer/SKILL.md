---
name: engineer
description: >
  Full software development lifecycle skill — from idea to shipped code. Use this skill for
  ANY new feature, change, bug fix, or technical task. This is the one skill to use every
  time you build something. It covers the entire engineering workflow: spec → plan → TDD →
  build → code review → test. Trigger whenever the user says "build this", "implement",
  "add a feature", "make a change", "fix this", "I want to create", "help me develop",
  "new feature", "let's build", or starts describing something they want to make. Also trigger
  for any sub-task like "write tests", "review code", "plan this out", "write a spec", etc.
  This skill replaces all individual engineering skills — always use this one.
---

# Engineer Skill — Full Development Lifecycle

This skill guides every engineering change from idea to shipped code.
Each phase has a dedicated reference file with full details — load it when you enter that phase.

## The Loop

```
 ┌─────────────────────────────────────────────────────┐
 │  SPEC → PLAN → TDD → BUILD → REVIEW → TEST → SHIP  │
 │    ↑_________________feedback_____________________|  │
 └─────────────────────────────────────────────────────┘
```

Every change — no matter how small — moves through this loop.
Small changes may compress phases (a 1-line fix still needs a test and a review).
Large features need every phase in full.

---

## Phase Router

When a user brings a task, identify which phase to start from and load that reference file.

| User says... | Start at phase | Load reference |
|---|---|---|
| "I want to build X" / "New feature: X" | SPEC | `../spec/SKILL.md` |
| "Here's my spec, what next?" | PLAN | `../plan/SKILL.md` |
| "Help me plan / break this down" | PLAN | `../plan/SKILL.md` |
| "Write tests first" / "TDD this" | TDD | `../tdd/SKILL.md` |
| "Implement this" / "Write the code" | BUILD | `../build/SKILL.md` |
| "Review this code" / "Check my PR" | REVIEW | `../code-review/SKILL.md` |
| "Write integration/E2E tests" | TEST | `../test/SKILL.md` |
| Ambiguous — e.g. "Help me with X" | SPEC | `../spec/SKILL.md` |

**When in doubt: start at SPEC.** A spec costs 10 minutes. Rebuilding costs days.

---

## Phase Summaries

### 1. SPEC — Define What to Build
*Full details: `../spec/SKILL.md`*

Answer: What, Why, Who, and How we measure success.
Output: A markdown spec file with user stories, acceptance criteria, and open questions.
Gate: No tasks created until spec is approved.

### 2. PLAN — Sequence the Work
*Full details: `../plan/SKILL.md`*

Break the spec into tasks sized S/M/L/XL, ordered by dependency.
Output: A numbered task list with acceptance criteria, estimates, and a risk register.
Gate: No code written until plan exists.

### 3. TDD — Write Failing Tests First
*Full details: `../tdd/SKILL.md`*

For each task: list behaviors → write failing tests → confirm red → then implement.
Output: A test file with failing tests that defines the contract for the implementation.
Gate: Tests must be written and failing before implementation starts.

### 4. BUILD — Implement
*Full details: `../build/SKILL.md`*

Write minimum code to make tests pass. One task at a time. Commit after each green.
Output: Working code, passing tests, clean commits.
Gate: All tests passing, no hardcoded secrets, no silent error swallowing.

### 5. REVIEW — Check the Code
*Full details: `../code-review/SKILL.md`*

Self-review using the full checklist: correctness, security, tests, readability, performance.
Output: Review findings with severity labels (🔴 BLOCKER / 🟡 ISSUE / 🔵 SUGGESTION).
Gate: No blockers or issues before merging.

### 6. TEST — Validate End-to-End
*Full details: `../test/SKILL.md`*

Write integration and E2E tests for the completed feature. Validate the pre-ship checklist.
Output: Integration tests, E2E tests for critical journeys, coverage report.
Gate: Pre-ship checklist complete before deploying.

---

## Cross-Phase Rules (Always Active)

These apply in every phase regardless of which reference file is loaded:

**Spec traceability**: Every task → spec requirement. Every test → acceptance criterion.
**No phase skipping**: Compressed is OK (small fix = 1-line spec + 1 test). Skipped is not.
**Commit discipline**: One commit per completed task. Never commit broken or untested code.
**Secrets**: Never hardcode. Always use environment variables. Check in every phase.
**your IDE**: Store specs/plans in `/docs/`. Use Shell for tests/git. Use Secrets for env vars.

---

## Quick Reference: Phase Outputs

| Phase | File(s) to create |
|-------|------------------|
| SPEC | `/docs/specs/[feature-name].md` |
| PLAN | `/docs/plans/[feature-name].md` |
| TDD | `[module].test.[ext]` co-located with source |
| BUILD | Source files + passing tests |
| REVIEW | Review notes inline or in `/docs/reviews/` |
| TEST | `tests/integration/` and `tests/e2e/` |

---

## How to Use This Skill

**Starting fresh**: Say "I want to build [X]" → load `../spec/SKILL.md` → work through each phase in order.

**Jumping in mid-flow**: Say where you are ("I have a spec, need a plan") → jump to the right phase → load its reference file.

**Single-phase help**: Say "review this code" or "write integration tests" → jump directly to that phase.

**AI agent delegation** (AI agent): After PLAN, each task description becomes a prompt. After TDD, "make these failing tests pass" is your build prompt.
