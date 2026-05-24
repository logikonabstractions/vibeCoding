# VIBE Workflow Contract

## Purpose

Shared rules for the vibe mode skills (`vibe_draft` and `vibe_implement`). Read by both skills as part of their setup step.

## Instruction precedence & read order

### VIBE-DRAFT

1. User instructions in chat
2. `agents.md` (repo root)
3. This file
4. `.vibe/state.md`
5. `.vibe/plan.md`
6. `.component/components_descriptions.md` (read-only reference)
7. `.architecture/architecture_description.md` (read-only reference)
8. `.vibe/history.md` (optional — skip if session is a quick status check)

### VIBE-IMPLEMENT

1. User instructions in chat
2. `agents.md` (repo root)
3. This file
4. `.vibe/state.md`
5. `.vibe/plan.md`
6. `.vibe/context.md`
7. `.vibe/history.md` (optional — skip if session is a quick status check)
8. `.vibe/plan_history.md` (optional — archived checkpoints for reference)

## Meta-templates

Found under `meta_templates/.vibe`

| File | Role |
|------|------|
| `meta_templates/.vibe/state_tplt.md` | Current checkpoint, status, work log, active issues, decisions |
| `meta_templates/.vibe/plan_tplt.md` | Ordered list of checkpoints with objectives and acceptance criteria |
| `meta_templates/.vibe/history_tplt.md` | Completed checkpoints and decisions |
| `meta_templates/.vibe/context_tplt.md` | Shared context: architecture notes, key decisions, gotchas, hot files |
| `meta_templates/.vibe/plan_history_tplt.md` | Archived DONE/CANCELLED checkpoint blocks moved from `plan.md` by `/vibe_cleanup` |

## Scope and cadence
- A **stage** groups a few related checkpoints.
- A **checkpoint** is a small, reviewable diff — at most a few commits.
- Limit changes to what is necessary to meet acceptance criteria.

## Metafile updates
- Update the STATUS of a checkpoint when it changes to keep it current.
- New checkpoints have a PLANNED status by default.
- When a checkpoint moves to DONE, update `.vibe/state.md` to reflect the new state.
- NEVER remove checkpoints from `.vibe/plan.md` unless explicitly asked to.
- NEVER re-number checkpoints unless explicitly asked to.

## Plan cleanup

Over time `.vibe/plan.md` accumulates `[DONE]` and `[CANCELLED]` checkpoint blocks. The skill `/vibe_cleanup` may be used to archive them to `plan_history.md`. Only perform this upon explicit request.

## Version control policy
- Commit message format (mandatory for vibe_implement): `<checkpoint-id> - <checkpoint name>`
- Commit coherent changes: one commit = one consistent set of related changes towards a checkpoint.

## Best practices & preferences
- Use local environment whenever possible (virtualenv for Python, local node_modules for JS) — avoid polluting system-wide environments.
