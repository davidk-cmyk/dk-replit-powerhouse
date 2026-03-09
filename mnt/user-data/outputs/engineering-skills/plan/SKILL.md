---
name: plan
description: >
  Break down a software spec or feature into a concrete, sequenced engineering plan. Use this
  skill whenever an engineer needs to plan their work, create a task breakdown, estimate effort,
  sequence dependencies, write tickets, prioritize a backlog, create a sprint plan, or figure
  out what to build and in what order. Trigger when the user says things like "help me plan
  this", "break this down into tasks", "how should I approach building this", "what should I
  build first", "create tickets for this feature", or "help me estimate this". Always use this
  skill after writing a spec and before writing code — planning prevents rework.
---

# Plan Skill

Transform a spec or feature idea into a sequenced, actionable engineering plan that any
developer (or AI agent) can execute without ambiguity.

## Planning Principles

1. **Vertical slices over horizontal layers** — Build end-to-end thin slices that deliver value,
   not all-backend-then-all-frontend.
2. **Smallest possible tasks** — Each task should be completable in a single focused session.
3. **Dependencies first** — Unblock others early; foundation work comes before feature work.
4. **Test and integration last** — Integration tests after units; E2E tests after integration.
5. **Risk first** — Tackle the riskiest or most uncertain tasks early to fail fast.

## Plan Template

```markdown
# Plan: [Feature / Project Name]

## Linked Spec
[Link or reference to the spec this plan implements]

## Approach Summary
2–3 sentences describing the overall technical strategy.
What are the major phases? What are the key architectural decisions?

## Milestones
| # | Milestone | Description | Target |
|---|-----------|-------------|--------|
| M1 | Foundation | Core data model + API skeleton | Day X |
| M2 | Core Feature | Main user-facing functionality | Day Y |
| M3 | Polish + Tests | Edge cases, error handling, coverage | Day Z |

## Task Breakdown

### M1: [Milestone Name]

**TASK-001** — [Short imperative title, e.g., "Create data model for User entity"]
- **What**: What exactly needs to be done
- **Why**: Why this is needed / what it unblocks
- **Acceptance**: How do we know it's done? (links to spec AC if applicable)
- **Estimate**: S (< 1hr) / M (1–3hr) / L (3–8hr) / XL (needs splitting)
- **Depends on**: TASK-XXX (if any)

**TASK-002** — [Title]
- ...

### M2: [Milestone Name]
...

## Risk Register
| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Third-party API rate limits | Medium | High | Cache responses; add retry logic |
| Unclear requirement X | High | Medium | Schedule spike; timebox 2hr |

## Definition of Done
A task is "done" when:
- [ ] Code written and self-reviewed
- [ ] Unit tests written and passing
- [ ] No new linting errors
- [ ] PR / changeset created and linked to task
- [ ] Reviewed by at least one other person (or AI agent review completed)
- [ ] Acceptance criteria from spec verified

## Parking Lot (Future)
Ideas and scope that are explicitly deferred to a future iteration.
```

## How to Break Down Tasks Well

### Right-sizing Tasks
| Size | Duration | Rule |
|------|----------|------|
| S | < 1 hr | Prefer this — maximizes flow and feedback |
| M | 1–3 hr | Acceptable for non-trivial units of work |
| L | 3–8 hr | Flag for review — can it be split? |
| XL | > 8 hr | Must be split before starting |

### Good Task Titles
- ✅ Imperative verb + clear noun: "Add email validation to signup form"
- ✅ Specific and testable: "Return 404 when user ID does not exist"
- ❌ Too vague: "Work on auth"
- ❌ Too broad: "Build the entire checkout flow"

### Sequencing Rules
1. Data models before business logic.
2. Business logic before UI.
3. Happy path before error handling.
4. Unit tests alongside the code they test (not after).
5. Integration tests after all units are stable.
6. Documentation last (but before Done).

### Dependency Mapping
When tasks depend on each other:
- Mark them explicitly with `Depends on: TASK-XXX`.
- Draw a simple ASCII dependency graph for complex chains:
```
TASK-001 → TASK-003 → TASK-005 (critical path)
TASK-002 → TASK-004 ↗
```

## Estimating

Use **relative sizing** (t-shirt sizes), not hours for planning.
Convert to time only when scheduling:
- S = ~1 focused session
- M = half a day
- L = full day
- XL = split it

Add a **20% buffer** to your total estimate for integration, bug fixing, and review overhead.

## Replit-Specific Notes
- Store the plan as `/docs/plans/[feature-name].md` alongside the spec.
- In Replit, the AI agent works best with small, focused tasks — prefer S and M sizes.
- When using Replit Agent, paste individual TASK descriptions as prompts for best results.
- Use Replit's built-in file comments and TODO markers to track in-progress tasks.
