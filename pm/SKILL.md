---
name: pm
description: >
  Track issues, manage project progress, and maintain a living backlog throughout the development
  lifecycle. Use this skill whenever an engineer needs to log a bug, track a feature request,
  triage issues, update project status, manage a backlog, prioritize work, track blockers,
  write a status update, or keep stakeholders informed on progress. Trigger when the user says
  "log this issue", "track this bug", "update the backlog", "what's the status of", "triage
  these issues", "prioritize the backlog", "write a status update", "what's blocking us",
  "create an issue for this", or "project status". This skill runs continuously alongside
  every other skill — every spec change, plan update, build task, code review finding, and
  test failure should be reflected in the issue tracker.
---

# PM Skill

Keep the project's issue tracker accurate, prioritized, and actionable at all times.
Every change in the codebase should have a corresponding issue, and every issue should
reflect the current state of reality.

## PM Principles

1. **Single source of truth** — The issue tracker is the canonical record of what's done,
   what's in progress, and what's next. If it's not in the tracker, it doesn't exist.
2. **Issues before code** — Every code change traces back to an issue. No orphan commits.
3. **Current over complete** — A short, up-to-date tracker beats a detailed stale one.
4. **Blockers surface immediately** — The moment something is blocked, it's flagged and visible.
5. **Close the loop** — Every issue gets resolved, deferred, or explicitly canceled. Nothing rots.

## Issue Template

```markdown
# [ISSUE-XXX]: [Short imperative title]

## Type
Bug | Feature | Task | Spike | Chore

## Status
Open | In Progress | In Review | Blocked | Done | Deferred | Canceled

## Priority
P0 (Critical) | P1 (High) | P2 (Medium) | P3 (Low)

## Description
What is this issue about? One paragraph max.

## Acceptance Criteria
- [ ] AC-1: [Testable outcome]
- [ ] AC-2: ...

## Linked Artifacts
- Spec: [link or file path]
- Plan Task: TASK-XXX
- PR/Commit: [reference]
- Blocking/Blocked by: ISSUE-YYY

## Notes / Context
Any additional context, screenshots, reproduction steps, or discussion.

## History
| Date | Author | Action |
|------|--------|--------|
| YYYY-MM-DD | [Name] | Created |
| YYYY-MM-DD | [Name] | Status → In Progress |
```

## Issue Types

| Type | When to Use | Example |
|------|-------------|---------|
| **Bug** | Something is broken or behaving incorrectly | "Login returns 500 when email has a plus sign" |
| **Feature** | New user-facing functionality | "Add password reset flow" |
| **Task** | Engineering work that isn't user-visible | "Set up CI pipeline" |
| **Spike** | Time-boxed research to reduce uncertainty | "Evaluate payment providers (2hr max)" |
| **Chore** | Maintenance, upgrades, cleanup | "Upgrade Node.js to v20" |

## Priority Definitions

| Priority | Meaning | Response Time |
|----------|---------|---------------|
| **P0 — Critical** | System down, data loss, security breach | Drop everything; fix now |
| **P1 — High** | Major feature broken, blocking other work | Fix within current sprint |
| **P2 — Medium** | Important but not urgent; workaround exists | Schedule in next sprint |
| **P3 — Low** | Nice to have, minor polish, tech debt | Backlog; do when capacity allows |

## Backlog Management

### Triage Process
When new issues come in:
1. **Classify** — Assign type (Bug / Feature / Task / Spike / Chore).
2. **Prioritize** — Assign priority (P0–P3) based on impact and urgency.
3. **Size** — Estimate effort (S / M / L / XL). XL must be split.
4. **Link** — Connect to relevant spec, plan task, or parent issue.
5. **Assign** — Assign an owner or leave unassigned for backlog.

### Backlog Hygiene (Weekly)
- [ ] Review all Open issues — are they still relevant?
- [ ] Close or Defer anything that's been Open for 2+ sprints with no progress.
- [ ] Re-prioritize based on current project goals.
- [ ] Ensure every In Progress issue has an owner.
- [ ] Check that Blocked issues have a clear unblocking path documented.

### Backlog File Structure
Store the backlog as a Markdown file in the repo:
```
/docs/
  backlog.md          ← master issue list
  issues/
    ISSUE-001.md      ← individual issue files (for complex issues)
    ISSUE-002.md
```

For small projects, a single `backlog.md` with a table is sufficient:
```markdown
| ID | Type | Priority | Title | Status | Owner | Updated |
|----|------|----------|-------|--------|-------|---------|
| 001 | Bug | P1 | Login fails with + in email | In Progress | @dev | 2025-01-15 |
| 002 | Feature | P2 | Add password reset | Open | — | 2025-01-14 |
```

## Status Updates

### When to Write a Status Update
- End of each sprint or work session
- When a blocker is encountered or resolved
- When scope changes significantly
- Before stakeholder check-ins

### Status Update Template
```markdown
# Status Update: [Date]

## Summary
One sentence: what happened since last update.

## Completed
- [ISSUE-XXX] [Title] — Done
- [ISSUE-YYY] [Title] — Done

## In Progress
- [ISSUE-ZZZ] [Title] — [% complete or current step]

## Blocked
- [ISSUE-AAA] [Title] — Blocked by: [reason]. Unblock path: [action].

## Up Next
- [ISSUE-BBB] [Title] — Starting [when]

## Risks / Flags
- [Any new risks, scope changes, or timeline concerns]
```

## Integration with Other Skills

The PM skill is the connective tissue across the entire workflow:

| Workflow Step | PM Action |
|---------------|-----------|
| **spec** | Create Feature issues for each major capability in the spec |
| **plan** | Link TASK-XXX entries to corresponding issues; update estimates |
| **tdd** | Log Bug issues when tests reveal unexpected behavior |
| **build** | Move issues to In Progress / In Review as work progresses |
| **code-review** | Create new issues for review findings that aren't fixed immediately |
| **test** | Create Bug issues for test failures; update status when tests pass |
| **tech-writer** | Create Chore issues for documentation gaps discovered during docs review |

## Replit-Specific Notes
- Store `backlog.md` in `/docs/` so it's visible in the Replit file tree alongside specs and plans.
- When using Replit Agent: start each session by reviewing `backlog.md` to pick the next issue.
- Use Replit's file comments or inline TODOs to flag issues discovered while coding — then
  immediately add them to the backlog so they aren't lost.
- Link issue IDs in commit messages so the history is traceable: `fix: resolve ISSUE-012 login error`.
