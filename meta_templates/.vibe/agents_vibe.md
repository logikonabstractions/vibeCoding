# VIBE Workflow Contract

## Purpose

Translate **one component** into implementable stages and checkpoints, then implement them. This mode has two sub-modes: **VIBE-DRAFT** (planning) and **VIBE-IMPLEMENT** (coding).

## Input requirements

**MUST contain one** of the following:

- **A component identifier** (ID or name, e.g. "component 10.2") → triggers **VIBE-DRAFT**
- **A checkpoint identifier** (e.g. "checkpoint 10.2.1.1") matching an entry in `.vibe/plan.md` → triggers **VIBE-IMPLEMENT**

If the input is ambiguous, **always ask to confirm** which sub-mode applies.


## VIBE-DRAFT — Plan stages and checkpoints for a component

Given a component identifier, read its description from `.component/components_descriptions.md` and draft `.vibe/plan.md`. Organize work into **stages** (groups of related implementation steps) and **checkpoints** (small, reviewable units within a stage).

### Instruction precedence & read order
1. As specified by `agents.md`
2. This file
3. `.architecture/architecture_description.md` (read-only reference)
4. `.component/components_descriptions.md` (read-only reference)
5. `.vibe/plan.md`
6. `.vibe/state.md`
7. `.vibe/context.md`
8. `.vibe/history.md` (optional — skip if session is a quick status check)

---

## VIBE-IMPLEMENT — Implement a specific checkpoint

Given a checkpoint identifier, implement it according to `.vibe/plan.md`.

### Instruction precedence & read order
1. As specified by `agents.md`
2. This file
3. `.vibe/plan.md`
4. `.vibe/state.md`
5. `.vibe/context.md`
6. `.vibe/history.md` (optional — skip if session is a quick status check)
7. `.vibe/plan_history.md` (optional — skip if session is a quick status check) for reference (archived checkpoints and stages)

---

## Meta-templates

Found under `/meta_templates/.vibe`

| File | Role |
|------|------|
| `/meta_templates/.vibe/state_tplt.md` | Current checkpoint, status, work log, active issues, decisions |
| `/meta_templates/.vibe/plan_tplt.md` | Ordered list of checkpoints with objectives and acceptance criteria |
| `/meta_templates/.vibe/history_tplt.md` | Completed checkpoints and decisions |
| `/meta_templates/.vibe/context_tplt.md` | Shared context: architecture notes, key decisions, gotchas, hot files |
| `/meta_templates/.vibe/plan_history_tplt.md` | Archived DONE/CANCELLED checkpoint blocks moved from `plan.md` by `/vibe_cleanup` |

## Scope and cadence
- A **stage** groups a few related checkpoints.
- A **checkpoint** is a small, reviewable diff — at most a few commits.
- Limit changes to what is necessary to meet acceptance criteria.

## Metafile updates
- Update the STATUS of a checkpoint when it changes to keep it current.
- New Checkpoint have by default a PLANNED status.
- When a checkpoint moves to DONE, update `.vibe/state.md` to reflect the new state.
- NEVER remove checkpoints from `.vibe/plan.md` unless explicitly asked to.
- NEVER re-number checkpoints unless explicitly asked to.

## Plan cleanup

Over time `.vibe/plan.md` accumulates `[DONE]` and `[CANCELLED]` checkpoint blocks. The skill `/vibe_cleanup` may be used to archive them to `plan_history.md`. You should only ever perform that upon explicit request.

## Version control policy
- The commit message MUST ALWAYS follow this format for `VIBE-IMPLEMENT` (and if you can, the PR title/message as well): <checkpoint-id> - <checkpoint name>
- Commit coherent changes: a commit should be a single set of related and consistent changes towards a given checkpoint.

## Best practices & preferences
- Use local environement whenever possible (virtualenv for python, local node_module for js, etc.) and avoid polluting system-wide environments
