---
name: vibe_trace
description: "vibeCoding trace mode. Given any ID (architectural element, component, stage, or checkpoint), outputs a compact snapshot: the item's parent context, its own summary, and its direct children. Read-only — does not modify any files."
argument-hint: <id> — e.g. "10", "10.2", "10.2.1", "10.2.1.1"
---

# Vibe Trace

## Setup

Read `agents.md` (repo root) for file location conventions.

## ID detection

Detect the ID type from the argument structure:

| Pattern | Type | Example |
|---------|------|---------|
| Single integer | Architectural element | `10`, `20` |
| `N.N` | Component | `10.2` |
| `N.N.N` | Stage | `10.2.1` |
| `N.N.N.N` | Checkpoint | `10.2.1.1` |

If no argument is given, ask the user which ID to trace.

## Files to read

Always these three only:
- `.architecture/architecture_description.md`
- `.component/components_descriptions.md`
- `.vibe/plan.md`

## Scoping rules

| Passed ID | Arch element | Components | Stages / checkpoints |
|-----------|-------------|------------|----------------------|
| `10` | element 10 | all 10.x | all 10.x.y and their checkpoints |
| `10.2` | element 10 | only 10.2 | all 10.2.x and their checkpoints |
| `10.2.1` | element 10 | only 10.2 | only stage 10.2.1 and its checkpoints |
| `10.2.1.1` | element 10 | only 10.2 | only stage 10.2.1 + checkpoint 10.2.1.1 |

## Output format

Produce a tight, readable snapshot in this order:

**1. Architectural element** (from `### Description` in `architecture_description.md`)

```
## [ID] — [Element name]
Category: [value]  |  Purpose: [value]
Responsibilities:
  - [bullet]
  - [bullet]
```

**2. Component(s)** (from `### Description` in `components_descriptions.md`)

If more than one component is in scope, open with a one-liner index:
```
Components: 10.1 — Name, 10.2 — Name, 10.3 — Name
```
Then for each component in scope:
```
### [ID] — [Component name]
Category: [value]  |  Purpose: [value]  |  Technology: [value]
```

**3. Stages & checkpoints** (from `plan.md`, objective field only)

```
Stage [ID] — [name]  [STATUS]
  [ID] — [checkpoint name]  [STATUS]
    Objective: [1 sentence]
  [ID] — [checkpoint name]  [STATUS]
    Objective: [1 sentence]
```

## Constraints

- Read-only: do not write, update, or append to any file.
- Do not load `state.md`, `history.md`, `discussion.md`, `context.md`, or `plan_history.md`.
