---
name: vibe_implement
description: "vibeCoding VIBE-IMPLEMENT mode. Given a checkpoint ID (e.g. '10.2.1.1'), implement it according to .vibe/plan.md."
argument-hint: checkpoint-id | --list — e.g. "10.2.1.1" to implement, "--list" to see open checkpoints
---

# Vibe Implement

## Setup

1. Read `agents.md` (repo root)
2. Read `.vibe/agents_vibe.md` — full workflow contract; follow its VIBE-IMPLEMENT instruction precedence

## Argument handling

| $ARGUMENTS | Action |
|---|---|
| Checkpoint ID — format `N.N.N.N` e.g. `10.2.1.1` | Implement that checkpoint |
| `--list` | Read `.vibe/plan.md`. Output a concise table of all non-DONE checkpoints: columns = ID, name, stage, status. Group by stage. Highlight active checkpoint from `.vibe/state.md`. |
| *(empty)* | Read `.vibe/state.md`, summarize current status, ask which checkpoint to implement |

