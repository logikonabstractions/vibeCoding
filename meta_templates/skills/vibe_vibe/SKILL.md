---
name: vibe
description: "vibeCoding vibe mode. Two sub-modes: VIBE-DRAFT (plan stages & checkpoints for a component) and VIBE-IMPLEMENT (implement a specific checkpoint). Use when the user says 'vibe mode', provides a component ID like '10.2' (triggers draft), or a checkpoint ID like '10.2.1.1' (triggers implement)."
argument-hint: [component-id | checkpoint-id | --list] — e.g. "10.2" to draft, "10.2.1.1" to implement, "--list" to see open checkpoints
---

# Vibe Mode

## Setup

Before doing anything else:
1. Read `agents.md` (repo root)
2. Read `.vibe/agents_vibe.md`
3. Read `.vibe/state.md` — check current checkpoint, status, active issues
4. Read `.vibe/plan.md`
5. Read `.vibe/context.md`
6. Read `.vibe/history.md` (optional — skip if session is a quick status check)
7. Read `.vibe/plan_history.md` (optional — skip if session is a quick status check) for reference (archived checkpoints and stages)

Then load additional context based on the sub-mode (see below).

Follow the instruction precedence defined in `.vibe/agents_vibe.md`.

## Determining the sub-mode

| $ARGUMENTS pattern | Sub-mode |
|-------------------|----------|
| *(empty)* | Read `.vibe/state.md`, summarize current status, ask what to do |
| `continue` | Resume from `.vibe/state.md` — detect sub-mode from current state |
| Component ID — format `N.N` e.g. `10.2` | **VIBE-DRAFT** |
| Checkpoint ID — format `N.N.N.N` e.g. `10.2.1.1` | **VIBE-IMPLEMENT** |
| Ambiguous | Ask to confirm before proceeding |
| `--list` | Read `.vibe/plan.md`. Output a concise table of all checkpoints that are NOT done: columns = checkpoint ID, name, stage, status. Group by stage. Also show the currently active checkpoint from `.vibe/state.md` highlighted at the top. |

---

## VIBE-DRAFT

**Goal**: given a component, write stages and checkpoints into `.vibe/plan.md`.

Additional files to read (after setup):
- `.architecture/architecture_description.md` (read-only reference)
- `.component/components_descriptions.md` — find the target component (read-only reference)

Use the template at `/meta_templates/.vibe/plan_tplt.md`.

A **stage** groups a few related checkpoints. A **checkpoint** is a small, reviewable diff — at most a few commits.

---

## VIBE-IMPLEMENT

**Goal**: implement the specified checkpoint according to `.vibe/plan.md`.

Additional files to read (after setup):
- `.vibe/context.md`

### Implementation discipline
- Limit changes to what is necessary to meet the checkpoint's acceptance criteria
- Commit coherent changes — one commit = one consistent set of changes towards the checkpoint
- Commit message format (mandatory): `<checkpoint-id> - <checkpoint name>`
- When done: update `.vibe/state.md`, append to `.vibe/history.md`

### Metafile rules
- NEVER remove checkpoints from `.vibe/plan.md` unless explicitly asked
- NEVER re-number checkpoints unless explicitly asked
- Update `.vibe/state.md` whenever focus shifts or a blocker is added/resolved
