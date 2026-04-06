# STATE

## State management rules

- The current target system should remain stable during a draft unless a human changes the problem statement or scope.
- The status can be set to `DONE` only when the human reviewer has explicitly approved the result
- Keep this file focused on current execution state. Put rollups and resolved items in `HISTORY.md`.

## Current focus

- Revision ID: Arch.0.1
- Status: FREEZE

## Objective (current draft)

Architecture workflow framework frozen as stable. Templates and workflow contracts are locked for downstream use.

## Active assumptions / constraints

- The architecture workflow structure (AGENTS.md, AGENTS_ARCHITECTURE.md, meta-templates) is considered stable and ready for downstream work.

## Work log (current session)

- 2026-03-22: Architecture mode frozen per human reviewer approval. Workflow contracts and templates locked.

## Key Architecture Decisions

- Architecture workflow uses three modes: **architecture**, **component**, and **vibe** (with sub-modes **vibe-draft** and **vibe-implement**)
- Elements are described by **role/function**, not by implementation choice — no concrete product names at architecture level
- Element numbering uses increments of 10 (10, 20, 30…)
- DISCUSSION.md serves as the question & investigation backlog for blocking architectural choices
- HISTORY.md is append-only for traceability
- STATE.md tracks current execution state; resolved items move to HISTORY.md
- All templates originate from `/meta_templates/.architecture`
