---
name: component
description: "vibeCoding component mode. Use to break down one architectural element into concrete, technology-specific components. Use when the user says 'component mode' or provides an architectural element number (e.g. 'element 10', 'component 10')."
argument-hint: [element-number | --list] — e.g. "10" or "20" to decompose, "--list" to see all open components
---

# Component Mode

## Setup

Before doing anything else:
1. Read `agents.md` (repo root)
2. Read `.component/agents_component.md`
3. Read `.component/components_descriptions.md`
4. Read `.component/state.md` — check current focus and blockers
5. Read `.component/discussion.md`
6. Read `.component/history.md` (optional — skip if session is a quick status check)
7. Read `.architecture/state.md` — specifically the `Key Architecture Decisions` section
8. Read `.architecture/architecture_description.md` — element $ARGUMENTS section

If `.architecture/state.md` status ≠ `FREEZE`, log a warning in `.component/state.md` but proceed unless told to stop.

Follow the instruction precedence defined in `.component/agents_component.md`.

## Interpreting $ARGUMENTS

| Argument | What to do |
|----------|-----------|
| *(empty)* | Read `.component/state.md`, summarize current status, ask which element to work on |
| `continue` | Pick up from `.component/state.md` — resume active element |
| `[number]` e.g. `10` | Decompose architectural element N into components N.1, N.2, … |
| `[number] review` e.g. `10 review` | Review the existing components for element N — check completeness and abstraction level |
| `--list` | Read `.component/components_descriptions.md`. Output a concise table: one row per component (ID, name, technology summary, status). Group by parent architectural element. Highlight any component with no status or an open `Comp-N.N` discussion item. |

## Output discipline

- Components are numbered as sub-elements: element 10 → `10.1`, `10.2`, `10.3` …
- Each component must name a **concrete technology or set of technologies**
- Each component must be large enough to require multiple checkpoints (roughly 1–few sprints)
- Output goes into `.component/components_descriptions.md` using the template at `/meta_templates/.component/components_description_tplt.md`
- Open questions go into `.component/discussion.md` as `Comp-N.N` items
- Update `.component/state.md` whenever focus shifts or a blocker is added/resolved
- Append to `.component/history.md` when a component is reviewed/approved or an important decision is made
