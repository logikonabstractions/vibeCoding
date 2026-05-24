---
name: architecture
description: "vibeCoding architecture mode. Use to define or refine the architectural design of a system: create/update architectural elements, answer Arch questions, review, or freeze the architecture. Use when the user says 'architecture mode', references an Arch-N.N item, or asks to freeze the architecture."
argument-hint: [action] — e.g. "continue", "freeze", "review", "--list", or a free-form problem statement to start from scratch
---

# Architecture Mode

## Setup

Before doing anything else:
1. Read `agents.md` (repo root)
2. Read `.architecture/agents_architecture.md`
3. Read `.architecture/architecture_description.md`
4. Read `.architecture/state.md` — check current status and active blockers
5. Read `.architecture/discussion.md`
6. Read `.architecture/history.md` (optional — skip if session is a quick status check)

Follow the instruction precedence defined in `agents_architecture.md`.

## Interpreting $ARGUMENTS

| Argument | What to do |
|----------|-----------|
| *(empty)* | Read state, summarize current status, ask what to do next |
| `continue` | Pick up from `.architecture/state.md` — resume active work |
| `freeze` | Run the FREEZE checklist from `agents_architecture.md` |
| `review` | Review the current architecture for gaps, missing elements, or open questions |
| `--list` | Read `.architecture/architecture_description.md` and `.architecture/discussion.md`. Output a concise table: one row per architectural element with its status, plus a second table of open `Arch-N.N` discussion items (ID, summary, blocking?) |
| *(free text)* | Treat as a problem/product statement and begin architectural design from scratch |

## Output discipline

- Architectural elements are numbered in increments of 10 (10, 20, 30 …)
- Describe elements by **role**, not by implementation choice — no concrete product names
- Open questions go into `.architecture/discussion.md` as `Arch-N.N` items
- Update `.architecture/state.md` whenever focus shifts or a blocker is added/resolved
- Append to `.architecture/history.md` when a status moves to DONE, a review round completes, or an important decision is made
