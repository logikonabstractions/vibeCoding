---
name: vibe_improvement
description: "Push local edits made to the vibeCoding submodule back to its remote, then bump the submodule pointer in the parent project. Use after editing meta_templates/, agents.md, or any skill inside the vibeCoding submodule."
argument-hint: (none)
---

# Vibe Improvement

## Purpose

Commit and push changes made inside the `vibeCoding/` submodule to its remote, then update the parent project's submodule pointer.

## Steps

### 1. Context check

Verify you are in submodule mode: `vibeCoding/meta_templates/` must exist at CWD. If not, report: "vibe_improvement is for submodule use only. In a standalone vibeCoding repo, use git directly." and stop.

### 2. Show pending changes

Run:
```
git -C vibeCoding status
git -C vibeCoding diff --stat
```

Summarize what files were changed. If there are no changes, report: "No changes in vibeCoding submodule." and stop.

### 3. Commit inside the submodule

Stage all modified files in the submodule and commit:
```
git -C vibeCoding add -A
git -C vibeCoding commit -m "<message>"
```

Generate the commit message from the diff — one concise sentence describing what was improved. Do not ask the user for a message unless the diff is ambiguous.

### 4. Push to remote

```
git -C vibeCoding push origin HEAD
```

Report the push result. If push fails (e.g. diverged), stop and explain — do not force-push.

### 5. Bump submodule pointer in parent project

```
git add vibeCoding
git commit -m "bump vibeCoding: <same summary as step 3>"
```

### 6. Skills sync check

If any files under `vibeCoding/meta_templates/skills/` were part of the changes, compare them against `~/.claude/skills/`. List any that differ and ask the user if they should be synced now.
