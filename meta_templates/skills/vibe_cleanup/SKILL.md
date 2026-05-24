---
name: vibe_cleanup
description: "vibeCoding cleanup mode. Archives DONE and CANCELLED checkpoints from .vibe/plan.md into .vibe/plan_history.md, keeping only stub title lines in place. Use when the user says 'cleanup', 'archive done checkpoints', 'clean plan', or similar."
argument-hint: [--dry-run] — pass --dry-run to preview what would be archived without modifying files
---

# Vibe Cleanup Mode

## Purpose

`plan.md` accumulates completed and cancelled checkpoints over time. This skill trims them down: full checkpoint blocks for `[DONE]` and `[CANCELLED]` checkpoints are moved to `plan_history.md`, and a one-line stub is left in `plan.md` so the numbering and historical record remain traceable.

---

## Setup

Before doing anything else:
1. Read `.vibe/plan.md` — this is the only source of truth for what to archive

---

## Interpreting $ARGUMENTS

| Argument | What to do |
|----------|-----------|
| *(empty)* | Run the full cleanup procedure (see below) |
| `--dry-run` | Report what would be archived and what stubs would remain, but make NO file changes |

---

## Cleanup Procedure

### Step 1 — Identify targets

Scan `.vibe/plan.md` for checkpoint headings with status `[DONE]` or `[CANCELLED]`.

A checkpoint heading has this shape:
```
### [DONE] <component>.<stage>.<checkpoint> — <title>
```
or
```
### [CANCELLED] <component>.<stage>.<checkpoint> — <title>
```

A **checkpoint block** is the heading line plus all indented content that follows it, up to (but not including) the next `###` or `##` heading, or end of file.

### Step 2 — Build the archive entries

For each target checkpoint, construct an archive entry:

```markdown
### [DONE | CANCELLED] <component>.<stage>.<checkpoint> — <title>

  **Objective**
  ...
  (full original content)
```

Group entries under their parent stage heading if it is not already present in `plan_history.md`. Use the same `## Stage [STATUS] <component>.<stage> — <name>` heading format. If the stage heading is not in `plan.md` (e.g. the stage itself was already cleaned), write the stage heading from the checkpoint ID.

### Step 3 — Append to plan_history.md

Append all archive entries to `.vibe/plan_history.md`.

- Never duplicate an entry that is already present in `plan_history.md` (match on checkpoint ID).

### Step 4 — Prune plan.md

For each archived checkpoint, replace the full block in `.vibe/plan.md` with a single stub line (should be unchanged from previously):

```markdown
### [DONE] <component>.<stage>.<checkpoint> — <title>
```

Preserve all blank lines and surrounding structure so that active checkpoints and stage headings are undisturbed.

## Output

After completing (or in `--dry-run` mode), report:

- How many checkpoints were archived (or would be archived)
- Which checkpoint IDs were affected
- Whether `plan_history.md` was created or updated
- Any skipped duplicates

Keep the report concise — a short bullet list is enough.

---

## Constraints

- NEVER delete or modify checkpoints with status `[PLANNED]`, `[IN PROGRESS]`, or `[FREEZE]`
- NEVER renumber checkpoints
- NEVER change the status label of any checkpoint (preserve `[DONE]` vs `[CANCELLED]` exactly)
- NEVER modify  any other metafile
