---
name: orchestrator
description: >
  Run the full software development lifecycle end-to-end for a feature — from spec through
  shipping code. Use this skill when an engineer wants to build a complete feature from scratch,
  go from idea to working code, or run the full development pipeline. Trigger when the user says
  "build this feature end to end", "take this from idea to code", "run the full pipeline",
  "orchestrate this feature", "build this from scratch", "full lifecycle", "spec to ship",
  or "do the whole thing". This skill chains all other skills in sequence and manages
  transitions, quality gates, and context passing between phases.
---

# Orchestrator Skill

Drive a feature from idea to shipped code by running every skill in the pipeline sequentially,
enforcing quality gates between phases, and carrying context forward so nothing is lost.

The context skill runs first as Phase 0 — it acts as the project's subject matter expert,
injecting domain knowledge, prior decisions, and conventions so that spec and plan start
with full awareness of the project state.

## Orchestrator Principles

1. **One phase at a time** — Complete each phase fully before moving to the next.
2. **Quality gates** — Each phase must meet its exit criteria before the next begins.
3. **Context carries forward** — Output from each phase feeds into the next as input.
4. **Learn at the end** — After shipping, always run the learn phase to capture knowledge.
5. **PM runs throughout** — Track issues and status at every phase transition.

## The Pipeline

```
┌───────────────────────────────────────────────────────────────────────────┐
│                            ORCHESTRATOR                                    │
│                                                                           │
│  ┌─────────┐   ┌─────┐   ┌──────┐   ┌─────┐   ┌───────┐   ┌────────┐  │
│  │ context  │ → │ spec │ → │ plan │ → │ tdd │ → │ build │ → │ review │  │
│  └─────────┘   └─────┘   └──────┘   └─────┘   └───────┘   └────────┘  │
│       │            │          │          │          │           │         │
│       │            ▼          ▼          ▼          ▼           ▼         │
│       │   ┌──────────────────────────────────────────────────────────┐   │
│       │   │                    pm (continuous)                        │   │
│       │   └──────────────────────────────────────────────────────────┘   │
│       │                                                                   │
│       │   ┌──────┐   ┌─────────────┐   ┌───────┐                        │
│       │   │ test │ → │ tech-writer  │ → │ learn │                        │
│       │   └──────┘   └─────────────┘   └───────┘                        │
│       │                                                                   │
│       └── context is consulted by spec and plan as SME ──────────────    │
└───────────────────────────────────────────────────────────────────────────┘
```

## Phase Execution Protocol

For each phase, follow this pattern:

```
1. STATE   the phase being entered and its goal
2. CHECK   prerequisites / input from previous phase
3. RUN     the skill for this phase
4. VERIFY  exit criteria are met
5. PASS    output artifacts to the next phase
6. LOG     status update via pm skill
```

## Phase Details

### Phase 0: Context

**Invoke**: context skill
**Input**: Feature request or problem statement + existing `.context/` folder
**Output**: Context briefing document for downstream skills

**Exit criteria**:
- [ ] `.context/` folder scanned and relevant files identified
- [ ] Context briefing produced with domain knowledge, prior decisions, conventions, and constraints
- [ ] Stale or missing context flagged for update
- [ ] Spec and plan skills have the domain understanding they need

**PM action**: Log context state — note any gaps that could cause rework.

---

### Phase 1: Spec

**Invoke**: spec skill (with context briefing from Phase 0)
**Input**: Feature request, user description, or problem statement + **context briefing**
**Output**: `docs/specs/[feature]-spec.md`

**Exit criteria**:
- [ ] Problem statement is clear and scoped
- [ ] User stories with acceptance criteria are defined
- [ ] Functional requirements are numbered and testable
- [ ] Non-goals are explicitly stated
- [ ] Open questions are captured (resolve before proceeding if blocking)

**PM action**: Create Feature issue(s) linked to the spec.

---

### Phase 2: Plan

**Invoke**: plan skill
**Input**: Approved spec from Phase 1
**Output**: `docs/plans/[feature]-plan.md`

**Exit criteria**:
- [ ] Tasks are sequenced with dependencies mapped
- [ ] Each task maps to spec requirements (traceability)
- [ ] Estimates are provided (S/M/L/XL — split XL tasks)
- [ ] First task is clearly identified as the starting point

**PM action**: Link TASK entries to issues. Update backlog.

---

### Phase 3: TDD

**Invoke**: tdd skill
**Input**: Plan from Phase 2 + spec acceptance criteria
**Output**: Failing test files for the first task (and subsequent tasks as you progress)

**Exit criteria**:
- [ ] Tests exist for the current task's acceptance criteria
- [ ] All tests fail (red) — confirming they test real behavior
- [ ] Test structure matches the plan's task breakdown

**PM action**: Move current task issue to In Progress.

---

### Phase 4: Build

**Invoke**: build skill
**Input**: Failing tests from Phase 3 + plan tasks
**Output**: Implementation code — all tests passing

**Exit criteria**:
- [ ] All tests from Phase 3 pass (green)
- [ ] No hardcoded secrets or environment-specific values
- [ ] Code follows project conventions
- [ ] Each task committed individually with descriptive messages

**PM action**: Update issues as tasks complete. Log any new bugs found.

---

### Phase 5: Code Review

**Invoke**: code-review skill
**Input**: All code written in Phase 4
**Output**: Review findings — fixes applied or issues logged

**Exit criteria**:
- [ ] No P0/P1 issues remain unresolved
- [ ] Security concerns addressed
- [ ] Code is clean and readable
- [ ] All review-driven fixes are committed

**PM action**: Create issues for deferred review findings (P2/P3).

---

### Phase 6: Test

**Invoke**: test skill
**Input**: Built and reviewed code
**Output**: Integration/E2E test results — all passing

**Exit criteria**:
- [ ] Integration tests pass
- [ ] E2E tests cover critical user flows from the spec
- [ ] Edge cases and error paths are tested
- [ ] No regressions in existing functionality

**PM action**: Create Bug issues for any failures. Update status to Done when passing.

---

### Phase 7: Documentation

**Invoke**: tech-writer skill
**Input**: Completed feature code + spec
**Output**: Updated docs, README, changelog, API reference as needed

**Exit criteria**:
- [ ] User-facing docs reflect new functionality
- [ ] Changelog updated
- [ ] README updated if applicable
- [ ] API docs updated if applicable

---

### Phase 8: Learn

**Invoke**: learn skill
**Input**: Everything produced during this pipeline run
**Output**: Updated knowledge files, decision log, lessons captured

**Exit criteria**:
- [ ] Decisions and rationale documented
- [ ] Patterns and conventions discovered are recorded
- [ ] Gotchas and lessons learned are captured
- [ ] Relevant project context files updated

---

## Handling Failures and Iterations

### Test Failures in Phase 4 (Build)
```
If tests fail → fix code → re-run tests → continue only when green
```

### Review Findings in Phase 5
```
If P0/P1 found → fix immediately → re-review the fix → continue
If P2/P3 found → log as issue in PM → continue (defer to backlog)
```

### Test Failures in Phase 6
```
If integration/E2E fails → loop back to Phase 4 (build fix) →
  Phase 5 (review fix) → Phase 6 (re-test)
```

### Scope Changes Mid-Pipeline
```
If new requirements surface → update spec (Phase 1) →
  update plan (Phase 2) → continue from where disrupted
```

## Quick Start

When the user provides a feature description, begin immediately:

```
"I'll orchestrate this feature through the full pipeline.
Starting with Phase 0: Context — gathering project knowledge."
```

Then proceed phase by phase, announcing each transition:

```
"Phase 0 (Context) complete. Moving to Phase 1: Spec."
```

## Resuming a Partial Pipeline

If a pipeline was interrupted, check for existing artifacts:
1. Check `.context/` for project context (always re-scan on resume)
2. Look in `docs/specs/` for a spec
3. Look in `docs/plans/` for a plan
4. Check for existing test files
5. Check git log for build commits
6. Resume from the earliest incomplete phase

## Context Passing Template

At each phase transition, summarize what's being passed forward:

```markdown
## Transition: [Phase N] → [Phase N+1]

### Artifacts produced:
- [file path]: [what it contains]

### Key decisions made:
- [decision]: [rationale]

### Open items carried forward:
- [item]: [status]
```
