---
name: tech-writer
description: >
  Maintain thorough, accurate project documentation, changelogs, and user-facing guides that stay
  in sync with the codebase. Use this skill whenever an engineer needs to write or update
  documentation, maintain a changelog, document an API, write a README, create onboarding docs,
  update docs after a feature ships, write a migration guide, document architecture decisions,
  or ensure that documentation reflects the current state of the code. Trigger when the user says
  "update the docs", "write documentation for this", "add to the changelog", "document this API",
  "write a README", "what needs documenting", "docs are out of date", "write a migration guide",
  "document this decision", or "update the architecture docs". This skill runs after every
  feature is built and reviewed — no feature is truly done until it's documented.
---

# Tech Writer Skill

Keep project documentation complete, accurate, and current. Documentation is not an afterthought —
it is a deliverable. A feature without documentation is an undiscoverable feature.

## Tech Writer Principles

1. **Docs ship with code** — Every feature PR includes documentation updates. No exceptions.
2. **Current over comprehensive** — Short, accurate docs beat long, stale ones.
3. **Write for the reader** — Developers reading docs want to do something. Lead with the action.
4. **Single source of truth** — One canonical location per topic. No duplicated explanations.
5. **Changelog is sacred** — Every user-visible change gets a changelog entry. Period.

## Documentation Inventory

Every project should maintain these core documents:

| Document | Purpose | Update Frequency |
|----------|---------|-----------------|
| **README.md** | Project overview, setup, quick start | Every major change |
| **CHANGELOG.md** | User-visible changes per release | Every feature/fix |
| **docs/architecture.md** | System design and key decisions | When architecture changes |
| **docs/api.md** | API reference (endpoints, params, responses) | Every API change |
| **docs/setup.md** | Detailed development environment setup | When deps or tooling change |
| **docs/guides/** | How-to guides for common tasks | When new workflows are added |
| **docs/adrs/** | Architecture Decision Records | When significant decisions are made |

## Changelog

### Changelog Format (Keep a Changelog)

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/).

## [Unreleased]

### Added
- New features that have been added

### Changed
- Changes to existing functionality

### Deprecated
- Features that will be removed in future versions

### Removed
- Features that have been removed

### Fixed
- Bug fixes

### Security
- Vulnerability fixes

## [1.0.0] - YYYY-MM-DD

### Added
- Initial release with [feature list]
```

### Changelog Rules
- Write entries from the **user's perspective**, not the developer's.
  - ✅ "Added password reset via email"
  - ❌ "Refactored AuthService to extract resetPassword method"
- One line per change. Be specific but concise.
- Group entries under the correct category (Added / Changed / Fixed / etc.).
- Internal refactors, test changes, and chores do NOT go in the changelog unless they affect users.
- Link to issues or PRs where helpful: `Fixed login error for emails with + character ([#12])`

### When to Update the Changelog
- After every merged feature, fix, or user-visible change.
- Before every release — review [Unreleased] and move entries to the new version section.
- During code review — verify the changelog is updated as part of the review checklist.

## README

### README Template

```markdown
# [Project Name]

One-sentence description of what this project does and who it's for.

## Quick Start

\`\`\`bash
# Clone and install
git clone [repo-url]
cd [project]
[install command]

# Configure
cp .env.example .env
# Edit .env with your values

# Run
[run command]
\`\`\`

## Features
- Feature 1: brief description
- Feature 2: brief description

## Development

### Prerequisites
- [Runtime] version X.X+
- [Database] version X.X+ (if applicable)
- [Other tools]

### Setup
Step-by-step instructions to get a development environment running.

### Running Tests
\`\`\`bash
[test command]
\`\`\`

### Project Structure
\`\`\`
src/           — application source code
tests/         — test suites
docs/          — documentation
scripts/       — utility scripts
\`\`\`

## API Reference
Link to full API docs or brief summary of key endpoints.

## Contributing
How to contribute: branching strategy, PR process, coding standards.

## License
[License type]
```

### README Rules
- Keep it under 200 lines. Link to detailed docs for deep dives.
- The Quick Start section should get someone running in under 5 minutes.
- Update Prerequisites whenever dependencies change.
- Test the Quick Start instructions on a clean environment periodically.

## API Documentation

### API Endpoint Template

```markdown
## [METHOD] /path/to/endpoint

**Description**: What this endpoint does in one sentence.

**Authentication**: Required | Optional | None

### Request

**Headers**:
| Header | Required | Description |
|--------|----------|-------------|
| Authorization | Yes | Bearer token |

**Path Parameters**:
| Parameter | Type | Description |
|-----------|------|-------------|
| id | string | The resource ID |

**Query Parameters**:
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| limit | integer | 20 | Max results to return |

**Body** (JSON):
\`\`\`json
{
  "email": "user@example.com",
  "name": "Jane Doe"
}
\`\`\`

### Response

**200 OK**:
\`\`\`json
{
  "id": "abc123",
  "email": "user@example.com",
  "name": "Jane Doe",
  "createdAt": "2025-01-15T10:30:00Z"
}
\`\`\`

**400 Bad Request**:
\`\`\`json
{
  "error": "VALIDATION_ERROR",
  "message": "Email is required"
}
\`\`\`

**404 Not Found**:
\`\`\`json
{
  "error": "NOT_FOUND",
  "message": "User not found"
}
\`\`\`
```

## Architecture Decision Records (ADRs)

### ADR Template

```markdown
# ADR-XXX: [Decision Title]

## Status
Proposed | Accepted | Deprecated | Superseded by ADR-YYY

## Context
What is the issue or question that prompted this decision?
What constraints exist?

## Decision
What is the decision and why?

## Consequences
### Positive
- [Benefit 1]
- [Benefit 2]

### Negative
- [Trade-off 1]
- [Trade-off 2]

### Neutral
- [Side effect that is neither clearly positive nor negative]
```

### When to Write an ADR
- Choosing a framework, library, or major dependency
- Deciding on an architectural pattern (monolith vs. microservices, REST vs. GraphQL)
- Changing a previously documented decision
- Any decision someone might reasonably question in 6 months

## Documentation Review Checklist

Run this checklist after every feature is built and before marking it as shipped:

### After Every Feature
- [ ] README updated if setup steps, features, or structure changed
- [ ] CHANGELOG.md updated with user-visible changes under [Unreleased]
- [ ] API docs updated if any endpoints were added, changed, or removed
- [ ] Inline code comments updated if behavior changed in non-obvious ways
- [ ] Any referenced file paths or config keys still valid

### After Every Release
- [ ] CHANGELOG.md [Unreleased] entries moved to new version section
- [ ] README version badge updated (if applicable)
- [ ] Migration guide written if there are breaking changes
- [ ] Architecture docs updated if system design changed
- [ ] ADR written for any significant decisions made during this release

### Periodic Review (Monthly)
- [ ] Read through all docs — flag anything that contradicts the current codebase
- [ ] Check that Quick Start instructions still work on a clean setup
- [ ] Remove docs for features that no longer exist
- [ ] Ensure no doc references stale file paths, removed APIs, or old config keys

## Integration with Other Skills

The Tech Writer skill ensures documentation stays in sync across the entire workflow:

| Workflow Step | Tech Writer Action |
|---------------|-------------------|
| **spec** | Verify spec is stored in `/docs/specs/` and linked from README |
| **plan** | Verify plan is stored in `/docs/plans/` and linked from spec |
| **tdd** | No action — tests are self-documenting |
| **build** | Update README, API docs, and inline comments for new/changed code |
| **code-review** | Check that docs are updated as part of the review checklist |
| **test** | No action unless test setup instructions changed |
| **pm** | Create Chore issues for any documentation gaps discovered |

## Replit-Specific Notes
- Store all docs in `/docs/` — Replit's file tree makes them easy to navigate alongside code.
- Keep `CHANGELOG.md` at the repo root so it's immediately visible.
- When using Replit Agent: after building a feature, prompt the agent with
  "Update documentation for the feature we just built" and provide the doc checklist above.
- Use Replit's Markdown preview to verify docs render correctly before committing.
- Link doc files in your `.replit` configuration or README so they're discoverable by the AI agent.
