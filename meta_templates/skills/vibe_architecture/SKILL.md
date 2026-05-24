---
name: architecture
description: "vibeCoding architecture mode. Use to define or refine the architectural design of a system: create/update architectural elements, answer Arch questions, review, or freeze the architecture. Use when the user says 'architecture mode', references an Arch-N.N item, or asks to freeze the architecture."
argument-hint: [action] — e.g. "continue", "freeze", "review", "--list", or a free-form problem statement to start from scratch
---

# Architecture Mode

## Setup

Before doing anything else:
1. Read `agents.md` (repo root)
2. Read `.architecture/agents_architecture.md` — full workflow contract; follow its instruction precedence

## Interpreting $ARGUMENTS

| Argument | What to do |
|----------|-----------|
| *(empty)* | Read state, summarize current status, ask what to do next |
| `continue` | Pick up from `.architecture/state.md` — resume active work |
| `freeze` | Run the FREEZE checklist from `agents_architecture.md` |
| `review` | Review the current architecture for gaps, missing elements, or open questions |
| `--list` | Read `.architecture/architecture_description.md` and `.architecture/discussion.md`. Output a concise table: one row per architectural element with its status, plus a second table of open `Arch-N.N` discussion items (ID, summary, blocking?) |
| *(free text)* | Treat as a problem/product statement and begin architectural design from scratch |

