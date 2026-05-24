---
name: vibe_scaffold
description: "Scaffold a vibeCoding working directory from meta_templates. Run once on a fresh clone to create the working files for a mode. Currently supports: architecture, component, vibe."
argument-hint: [architecture | component | vibe]
---

# Vibe Scaffold

## Purpose

Create the working directory for a vibeCoding mode by copying files from `meta_templates/`.
Invoke this once when starting a new project, before invoking any mode skill.

## Steps

### Architecture

1. Create the `.architecture/` directory.
2. Copy each file from `meta_templates/.architecture/` to `.architecture/` as follows:

| Source (`meta_templates/.architecture/`) | Destination (`.architecture/`) |
|---|---|
| `agents_architecture.md` | `agents_architecture.md` |
| `architecture_description_tplt.md` | `architecture_description.md` |
| `state_tplt.md` | `state.md` |
| `discussion_tplt.md` | `discussion.md` |
| `history_tplt.md` | `history.md` |

3. Confirm all 5 files are present, then report: "Architecture mode scaffolded."
4. Tell the user they can now invoke `/vibe_architecture` to begin.

### Component

1. Create the `.component/` directory.
2. Copy each file from `meta_templates/.component/` to `.component/` as follows:

| Source (`meta_templates/.component/`) | Destination (`.component/`) |
|---|---|
| `agents_component.md` | `agents_component.md` |
| `components_description_tplt.md` | `components_descriptions.md` |
| `state_tplt.md` | `state.md` |
| `discussion_tplt.md` | `discussion.md` |
| `history_tplt.md` | `history.md` |

3. Confirm all 5 files are present, then report: "Component mode scaffolded."
4. Tell the user they can now invoke `/vibe_component` to begin.

### Vibe

1. Create the `.vibe/` directory.
2. Copy each file from `meta_templates/.vibe/` to `.vibe/` as follows:

| Source (`meta_templates/.vibe/`) | Destination (`.vibe/`) |
|---|---|
| `agents_vibe.md` | `agents_vibe.md` |
| `plan_tplt.md` | `plan.md` |
| `plan_history_tplt.md` | `plan_history.md` |
| `state_tplt.md` | `state.md` |
| `context_tplt.md` | `context.md` |
| `history_tplt.md` | `history.md` |

3. Confirm all 6 files are present, then report: "Vibe mode scaffolded."
4. Tell the user they can now invoke `/vibe_draft` or `/vibe_implement` to begin.

### Skills

Skills associated with this repo live under `meta_templates/skills/`. When scaffolding a project, check that:

1. the Claude skills in that system (typically `~/.claude/skills/`) contain the `vibe_architecture`, `vibe_component`, `vibe_draft`, `vibe_implement`, `vibe_cleanup` skills. If not, ask for permission to add them.
2. If the skills are present under `~/.claude/skills/`, check that they are up to date. They should match those under `meta_templates/skills/`. If different, highlight discrepancies and wait for instructions.