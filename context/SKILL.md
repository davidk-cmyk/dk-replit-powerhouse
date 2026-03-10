---
name: context
description: >
  Subject matter expert that provides project-specific context to all other skills. This skill
  is consulted BEFORE the orchestrator or spec skill begins work. It manages the `.context/`
  folder — a collection of markdown files that capture domain knowledge, project history,
  architectural context, stakeholder requirements, and lessons from prior conversations.
  Trigger when: a new feature pipeline starts (automatically invoked by orchestrator and spec),
  the user says "update context", "add this to context", "what do we know about [topic]",
  "inject context", "refresh context", "save this context", or "what's the project context".
  Also runs automatically as Phase 0 of the orchestrator pipeline.
---

# Context Skill

Serve as the project's subject matter expert. Maintain, inject, and evolve the `.context/`
folder — a living knowledge base of markdown files that gives every other skill the domain
understanding it needs to make good decisions.

## Context Principles

1. **Context before action** — No skill should start work without first consulting context. Bad context leads to bad specs, bad plans, and wasted builds.
2. **Structured over scattered** — All project context lives in `.context/` as organized markdown files, not spread across random docs or chat history.
3. **Evolve continuously** — Context is not static. Every meaningful conversation, decision, or discovery should flow back into `.context/`.
4. **Relevant over exhaustive** — Inject only the context that matters for the current task. Don't dump everything — curate.
5. **Single source of truth** — If it contradicts `.context/`, the `.context/` file is stale and must be updated, not ignored.

## The `.context/` Folder Structure

```
.context/
├── PROJECT.md          # Project overview, mission, core domain concepts
├── ARCHITECTURE.md     # System architecture, tech stack, key design decisions
├── DOMAIN.md           # Domain glossary, business rules, domain model
├── STAKEHOLDERS.md     # Who the users are, what they care about, personas
├── CONVENTIONS.md      # Coding conventions, naming patterns, file organization
├── HISTORY.md          # Timeline of major decisions, pivots, and milestones
├── INTEGRATIONS.md     # External services, APIs, third-party dependencies
├── CONSTRAINTS.md      # Hard limits — budget, timeline, platform, compliance
└── [topic].md          # Additional topic-specific context files as needed
```

Not all files are required. Start with what matters and grow organically.

## File Templates

### PROJECT.md

```markdown
# Project Context

## What This Project Is
[One paragraph — what it does, who it's for, why it exists]

## Core Domain Concepts
- **[Concept]**: [Definition in plain language]
- **[Concept]**: [Definition]

## Current State
[What exists today — features shipped, what's working, what's not]

## North Star
[Where this project is heading — the vision in 1-2 sentences]
```

### ARCHITECTURE.md

```markdown
# Architecture Context

## Tech Stack
- **Language**: [e.g., TypeScript]
- **Framework**: [e.g., Next.js]
- **Database**: [e.g., PostgreSQL via Prisma]
- **Hosting**: [e.g., Replit / Vercel]

## System Shape
[High-level description or ASCII diagram of how components connect]

## Key Design Decisions
- [Decision]: [Why — link to ADR if exists]

## Boundaries
- [What the system does NOT do and why]
```

### DOMAIN.md

```markdown
# Domain Context

## Glossary
| Term | Definition | Example |
|------|-----------|---------|
| [Term] | [Plain language definition] | [Concrete example] |

## Business Rules
- BR-001: [Rule in plain language]
- BR-002: [Rule]

## Domain Model
[Describe key entities and relationships — can use text or simple diagrams]
```

### STAKEHOLDERS.md

```markdown
# Stakeholders & Users

## Primary Users
- **[Persona name]**: [Who they are, what they need, what frustrates them]

## Secondary Users
- **[Persona/system]**: [Role and interaction pattern]

## Decision Makers
- **[Role]**: [What they care about, what they approve]
```

### CONVENTIONS.md

```markdown
# Project Conventions

## Code Style
- [Convention]: [Rationale]

## File Organization
- [Pattern]: [Where things go and why]

## Naming
- [Pattern]: [Examples]

## Testing
- [Approach]: [What to test, how, what tools]

## Git / Workflow
- [Convention]: [Branch naming, commit style, PR process]
```

### HISTORY.md

```markdown
# Project History

## Timeline
- **[YYYY-MM-DD]**: [What happened — decision, pivot, milestone, or lesson]
- **[YYYY-MM-DD]**: [Event]

## Key Pivots
- **From [A] to [B]**: [Why the change was made, what was learned]
```

## How Context Gets Used

### Phase 0: Context Injection (Before Spec)

When the orchestrator starts a new pipeline or the spec skill begins work:

```
1. SCAN    .context/ folder — read all files
2. ASSESS  which context files are relevant to the current task
3. INJECT  relevant context as a briefing for the next skill
4. FLAG    any stale or missing context that should be updated
5. ADVISE  the spec/orchestrator of constraints, prior decisions,
           domain rules, or conventions that affect this work
```

**Output format** — deliver context as a structured briefing:

```markdown
## Context Briefing: [Feature/Task Name]

### Relevant Domain Knowledge
- [Key domain concepts and business rules that apply]

### Prior Decisions That Constrain This Work
- [Decision]: [Impact on current task]

### Conventions to Follow
- [Convention]: [Why it matters here]

### Stakeholder Context
- [Who cares about this and what they expect]

### Risks / Watch-outs
- [Things that went wrong before in similar work]
- [Constraints that are easy to miss]

### Missing Context
- [Questions that should be answered before proceeding]
```

### During Conversations: Context Capture

After any conversation that produces valuable context:

```
1. IDENTIFY  new knowledge — decisions, domain clarifications, constraints,
             user feedback, architectural insights
2. CLASSIFY  which .context/ file it belongs in (or create a new topic file)
3. UPDATE    the file — append new entries, revise stale ones, remove outdated info
4. LOG       what was updated in HISTORY.md with date
5. NOTIFY    the user: "Updated .context/[FILE].md with [brief summary]"
```

### Periodic Context Maintenance

Every few features or at natural milestones:

- [ ] Review all `.context/` files — remove stale entries, consolidate duplicates
- [ ] Cross-reference with `CLAUDE.md` and `replit.md` — keep aligned
- [ ] Check that DOMAIN.md glossary matches actual codebase terminology
- [ ] Verify ARCHITECTURE.md reflects current system shape
- [ ] Ensure CONSTRAINTS.md is still accurate (timelines, budgets, compliance)

## Context Skill as SME

When consulted by other skills, the context skill acts as a **subject matter expert**:

| Skill Asking | What Context Provides |
|--------------|----------------------|
| **orchestrator** | Overall project state, what's been built, what constraints apply to the pipeline |
| **spec** | Domain knowledge, business rules, user personas, prior decisions that constrain requirements |
| **plan** | Architecture context, tech stack, conventions that affect task breakdown and estimates |
| **tdd** | Testing conventions, domain edge cases, business rules that need test coverage |
| **build** | Code conventions, architecture patterns, integration details, existing patterns to follow |
| **code-review** | Conventions to enforce, known gotchas, security constraints |
| **learn** | What context files need updating based on what was learned |

## Creating New Context Files

When a topic doesn't fit existing files, create a new one:

```
.context/[topic-name].md
```

Rules:
- Use lowercase kebab-case for filenames
- Start with a `# [Topic] Context` heading
- Include a one-line description of what this file covers
- Keep it focused — one topic per file
- Add the new file to HISTORY.md

## Syncing with CLAUDE.md and replit.md

`.context/` is the **deep** knowledge base. `CLAUDE.md` and `replit.md` are the **summary** layer.

```
.context/  (detailed, structured, many files)
    ↓ distill key points
CLAUDE.md / replit.md  (concise, scannable, single file)
```

When `.context/` is updated:
1. Check if the update affects what should be in `CLAUDE.md` or `replit.md`
2. If so, update them with a concise summary — don't duplicate the full detail
3. Link back to the `.context/` file for anyone who needs depth

## Quick Start

When invoked directly by the user:

**"What do we know about [topic]?"** — Search `.context/` files, synthesize relevant knowledge, present it clearly.

**"Update context with [info]"** — Classify the info, update the right `.context/` file, confirm what was added.

**"Refresh context"** — Review all `.context/` files, flag stale entries, suggest updates.

**"Inject context for [task]"** — Read `.context/`, curate relevant knowledge, deliver a context briefing.

## Replit-Specific Notes
- Store `.context/` in the project root alongside `replit.md`.
- The `.context/` folder is version-controlled — it evolves with the codebase.
- When the Replit agent starts a session, it should read `replit.md` which should reference `.context/` for deeper knowledge.
- Keep `replit.md` as a lightweight summary; let `.context/` hold the depth.
