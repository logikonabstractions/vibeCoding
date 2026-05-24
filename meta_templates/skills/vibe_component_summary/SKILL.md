---
name: vibe_component_summary
description: "Reads .component/components_descriptions.md and writes (or overwrites) .component/component_summary.md — a compact, daily-use reference listing each component's parent element, purpose, technology choice, and dependencies."
argument-hint: (none)
---

# Vibe Component Summary

## Steps

1. Read `.component/components_descriptions.md`.
2. For each component, extract:
   - Parent architectural element: ID + name (from the `## Parent architectural element` section at the top)
   - Component: ID + name (from each `## [ID] — [name]` heading)
   - Purpose (from `**Purpose:**`)
   - Technology choice (from `**Technology choice:**`)
   - Internal dependencies (from `#### Internal` under `### Dependencies`)
   - External dependencies (from `#### External` under `### Dependencies`)
3. Write `.component/component_summary.md` using the output format below.

## Output format

```markdown
# Component Summary

## [parent-id] — [Parent element name]

**[compo-id] — [Component name]**
Purpose: [value]
Technology: [value]
Internal deps: [10.1, 10.3 — or "none"]
External deps: [element 20, element 30 — or "none"]

**[compo-id] — [Component name]**
...

## [parent-id] — [Parent element name]
...
```

Group components by their parent architectural element. Within each group, list components in ID order. Keep each entry to 5 lines maximum. If a dependency list is empty, write `none`.

## Constraints

- Overwrite `.component/component_summary.md` completely on each run (it is a generated file).
- Do not modify `components_descriptions.md` or any other file.
- Do not include interfaces, responsibilities, observability, constraints, or principal alternatives — those belong in the full `components_descriptions.md`.
