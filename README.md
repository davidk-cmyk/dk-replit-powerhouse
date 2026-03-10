# Software Engineering Skills — Index

A set of 10 technology-agnostic skills covering the full software development lifecycle.
Each skill is a SKILL.md file that can be installed into Claude to guide engineering work.
All skills are compatible with Replit and any AI agent environment.

## Skills

### Core Workflow (Sequential)

| Skill | Purpose | Use After |
|-------|---------|-----------|
| **spec** | Write software requirements & acceptance criteria | Nothing — start here |
| **plan** | Break spec into sequenced tasks with estimates | spec |
| **tdd** | Write failing tests to drive implementation | plan |
| **build** | Implement features systematically | tdd |
| **code-review** | Review code for correctness, security, quality | build |
| **test** | Integration, E2E, and pre-ship test strategy | code-review |

### Orchestration

| Skill | Purpose | Use When |
|-------|---------|----------|
| **orchestrator** | Run the full pipeline end-to-end for a feature | Starting a feature from scratch |
| **learn** | Capture decisions, patterns, and knowledge | After completing work (or as orchestrator Phase 8) |

### Cross-Cutting (Continuous)

| Skill | Purpose | When |
|-------|---------|------|
| **pm** | Track issues, manage backlog, report status | Throughout — every step |
| **tech-writer** | Maintain docs, changelogs, API reference, ADRs | After every feature ships |

## The Development Loop

```
orchestrator (chains all phases automatically)
  ┌──────────────────────────────────────────────────────┐
  │ spec → plan → tdd → build → code-review → test      │
  │   ↑___________________________|                      │
  │         (iterate on next feature)                    │
  │                                                      │
  │ → tech-writer → learn                                │
  └──────────────────────────────────────────────────────┘

  pm ─────────────── runs continuously across all steps ───────────────
```

## How to Install

Each skill lives in its own folder with a `SKILL.md` file.
Install into Claude's skills directory to make them available in your project.

## Skill Descriptions (for triggering)

- **spec**: Triggered by: "write a spec", "define requirements", "document this feature", "PRD", "acceptance criteria"
- **plan**: Triggered by: "plan this feature", "break into tasks", "create tickets", "estimate this", "what order should I build"
- **tdd**: Triggered by: "write tests first", "TDD", "failing tests", "test-drive this", "define behavior with tests"
- **build**: Triggered by: "implement this", "write the code", "set up the project", "scaffold", "start coding"
- **code-review**: Triggered by: "review this code", "PR review", "find bugs", "self-review", "check my code"
- **test**: Triggered by: "integration tests", "E2E tests", "test strategy", "test coverage", "validate before shipping"
- **pm**: Triggered by: "log this issue", "track this bug", "update the backlog", "project status", "triage issues", "what's blocking us"
- **tech-writer**: Triggered by: "update the docs", "write documentation", "add to the changelog", "document this API", "write a README", "docs are out of date"
- **orchestrator**: Triggered by: "build this end to end", "full pipeline", "spec to ship", "orchestrate this feature", "build from scratch", "do the whole thing"
- **learn**: Triggered by: "learn from this", "capture what we did", "update context", "save this knowledge", "remember this", "what did we learn", "update project memory"
