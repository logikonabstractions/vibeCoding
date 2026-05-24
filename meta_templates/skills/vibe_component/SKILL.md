---
name: component
description: "vibeCoding component mode. Use to break down one architectural element into concrete, technology-specific components. Use when the user says 'component mode' or provides an architectural element number (e.g. 'element 10', 'component 10')."
argument-hint: [element-number | --list] — e.g. "10" or "20" to decompose, "--list" to see all open components
---

# Component Mode

## Setup

Before doing anything else:
1. Read `agents.md` (repo root)
2. Read `.component/agents_component.md` — full workflow contract; follow its instruction precedence

## Interpreting $ARGUMENTS

| Argument | What to do |
|----------|-----------|
| *(empty)* | Read `.component/state.md`, summarize current status, ask which element to work on |
| `continue` | Pick up from `.component/state.md` — resume active element |
| `[number]` e.g. `10` | Decompose architectural element N into components N.1, N.2, … |
| `[number] review` e.g. `10 review` | Review the existing components for element N — check completeness and abstraction level |
| `--list` | Read `.component/components_descriptions.md`. Output a concise table: one row per component (ID, name, technology summary, status). Group by parent architectural element. Highlight any component with no status or an open `Comp-N.N` discussion item. |

