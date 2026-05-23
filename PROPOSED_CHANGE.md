# PROPOSED CHANGE — vibeCoding coherence pass

Goal: make the repo (`agents.md`, the `.<mode>/` working dirs, and
`meta_templates/.<mode>/`) and the related skills under
`~/.claude/skills/` (`vibe_architecture`, `vibe_component`, `vibe_vibe`,
`vibe_cleanup`) agree with one another.

This document is **observations + proposals only** — no files other than this
one have been edited.

Status: DRAFT — awaiting decisions before any edits are applied.

---

## Sources compared

| Mode | Contract | Live working dir | Templates | Skill(s) |
|---|---|---|---|---|
| architecture | `.architecture/agents_architecture.md` | `.architecture/*` | `meta_templates/.architecture/*` | `vibe_architecture` |
| component | `.component/agents_component.md` | `.component/*` | `meta_templates/.component/*` | `vibe_component` |
| vibe | `.vibe/agents_vibe.md` | `.vibe/*` | `meta_templates/.vibe/*` | `vibe_vibe`, `vibe_cleanup` |
| (root) | `agents.md` | — | — | — |

---

## Cross-cutting patterns (apply to all three modes)

These are the recurring root causes. The per-mode sections list concrete
instances.

- **P1 — Skill path bug: `agents_<mode>.md` is NOT at repo root.** All three
  mode skills tell the agent to "Read `agents_<mode>.md` (repo root)", but every
  one of those files actually lives in its `.<mode>/` dir. `agents.md` defines
  the convention as `.<mode>/agents_<mode>.md`. Instances: A1, C1, V1.
- **P2 — Skill Setup omits files the contract requires.** Each skill's Setup
  drops `history.md` (and, for vibe-implement, `plan_history.md`) that the
  contract's read order includes; read order is also reshuffled. Instances:
  A3, C5, V7.
- **P3 — Template ↔ live drift.** The live `.<mode>/*` files were hand-seeded
  and have diverged from `meta_templates/.<mode>/*`: extra `PROBLEM STATEMENT`
  prose, heading-level differences (`#` vs `##`), and leftover template
  artifacts. Root cause is a **dual source of truth** for format (the live file
  doubles as an implicit spec). Instances: A4/A5, C4, V2/V4.
- **P4 — ID / status vocabulary inconsistency.** Separators and label sets
  differ across files within a mode (dash vs dot; `Comp` vs `Compo`; checkpoint
  status set vs state status set). Instances: A2, C2, V3.
- **P5 — Meta-template tables omit `discussion_tplt.md`.** The architecture and
  component contracts list their meta-templates but skip `discussion_tplt.md`,
  even though the file exists and is used. Instances: A6, C6.
- **P6 — Orphaned / undefined mechanisms.** Some machinery exists in the
  templates but is described nowhere in the contracts or skills (so no agent
  knows when to drive it), and some status labels are used without a definition.
  Instances: VI4 (`Workflow state` dispatcher flags), VI5 (checkpoint `FREEZE`
  semantics).
- **P7 — Skill omits contract/template *behaviour* (not just files).** Beyond
  dropping files (P2), the skills fail to operationalize protocols the
  contracts/templates clearly anticipate — e.g. the Demo-commands → Evidence →
  Acceptance verification loop and the DONE/advancement gating rules.
  Instances: VI6, VI7.

**Single most consequential item:** **V2** — the canonical
`plan_tplt.md` uses heading levels (`#`/`##`) that do NOT match what the
`vibe_cleanup` skill scans for (`##`/`###` with a `[STATUS]` prefix). A plan
drafted strictly from the template would be invisible to cleanup. This is a real
functional break, not just cosmetics.

---

## Architecture mode

### A1 — SKILL points to wrong path for `agents_architecture.md` (P1)
SKILL Setup step 2: "Read `agents_architecture.md` (repo root)". File is at
`.architecture/agents_architecture.md`. **Fix:** correct the path.

### A2 — `Arch-N.N` (dash) vs `Arch.N.N` (dot) (P4)
- Dash: `discussion.md`, `history.md`, their templates, SKILL.
- Dot: `state.md` + `state_tplt.md` (both `Revision ID` `Arch.0.1` and active-issue IDs).
- **Open decision:** is `Revision ID` the same namespace as discussion `Arch-N.N`
  items, or a distinct revision counter that just looks similar?
- **Fix (pending):** standardize on dash; if `Revision ID` is distinct, rename it
  (e.g. `Rev-N.N`) to stop the conflation.

### A3 — SKILL Setup read-order/coverage drift (P2)
Contract order: `architecture_description → state → discussion → history`.
SKILL reads `state` before `architecture_description`, makes `discussion`
conditional, and omits `history`. **Fix:** align order; add `history` (or note
it optional).

### A4 — `architecture_description` template vs live drift (P3)
| Aspect | Live `.architecture/architecture_description.md` | `architecture_description_tplt.md` |
|---|---|---|
| Problem statement | Full `# PROBLEM STATEMENT` section | Absent (starts at `# Architectural elements`) |
| Data header | `#### Data & state:` | `#### Data / state` |
| Security header | `### Security` | `### Security / access considerations` |
| Observability header | `### Observability` | `### Observability / operational considerations` |
| Interaction summary | `## Main data flows:` / `## Main event flows:` | `## Primary data flows` / `## Primary event flows` |
| Lead-in | "The response must provide…adhering to this format." | "Template file, copy but do not edit" |
- **Underlying question:** contract says deliverable conforms to
  `architecture_description.md`, but `agents.md` says always start from the
  meta-template → two sources of truth. **Fix:** make the meta-template
  canonical; reconcile headers; decide whether `PROBLEM STATEMENT` belongs in
  the template (it likely does — Core output requires "describe the target
  system in plain language"); regenerate the live file from it.

### A5 — Same drift in `state/discussion/history` (P3)
Live files use `##` headings + extra rules/intro prose; templates use `#` and
"copy but do not edit". **Fix:** reconcile each pair, treat meta-template as
canonical, regenerate live files.

### A6 — Contract meta-template table omits `discussion_tplt.md` (P5)
`agents_architecture.md` lists description/state/history templates but not
`meta_templates/.architecture/discussion_tplt.md`, which exists. **Fix:** add the
row.

---

## Component mode

### C1 — SKILL points to wrong path for `agents_component.md` (P1)
SKILL Setup step 2: "Read `agents_component.md` (repo root)". File is at
`.component/agents_component.md`. **Fix:** correct the path.

### C2 — Component ID convention is inconsistent three ways (P4)
- `Comp-N.N` (dash): `agents_component.md`, `history.md`, `discussion.md`
  *issue headings*, SKILL.
- `Compo-N.N` (dash, **different prefix**): the *response template* inside both
  `discussion.md` and `discussion_tplt.md` ("record a response to any
  `Compo-x.y` item", `#### Response — Compo-[N.N]`).
- `Comp.[element].0.1` (dot, element-embedded): `state.md` + `state_tplt.md`
  (`Revision ID` and active-issue IDs).
- **Fix (pending):** pick one prefix (`Comp`) and one separator (dash);
  fix the `Compo` typo in both discussion files; reconcile the state `Revision
  ID` naming as in A2.

### C3 — Live `components_descriptions.md` numbering header drops the dot (P3/P4)
Live heading: `## [ARCHITECTURAL_ELEMENT_ID][COMPO_ID]` (no separator).
Template: `## [ARCHITECTURAL_ELEMENT_ID].[COMPO_ID]`. Numbering rule in the
contract is dotted (`10.1`, `10.2`). **Fix:** restore the dot in the live file.

### C4 — `components_descriptions` template vs live drift (P3)
Live has a `# PROBLEM STATEMENT` block (Parent element / Assumptions /
Constraints) + intro prose; template starts at `# Component breakdown`. Same
"does PROBLEM STATEMENT belong in the template?" question as A4. **Fix:** decide
and reconcile; regenerate live from canonical template.

### C5 — `state.md` is a reduced/divergent copy of its template (P2/P3)
Live `state.md` **drops** the `Status` field, the `Work log` section, and the
`Workflow state` checkboxes that `state_tplt.md` has; and its active-issue
`Impact`/`Status` option sets differ (live `QUESTION|BLOCKER` and
`NOT_STARTED|IN_PROGRESS|IN_REVIEW|DONE` vs template `QUESTION|MINOR|MAJOR|BLOCKER`
and `OPEN|IN_PROGRESS|BLOCKED|RESOLVED|DECISION_REQUIRED`). **Fix:** reconcile to
one shape; regenerate live from canonical template. (Also: SKILL Setup omits
`history.md`/`discussion.md` — P2.)

### C6 — Contract meta-template table omits `discussion_tplt.md` (P5)
Same as A6, for `agents_component.md`. **Fix:** add the row.

---

## Vibe mode

### V1 — SKILL points to wrong path for `agents_vibe.md` (P1)
SKILL Setup step 2: "Read `agents_vibe.md` (repo root)". File is at
`.vibe/agents_vibe.md`. **Fix:** correct the path.

### V2 — Plan template heading levels don't match `vibe_cleanup` (P3) — **functional break**
- `plan_tplt.md`: Stage = `# Stage …`, Checkpoint = `## …`, **no `[STATUS]`
  prefix in the heading**.
- Live `plan.md`: Stage = `## Stage [STATUS: …] …`, Checkpoint =
  `### [STATUS: …] …`.
- `vibe_cleanup` SKILL scans for: Stage `## Stage [STATUS] …`, Checkpoint
  `### [DONE|CANCELLED] …`.
- So **live plan + cleanup agree**, but the **canonical template disagrees**. The
  vibe SKILL tells drafters to "Use the template at
  `/meta_templates/.vibe/plan_tplt.md`" → a strictly templated plan would be
  invisible to `vibe_cleanup`.
- **Fix:** make `plan_tplt.md` match the live/cleanup convention (`##`/`###`
  headings **with** the `[STATUS]` prefix on both stage and checkpoint lines).

### V3 — Checkpoint status vocabulary ≠ state "Current focus" status (P4)
- Plan + cleanup checkpoint statuses: `PLANNED | IN PROGRESS | FREEZE | DONE |
  CANCELLED` (contract: new checkpoints default `PLANNED`).
- `state.md` Current-focus `Status`: `NOT_STARTED | IN_PROGRESS | IN_REVIEW |
  BLOCKED | DONE`.
- The state field tracks the active checkpoint, yet uses a different label set
  (no `PLANNED`/`FREEZE`/`CANCELLED`; adds `NOT_STARTED`/`IN_REVIEW`/`BLOCKED`).
- **Fix (pending):** decide whether checkpoint status and current-focus status
  share one vocabulary (recommended: align state to the plan's set) or are
  intentionally separate (then document why).

### V4 — `context.md` live file contains a template artifact (P3)
Live `.vibe/context.md` line 3 still says "**Template file, copy but do not
edit**", and uses `##` headings vs the template's `#`. **Fix:** strip the
artifact; reconcile heading levels; regenerate from canonical template.

### V5 — Dangling `VIBE.md` reference in `state_tplt.md`
`state_tplt.md` work-log example: "Updated `readme.md` and `VIBE.md` …". There is
no `VIBE.md` in the repo (the contract file is `agents_vibe.md`; the live
`state.md` correctly says `.vibe/agents_vibe.md`). Also these look like real
work-log entries baked into a template rather than placeholders. **Fix:** fix the
filename and replace with a neutral placeholder.

### V6 — Stray empty list item in `agents_vibe.md`
VIBE-DRAFT read order ends with an empty `9.` bullet. **Fix:** delete it.

### V7 — SKILL Setup omits `history.md` / `plan_history.md` (P2)
Contract read order includes `history.md` (both sub-modes) and `plan_history.md`
(IMPLEMENT). SKILL Setup loads only `state.md` + `plan.md`, then `context.md` per
sub-mode. **Fix:** add `history.md` (both) and `plan_history.md` (IMPLEMENT) to
the SKILL.

### V8 — (No finding) vibe has no `discussion.md`
By design — in vibe, `plan.md` is the backlog (per `agents.md`). Consistent;
noted so it isn't mistaken for a gap.

---

## Vibe mode — VIBE-IMPLEMENT sub-mode (deep dive)

VIBE-IMPLEMENT is the coding sub-mode: given a checkpoint ID it implements the
checkpoint per `plan.md`, updates `state.md`/`history.md`, and commits. These
findings are specific to that flow (the general vibe findings V1–V8 still apply).

### VI1 — SKILL IMPLEMENT additional-reads list is incomplete (P2)
Contract IMPLEMENT read order: `plan.md → state.md → history.md → context.md →
plan_history.md`. After Setup (which loads `state.md` + `plan.md`), the SKILL's
"Additional files to read" lists **only `context.md`** — missing `history.md`
and `plan_history.md`. Same root cause as V7; called out here because
`plan_history.md` matters specifically to implement (archived checkpoints can be
referenced when working a new one). **Fix:** add `history.md` and
`plan_history.md` to the IMPLEMENT additional-reads.

### VI2 — Dual status update with two vocabularies (P4)
Completing a checkpoint requires updating **two** status fields with **two**
different label sets: the checkpoint's status in `plan.md`
(`PLANNED|IN PROGRESS|FREEZE|DONE|CANCELLED`) and the `Status` in `state.md`
Current focus (`NOT_STARTED|IN_PROGRESS|IN_REVIEW|BLOCKED|DONE`). An agent must
mentally map between them on every transition. Same underlying issue as V3, but
it bites hardest during implement (the status-churn sub-mode). **Fix:** resolve
via D2 — share one vocabulary, or document the mapping explicitly in the IMPLEMENT
section.

### VI3 — Contract omits the `history.md` append on DONE (P2, reversed)
`agents.md` (root) says append to history when a status moves to DONE, and the
SKILL says "when done: update `state.md`, append to `history.md`." But
`agents_vibe.md` → "Metafile updates" only says "When a checkpoint moves to DONE,
update `.vibe/state.md`" — it never mentions the `history.md` append. So here the
*contract* is the one missing a step the skill and root both require. **Fix:** add
the `history.md` append to `agents_vibe.md` Metafile updates.

### VI4 — `state.md` Stage/Checkpoint IDs don't match the canonical 4-part ID (P3)
Canonical checkpoint ID is 4-part (`10.2.1.1` = component `10.2` · stage `1` ·
checkpoint `1`), and the SKILL dispatches IMPLEMENT on exactly that `N.N.N.N`
shape. But `state.md` Current focus represents the active position as
`Stage: 0` (1 part) and `Checkpoint: 0.0` (2 parts) — which cannot encode a real
checkpoint (the component is dropped). When implementing `10.2.1.1`, it's
ambiguous what goes in those fields. **Fix:** make the state template use the
canonical IDs — `Stage:` = 3-part (`10.2.1`), `Checkpoint:` = 4-part
(`10.2.1.1`).

### VI5 — Checkpoint `FREEZE` status is undefined (P6)
`plan.md` and `vibe_cleanup` both recognize `FREEZE` as a checkpoint/stage
status (cleanup will refuse to archive it), but **nothing** in `agents_vibe.md`
or the SKILL defines what a frozen checkpoint means or when implement should set
it. (Architecture defines FREEZE for the whole architecture; vibe reuses the
label for checkpoints without a definition.) **Fix:** define checkpoint/stage
`FREEZE` semantics in `agents_vibe.md`, or drop the label if unused.

### VI6 — IMPLEMENT skill never operationalizes the Demo→Evidence→Acceptance loop (P7)
The plan checkpoint template defines `Acceptance` (checkboxes), `Demo commands`,
and `Evidence`, and `state.md` has matching `Acceptance`/`Evidence` sections —
clearly an evidence-based acceptance protocol. But the SKILL's IMPLEMENT
discipline only says "limit changes to acceptance criteria" + "commit" + "update
state/history". It never tells the agent to **run the demo commands, tick the
acceptance boxes, and paste evidence into `state.md`**. **Fix:** add the
verification loop to the IMPLEMENT discipline (run demo commands → record
evidence → check acceptance → only then DONE).

### VI7 — IMPLEMENT skill doesn't surface the DONE / advancement gating rules (P7)
`state.md` rules say: Current focus may advance **only** once the previous
checkpoint is `DONE`, and a checkpoint is `DONE` **only** when all acceptance
points are met **or** a human explicitly approves. The SKILL's IMPLEMENT section
states neither gate, so an agent following only the skill could mark DONE or move
on prematurely. **Fix:** reference both gating rules in the IMPLEMENT discipline.

### VI8 — `Workflow state` dispatcher flags are orphaned (P6)
`state.md` carries flags `RUN_CONTEXT_CAPTURE / STAGE_DESIGNED /
MAINTENANCE_CYCLE_DONE / RETROSPECTIVE_DONE`, annotated "Cleared by the loop that
handles each flag." No such loop is described in `agents_vibe.md` or either
skill, and several flags (context capture, retrospective) would naturally fire
during implement. **Fix:** either document the dispatcher loop that drives these
flags, or remove them from the template.

### VI9 — (Minor) commit-message `<checkpoint name>` source not pinned
Commit format `<checkpoint-id> - <checkpoint name>` is consistent between
contract and skill, but neither states that `<checkpoint name>` is the text after
the `—` in the plan checkpoint heading. Low risk; worth a one-line clarification.

### VI10 — (No finding) commit policy, change-scoping, local-env
Commit-coherence wording matches between `agents_vibe.md` and the SKILL; the
"limit changes to acceptance criteria" rule matches; "use local environment"
appears only in the contract (a best-practice, fine to leave skill-side
unstated). Noted so these aren't re-investigated.

---

## The deeper issue (P3 restated)

Across all modes the live `.<mode>/*` files act as **both** the working document
**and** an implicit format spec, which is why they keep drifting from
`meta_templates/.<mode>/*`. Cleanest resolution:

- `meta_templates/.<mode>/*` = single canonical format source.
- `.<mode>/*` = working instances, always (re)generated from templates.
- Update each `agents_<mode>.md` so it points to the meta-template for *format*
  and to `.<mode>/*` only as the live deliverable.

Note: the live working files currently hold placeholder content only (all modes
are at `NOT_STARTED`), so regenerating them loses nothing real.

---

## Proposed fix order (once approved)

1. **P1 path fixes** — A1, C1, V1 (isolated, unambiguous; do first).
2. **V2** — fix `plan_tplt.md` headings (functional break; high value, low risk).
3. **P5** — add `discussion_tplt.md` rows (A6, C6).
4. **V5, V6, VI3** — dangling `VIBE.md` ref, stray `9.` bullet, missing
   `history.md`-append in the vibe contract (all trivial text fixes).
5. **P7 skill behaviour** — VI6, VI7 (add the verification loop + gating rules to
   IMPLEMENT discipline; low risk, high value).
6. **P4 conventions** — A2, C2, V3, VI2 (need decisions first; then apply).
7. **P2 skill Setup** — A3, C5, V7, VI1 (align read order/coverage to contracts).
8. **P6 orphaned mechanisms** — VI5, VI8 (define or remove FREEZE semantics +
   workflow-state flags; needs a decision).
9. **P3 template/live reconciliation** — A4/A5, C3/C4/C5, V4, VI4 (largest; do
   last, regenerate live files from reconciled templates).

## Open decisions needed

- **D1 (P4 / A2, C2):** dash vs dot for issue IDs; and is each mode's
  `Revision ID` the same namespace as its discussion items or a distinct
  counter? (Also: fix `Comp` vs `Compo` typo — assumed yes.)
- **D2 (V3 / VI2):** one shared status vocabulary for checkpoint + current-focus,
  or intentionally separate? (Drives the dual-status-update friction in
  IMPLEMENT.)
- **D3 (P3 / A4, C4):** does `PROBLEM STATEMENT` belong in the
  description templates (architecture & component)?
- **D4 (P3):** confirm `meta_templates/.<mode>/*` as the single canonical format
  source, and OK to regenerate the live `.<mode>/*` files from reconciled
  templates (placeholder content only — nothing real lost)?
- **D5 (P2):** confirm skill Setup sections should mirror the full contract read
  order (incl. `history.md` / `plan_history.md`).
- **D6 (P6 / VI5, VI8):** keep or drop checkpoint/stage `FREEZE` and the
  `Workflow state` dispatcher flags? If kept, where is the driving loop /
  semantics documented?
- **D7 (P7 / VI6, VI7):** should the IMPLEMENT skill spell out the
  Demo→Evidence→Acceptance verification loop and the DONE/advancement gates
  (recommended), or keep them only in `state.md`/`plan.md` rules?
