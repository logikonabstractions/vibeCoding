# vibeCoding coherence refactor — reference & plan

Source of truth for decisions made during the architecture pass. Use this as the
playbook when tackling component and vibe modes.

---

## Decisions established (apply to all modes)

### D1 — `meta_templates/.<mode>/` is the single canonical format source
Working files in `.<mode>/` are project artifacts generated from templates.
Templates define format; live files hold content.
Contracts reference the template for format and the live file as the deliverable.

### D2 — `.<mode>/` is gitignored and scaffolded on first run
All working directories are excluded from the repo. `meta_templates/skills/vibe_scaffold/SKILL.md`
creates them on demand. Pattern: `/<mode>/` (root-anchored) in `.gitignore`.

### D3 — `agents_<mode>.md` lives in `meta_templates/.<mode>/`, not in `.<mode>/`
It is a static workflow contract, not a project artifact. The scaffold copies it into
`.<mode>/agents_<mode>.md` so all existing path references inside it remain valid.

### D4 — Heading levels: `#` for document title, `##` for sections
Live files (and therefore templates) use `#` as the file title and `##` for all sections.
Templates no longer carry the "Template file, copy but do not edit" artifact tag.

### D5 — Instructional prose belongs in the templates
Rules, usage notes, and resolution guidance in working files must also be present in the
corresponding template, so the template is truly the canonical format source.

### D6 — ID conventions
- Discussion/issue IDs: `<Mode>-N.N` with a **dash** (e.g. `Arch-1.2`, `Comp-1.2`).
- Revision IDs in `state.md`: `Rev-N.N` — distinct namespace, not confused with issue IDs.

### D7 — `history.md` is optional in the SKILL read order
Mark it `(optional — skip if session is a quick status check)` in both the SKILL and
the contract read order. Required for FREEZE and review sessions.

---

## Per-file checklist (apply to each mode)

For each mode, work through this list before implementing scaffold support:

### SKILL (`~/.claude/skills/vibe_<mode>/SKILL.md`)
- [ ] Step 2 path: `.<mode>/agents_<mode>.md` (not repo root)
- [ ] Read order matches contract: `architecture_description → state → discussion → history`
- [ ] `history.md` marked optional
- [ ] `discussion.md` read unconditionally (not conditional on open items)

### Contract (`meta_templates/.<mode>/agents_<mode>.md`)
- [ ] Meta-template table includes ALL template files, including `discussion_tplt.md`
- [ ] Core output section references template for format, live file as deliverable
- [ ] Read order includes `history.md` marked optional
- [ ] `Revision ID` uses `Rev-N.N` in `state.md`

### Templates (`meta_templates/.<mode>/`)
- [ ] All templates use `#` title + `##` sections
- [ ] "Template file, copy but do not edit" tag stripped from all
- [ ] Instructional prose present (matching live file)
- [ ] Issue/ID placeholders use dash convention (`<Mode>-N.N`)
- [ ] `discussion_tplt.md` exists and is consistent with live `discussion.md`

### Live files (`.<mode>/`) — verify before gitignoring
- [ ] All template/live pairs are identical (run comparison before untracking)
- [ ] Blank lines, heading levels, ID conventions consistent with templates

### Git
- [ ] `/<mode>/` added to `.gitignore` (root-anchored)
- [ ] All `.<mode>/` files untracked with `git rm --cached`
- [ ] `agents_<mode>.md` moved to `meta_templates/.<mode>/`

### `vibe_scaffold` SKILL
- [ ] New section added for the mode under `## Steps`
- [ ] Copy table lists all files (contract + 4 working files)

---

## Architecture mode — STATUS: DONE

All items above completed. Key fixes applied beyond the checklist:

- **A2:** `Arch-N.N` dash convention fixed in `state.md` active issues (was dot).
- **A4:** `architecture_description.md` reconciled — `PROBLEM STATEMENT` section added
  to template; headers unified (`Data / state`, `Security / access considerations`,
  `Observability / operational considerations`, `Primary data flows/event flows`).
- **A5:** `discussion.md` typo fixed (`RESOVLED` → `RESOLVED`); "Entry Templates" →
  "Entry templates" in `history.md`.

---

## Component mode — STATUS: DONE

All items from the checklist completed. Key fixes applied:

- **C1:** SKILL path fixed — `.component/agents_component.md` (was repo root); SKILL moved to `meta_templates/skills/vibe_component/SKILL.md`.
- **C2:** `RESOVLED` → `RESOLVED`; `Compo-` → `Comp-` (dash convention).
- **C3:** Heading separator added to `components_descriptions.md` (`[AE_ID][COMP_ID]` → `[AE_ID].[COMP_ID]`).
- **C4:** `PROBLEM STATEMENT` section added to `components_description_tplt.md`.
- **C5:** Added missing `Status` field, `Work log`, `Workflow state` sections to `state_tplt.md` and live file; aligned status vocabulary.
- **C6:** `discussion_tplt.md` added to contract meta-template table.
- **D3:** `agents_component.md` moved to `meta_templates/.component/` (contract file, not live artifact).
- **D4:** Heading levels fixed to `#` title + `##` sections across all templates.
- **D5:** Instructional prose added to templates (How to use, Question resolution, Rules, Entry templates).
- **D7:** `history.md` marked optional in read order.
- **Git:** `/.component/` added to `.gitignore`; all `.component/` files untracked with `git rm --cached`.
- **Scaffold:** Component mode added to `vibe_scaffold` SKILL.

## Vibe mode — STATUS: DONE

All items from the checklist completed. Key fixes applied:

- **V1:** SKILL path fixed — `.vibe/agents_vibe.md` (was repo root); SKILL moved to `meta_templates/skills/vibe_vibe/SKILL.md`.
- **V2:** `plan_tplt.md` heading levels fixed — `##` for stages, `###` for checkpoints with `[STATUS]` prefix; added `## How to use this file` section matching live file.
- **V3:** No alignment needed — checkpoint status vocabulary (`[PLANNED | IN PROGRESS | FREEZE | DONE | CANCELLED]`) is separate from current-focus status (`NOT_STARTED | IN_PROGRESS | IN_REVIEW | BLOCKED | DONE`).
- **V4:** `context.md` template artifact removed; heading levels aligned to `##`.
- **V5:** `VIBE.md` → `agents_vibe.md` in `state_tplt.md` example; already correct in live `state.md`.
- **V6:** Stray `9.` bullet already resolved in current `agents_vibe.md`.
- **V7:** Added `history.md` and `plan_history.md` (both marked optional) to SKILL setup read order.
- **D1–D5:** All decisions applied — `meta_templates/.vibe/agents_vibe.md` created; all 6 templates updated; template artifact tags removed; heading levels standardized to `#` title + `##` sections; instructional prose added to templates.
- **Git:** `/.vibe/` added to `.gitignore`; all `.vibe/` files untracked with `git rm --cached`.
- **Scaffold:** Vibe mode added to `vibe_scaffold` SKILL with 6-file copy table.
- **Skills in repo:** `vibe_vibe/SKILL.md` and `vibe_cleanup/SKILL.md` created at `meta_templates/skills/`.
