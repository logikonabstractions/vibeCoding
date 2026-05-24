---
name: vibe_update
description: "Pull the latest vibeCoding improvements from its remote into the local submodule, then bump the parent project's submodule pointer. Use to bring in new skills, template changes, or workflow improvements published to the vibeCoding remote."
argument-hint: (none)
---

# Vibe Update

## Purpose

Update the `vibeCoding/` submodule to the latest commit on its remote, report what changed, sync any updated skills to `~/.claude/skills/`, and commit the new submodule pointer in the parent project.

## Steps

### 1. Context check

Verify you are in submodule mode: `vibeCoding/meta_templates/` must exist at CWD. If not, report: "vibe_update is for submodule use only. In a standalone vibeCoding repo, use git pull directly." and stop.

### 2. Record current state

```
git -C vibeCoding log --oneline -1
```

Note the current commit hash for the diff report in step 4.

### 3. Pull latest from remote

```
git submodule update --remote vibeCoding
```

If this fails (e.g. local uncommitted changes in the submodule), report the error and stop — do not discard local changes.

### 4. Report what changed

```
git -C vibeCoding log --oneline ORIG_HEAD..HEAD
```

List the new commits. If nothing changed, report: "vibeCoding is already up to date." and stop.

### 5. Skills sync check

Compare every skill directory under `vibeCoding/meta_templates/skills/` against its counterpart in `~/.claude/skills/`. List any that differ (new or updated). Ask the user if they should be synced now. If yes, copy them:

```
cp vibeCoding/meta_templates/skills/<skill>/SKILL.md ~/.claude/skills/<skill>/SKILL.md
```

Create the `~/.claude/skills/<skill>/` directory first if it does not exist.

### 6. Bump submodule pointer in parent project

```
git add vibeCoding
git commit -m "bump vibeCoding to <new short hash>"
```
