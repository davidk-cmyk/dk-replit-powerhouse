---
name: spec
description: >
  Write software specification documents. Use when writing a spec, defining requirements, creating a PRD, capturing acceptance criteria, or describing what to build. Triggered by: write a spec, define requirements, document this feature, acceptance criteria, what should we build.
---

# Spec Skill

Write clear, complete, technology-agnostic software specifications that can be handed to any
engineer (or AI agent) to implement correctly the first time.

## What a Good Spec Contains

A spec answers: **What** should be built, **Why**, **Who** uses it, and **How** success is measured.
It does NOT prescribe implementation details unless they are constraints.

## Spec Template

```markdown
# Spec: [Feature / System Name]

## Status
Draft | In Review | Approved

## Problem Statement
One paragraph. What user pain or business need does this solve?
Why does it need to be solved now?

## Goals
- [ ] Goal 1 (measurable outcome)
- [ ] Goal 2

## Non-Goals (Out of Scope)
- Explicitly list what this does NOT cover to prevent scope creep.

## Users & Personas
Who uses this? Describe the primary user(s) in plain language.
Include any secondary users or system actors (e.g., cron jobs, external services).

## User Stories
As a [persona], I want to [action], so that [outcome].

Format each story as:
**US-001**: As a [user], I want [capability] so that [benefit].
  - Acceptance Criteria:
    - [ ] Given [context], when [action], then [outcome]
    - [ ] ...

## Functional Requirements
List what the system MUST do. Number them for traceability.

FR-001: The system shall...
FR-002: The system shall...

## Non-Functional Requirements
- **Performance**: e.g., "Response time < 200ms for 95th percentile"
- **Security**: e.g., "All data in transit must be encrypted"
- **Reliability**: e.g., "99.9% uptime SLA"
- **Scalability**: e.g., "Must support 10,000 concurrent users"
- **Accessibility**: e.g., "WCAG 2.1 AA compliant"

## Constraints & Assumptions
- Constraints: Hard limits (budget, deadline, existing systems to integrate with)
- Assumptions: Things believed to be true that should be validated

## Data Model (if applicable)
Describe the key data entities and their relationships in plain language or simple diagrams.
Avoid specifying database technology unless it is a hard constraint.

## Key Flows / Scenarios
Walk through the 2–3 most important user journeys step by step.
Use numbered steps. Include error/edge cases for each flow.

**Happy Path: [Flow Name]**
1. User does X
2. System responds with Y
3. ...

**Error Case: [Scenario]**
1. User does X
2. System detects Y condition
3. System responds with Z

## Open Questions
- [ ] Q1: [Question] — Owner: [Name] — Due: [Date]
- [ ] Q2: ...

## Success Metrics
How will we know this feature succeeded after launch?
- Metric 1: e.g., "Sign-up conversion rate increases by 10%"
- Metric 2: e.g., "Support tickets about X decrease by 50%"

## Dependencies
- Upstream: What must be done before this can start?
- Downstream: What is blocked until this is done?

## References
- Link to designs, prior art, related tickets, research docs
```

## How to Write Each Section Well

### Problem Statement
- Start with the user, not the technology.
- Quantify the pain if possible ("users abandon checkout at 40%").
- One paragraph max.

### User Stories + Acceptance Criteria
- Each story = one independently testable behavior.
- Acceptance criteria use Given/When/Then format.
- These become your test cases in TDD — write them precisely.

### Functional Requirements
- Use "shall" for mandatory, "should" for preferred.
- One requirement per line. Avoid "and" — split compound requirements.
- Every FR should map to at least one User Story.

### Open Questions
- Capture every unresolved assumption immediately.
- Assign an owner and a resolution date.
- A spec with no open questions is either perfect or ignoring problems.

## Process

1. **Start async** — Draft the spec alone first; iterate with stakeholders after.
2. **Review loop** — Share with: engineer, PM, design, QA (at minimum).
3. **Approval gate** — No tickets or code until the spec is Approved.
4. **Living doc** — Update the spec when requirements change; don't let it go stale.

## IDE / Environment Notes
- Store specs as Markdown files in a `/docs/specs/` folder in the repo.
- Use your project file tree to keep specs co-located with the code they describe.
- Link spec files in your your project README so the AI agent has context when building.
