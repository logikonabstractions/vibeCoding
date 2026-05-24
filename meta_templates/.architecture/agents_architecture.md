# ARCHITECTURE WORKFLOW CONTRACT

## Purpose

Translate a product or problem statement into an **architectural design**.

## Instruction precedence & read order

  + As specified by `agents.md`
  + This file
  + `.architecture/architecture_description.md`
  + `.architecture/state.md`
  + `.architecture/discussion.md`
  + `.architecture/history.md` (optional — skip if session is a quick status check)

## Meta-templates

Found under `/meta_templates/.architecture`

| File | Role |
|------|------|
| `/meta_templates/.architecture/architecture_description_tplt.md` | Format for each architectural element description |
| `/meta_templates/.architecture/state_tplt.md` | Current draft, focus, active blockers, work log |
| `/meta_templates/.architecture/discussion_tplt.md` | Architecture questions requiring clarification or decision |
| `/meta_templates/.architecture/history_tplt.md` | Resolved questions, completed review rounds, durable decisions |

## Scope

This mode must define the major architectural elements of the target system, the responsibility of each, how they interact, and the main system-wide concerns. This is meant as a first, high-level conception of the solution.

## Core output

The deliverable is a markdown document that gives a **structured architectural breakdown** of the proposed solution. Format is defined by `meta_templates/.architecture/architecture_description_tplt.md`; the live working file is `.architecture/architecture_description.md`.

The output must:

  + describe the target system in plain language
  + identify the major architectural elements required
  + describe each element at the **functional type** level
  + define responsibilities for each element
  + capture the important interfaces and interactions between elements
  + capture relevant system-wide concerns, assumptions, constraints, and open questions

## Abstraction rule

Describe elements by **role**, not by implementation choice.

Do **not** use concrete product names. The exception to that would be if we consider that a specific tech choice can already be decided at conception time. For instance, if other existing components or software integration force a choice, or existing cloud providers must be used, etc.

## Element rule

Architectural elements must be large enough to matter at system-design level and small enough to have a clear responsibility.

Do not model low-level implementation artifacts as architectural elements.

## Numbering rules

Use top-level element numbering in increments of 10 (10, 20, 30...)

## Architecture planning rule

Use `.architecture/discussion.md` to track architecture questions that require discussion, investigation, clarification, or explicit decision. Do not over-use this track for minor decisions. Keep it for blocking architectural choices.

## FREEZE step

When the architecture has been reviewed, approved, and is considered stable, it enters the **FREEZE** state. This signals that the architectural design is locked and downstream work (component breakdown, implementation) can safely rely on it.

### FREEZE checklist

When the human reviewer confirms the architecture is ready to freeze:

  + **Update `.architecture/state.md`**
    + Set `Status` to `FREEZE`
    + Remove all HTML comments and placeholder text (clean up the template artifacts)
    + Populate the `## Key Architecture Decisions` section summarizing the major orientations chosen during the architecture review (providers, core patterns, technology families, key constraints, priorities.) as concise bullet points. This section is the **primary reference** for downstream agents working on components
    + Clear the `Active issues` section (move any remaining items to `history.md`)
    + Clear the `Workflow state` checkboxes

  + **Update `.architecture/history.md`**: append a FREEZE entry recording the date and revision that was frozen.

  + **Update this file or other architecture documents** if any process notes need to reflect the frozen state.

### Purpose of Key Architecture Decisions

The `Key Architecture Decisions` section in state.md serves as a compact reference so that agents working on components or implementation have immediate visibility into the choices that were made at the architecture level. This prevents contradictory decisions (e.g., choosing a vendor that conflicts with an already-decided cloud provider).

## Question lifecycle

  + Create an `Arch-N.N` item in `.architecture/discussion.md`. Follow instructions provided there.
