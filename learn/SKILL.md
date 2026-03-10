---
name: learn
description: >
  Capture knowledge, decisions, patterns, and lessons learned from completed work and embed
  them into the project's context files so the agent retains institutional memory across sessions.
  Use this skill after completing a feature, finishing a debugging session, making an architectural
  decision, or any time significant knowledge was generated that should persist. Trigger when the
  user says "learn from this", "capture what we did", "update context", "document what we learned",
  "save this knowledge", "remember this for next time", "what did we learn", or "update the
  project memory". Also runs automatically as the final phase of the orchestrator pipeline.
---

# Learn Skill

Capture decisions, patterns, gotchas, and project knowledge from completed work and persist
them into the project's context so the agent (and future engineers) benefit from what was learned.

## Learn Principles

1. **Knowledge decays** — If it's not written down, it's lost next session.
2. **Context over conclusion** — Record the *why* behind decisions, not just the *what*.
3. **Update, don't append forever** — Improve existing docs rather than creating new ones when possible.
4. **Actionable over anecdotal** — Every lesson should help someone make a better decision next time.
5. **Embedded over external** — Put knowledge where it will be found — near the code, in the README, in config comments — not buried in a wiki nobody reads.

## What to Capture

After any significant work session, extract knowledge in these categories:

### 1. Decisions Made
What was decided and why. What alternatives were considered and rejected.

```markdown
## Decision: [Short title]
**Date**: [YYYY-MM-DD]
**Context**: [What situation prompted this decision]
**Decision**: [What was decided]
**Alternatives considered**:
- [Alternative A]: [Why rejected]
- [Alternative B]: [Why rejected]
**Consequences**: [What follows from this decision — tradeoffs accepted]
```

### 2. Patterns Discovered
Coding patterns, architectural patterns, or workflow patterns that emerged and should be reused.

```markdown
## Pattern: [Short title]
**Context**: [When to use this pattern]
**Implementation**: [How to apply it]
**Example**: [Code snippet or reference to file]
**Avoid**: [Common mistakes or anti-patterns]
```

### 3. Gotchas and Pitfalls
Things that were surprisingly hard, broke unexpectedly, or wasted time.

```markdown
## Gotcha: [Short title]
**What happened**: [Brief description]
**Root cause**: [Why it happened]
**Fix/Workaround**: [How it was resolved]
**Prevention**: [How to avoid it next time]
```

### 4. Project Conventions
Naming conventions, file organization patterns, testing approaches, or workflow norms that
were established or reinforced.

```markdown
## Convention: [Short title]
**Rule**: [The convention in one sentence]
**Rationale**: [Why this convention exists]
**Examples**: [Good and bad examples]
```

### 5. Environment and Tooling Notes
Setup quirks, configuration that was non-obvious, CI/CD behavior, dependency gotchas.

```markdown
## Environment Note: [Short title]
**Context**: [What tool/system this relates to]
**Detail**: [The non-obvious thing]
**Impact**: [What breaks or gets confusing without this knowledge]
```

## Where to Persist Knowledge

Knowledge is only valuable if it's found when needed. Place it strategically:

| Knowledge Type | Where to Put It |
|----------------|-----------------|
| **Architecture decisions** | `docs/decisions/` as ADR files (ADR-001.md, etc.) |
| **Project conventions** | `CLAUDE.md`, `replit.md`, or project root config files |
| **API behavior / gotchas** | Inline comments near the relevant code |
| **Setup / environment** | `README.md` or `docs/setup.md` |
| **Patterns for reuse** | `docs/patterns.md` or `CLAUDE.md` |
| **Lessons from bugs** | `docs/decisions/` or inline comments at fix site |
| **Agent context** | `CLAUDE.md` and `replit.md` — persistent agent memory |

### CLAUDE.md as Agent Memory

The `CLAUDE.md` file (or equivalent project context file) is the primary place to store
knowledge that the agent needs across sessions. Update it with:

- Project-specific conventions the agent should follow
- Known gotchas the agent should avoid
- Architectural decisions that constrain future work
- Testing patterns specific to this project
- Build/deploy quirks

### replit.md as Replit Agent Memory

The `replit.md` file is the Replit agent's equivalent of `CLAUDE.md` — a markdown file in the
project root that the Replit AI agent reads at the start of every session for project context.
Update it alongside `CLAUDE.md` so knowledge is available regardless of which agent environment
is used.

**What to put in `replit.md`:**

- Project overview and architecture summary
- How to run, build, and test the project
- Project-specific conventions and constraints
- Known gotchas the agent should avoid
- Key architectural decisions that constrain future work
- Environment setup notes (ports, entry points, dependencies)
- Testing patterns specific to this project

**When to update `replit.md`:**
- New build/run/test commands were established or changed
- Project conventions were decided that the agent should follow
- Architecture decisions were made that constrain future work
- Environment setup changed (ports, entry points, dependencies)
- Gotchas were discovered that affect how code should be written or run

Keep `replit.md` and `CLAUDE.md` in sync — they serve the same purpose for different agent
environments. When you update one, check if the other needs the same update.

**Format for CLAUDE.md entries:**

```markdown
## [Category]

- [Concise, actionable statement]. [Brief rationale if not obvious.]
- [Another statement.]
```

Keep entries concise and scannable. The agent reads this at the start of every session,
so respect its attention — remove outdated entries, merge duplicates, keep it tight.

## Learn Workflow

### After a Feature (Orchestrator Phase 8)

Run through this checklist:

```
1. REVIEW  what was built — read the spec, plan, code, and test results
2. EXTRACT decisions, patterns, gotchas, and conventions (use categories above)
3. CHECK   existing knowledge files — is this an update or net-new?
4. WRITE   new entries or update existing files
5. PRUNE   remove anything now outdated or superseded
6. COMMIT  knowledge changes with message: "learn: capture [feature] knowledge"
```

### After a Debugging Session

```
1. IDENTIFY the root cause and the fix
2. DOCUMENT as a Gotcha (see template above)
3. ADD     a code comment at the fix site explaining the non-obvious behavior
4. UPDATE  CLAUDE.md and replit.md if this is a recurring or project-wide concern
```

### After an Architectural Decision

```
1. WRITE   an ADR (Architecture Decision Record) in docs/decisions/
2. UPDATE  CLAUDE.md and replit.md with the constraint this creates
3. UPDATE  README.md if it affects setup, build, or usage
```

### Periodic Knowledge Maintenance

Every few features or at natural milestones:

- [ ] Review `CLAUDE.md` — remove stale entries, consolidate duplicates
- [ ] Review `replit.md` — keep in sync with `CLAUDE.md`
- [ ] Review `docs/decisions/` — mark superseded ADRs
- [ ] Review inline code comments — remove any that are now obvious or wrong
- [ ] Check README — does it still reflect how to set up and run the project?

## ADR (Architecture Decision Record) Template

Store in `docs/decisions/ADR-NNN.md`:

```markdown
# ADR-NNN: [Decision Title]

## Status
Accepted | Superseded by ADR-XXX | Deprecated

## Date
YYYY-MM-DD

## Context
What is the issue we're facing? What forces are at play?

## Decision
What did we decide to do?

## Consequences

### Positive
- [benefit]

### Negative
- [tradeoff accepted]

### Neutral
- [side effect worth noting]
```

## Integration with the Orchestrator

When run as Phase 8 of the orchestrator, the learn skill receives context from all prior phases:

- **From spec**: Requirements decisions, scope choices, non-goals rationale
- **From plan**: Task breakdown approach, estimation accuracy, dependency insights
- **From tdd**: Testing patterns used, what was hard to test and why
- **From build**: Implementation patterns, library choices, code structure decisions
- **From review**: Quality findings, recurring issues, conventions established
- **From test**: Integration challenges, environment quirks, flaky test causes
- **From tech-writer**: Documentation gaps discovered, API surface observations

Synthesize across all phases — the most valuable learnings often span multiple phases
(e.g., "We specced X but discovered during build that Y was a better approach because Z").

## Examples of Good Learn Entries

**Good** (actionable, contextual):
```
- Use `node:test` instead of Jest for this project — Jest config conflicts with our ESM setup
  and adds 15s to CI. Native test runner works with zero config. (ADR-003)
```

**Bad** (vague, no context):
```
- Jest is bad, use something else.
```

**Good** (specific gotcha):
```
- The Stripe webhook endpoint must return 200 within 5 seconds or Stripe retries.
  Our initial handler did DB writes before responding — moved to async processing. (See src/payments/webhook.ts:42)
```

**Bad** (too abstract):
```
- Webhooks can time out if you're not careful.
```
