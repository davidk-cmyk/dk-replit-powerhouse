# Software Engineering Skills — Index

A set of 6 technology-agnostic skills covering the full software development lifecycle.
Each skill is a SKILL.md file that can be installed into Claude to guide engineering work.
All skills are compatible with Replit and any AI agent environment.

## Skills

| Skill | Purpose | Use After |
|-------|---------|-----------|
| **spec** | Write software requirements & acceptance criteria | Nothing — start here |
| **plan** | Break spec into sequenced tasks with estimates | spec |
| **tdd** | Write failing tests to drive implementation | plan |
| **build** | Implement features systematically | tdd |
| **code-review** | Review code for correctness, security, quality | build |
| **test** | Integration, E2E, and pre-ship test strategy | code-review |

## The Development Loop

```
spec → plan → tdd → build → code-review → test → ship
  ↑___________________________|
        (iterate on next feature)
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
