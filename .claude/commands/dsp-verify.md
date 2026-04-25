---
name: dsp-verify
description: Verify design deliverables are complete using goal-backward verification. Checks truths, artifacts, and wiring to ensure design quality before handoff.
---

# /dsp:verify — Design Deliverable Verification

You are running goal-backward verification on the design workflow. This checks what must be TRUE, what must EXIST, and what must CONNECT.

## Verification Philosophy

**Goal-backward verification** asks:
1. What must be TRUE for this design to succeed?
2. What artifacts must EXIST to prove we've done the work?
3. What must CONNECT between phases for coherence?

This is NOT a checklist of "did we do the task" — it's validation that the design is actually ready.

## Workflow

### Step 1: Check Workflow Exists

```bash
ls .design/config.json 2>/dev/null
```

### Step 2: Load All Phase Documents

Read whatever exists:
- `.design/PROJECT.md`
- `.design/REQUIREMENTS.md`
- `.design/phases/DISCOVERY.md`
- `.design/phases/PRD.md` (if `optional_phases.prd.enabled` in config)
- `.design/phases/JOURNEY-MAP.md` (if `optional_phases.journey.enabled` in config)
- `.design/phases/ROADMAP.md` (if `optional_phases.roadmap.enabled` in config)
- `.design/phases/UX-DECISIONS.md`
- `.design/phases/COLOR-SYSTEM.md` (if `optional_phases.color_system.enabled` in config)
- `.design/phases/UI-SPEC.md`
- `.design/phases/REVIEW.md`
- `.design/phases/PRESENTATION-*.md` (any file matching, if `optional_phases.storytell.enabled` in config)

### Step 3: Run Truth Verification

**Design Truths** — What must be TRUE:

| Truth | Check | Source |
|-------|-------|--------|
| Problem is clearly defined | Is there a problem statement in discovery? | DISCOVERY.md |
| User is well-understood | Is primary user documented with goals? | DISCOVERY.md |
| Requirements are prioritized | Are there must/should/could categories? | DISCOVERY.md, REQUIREMENTS.md |
| User flow is complete | Is there an end-to-end flow? | UX-DECISIONS.md |
| All states are specified | Are default/hover/focus/error/empty/loading documented? | UX-DECISIONS.md |
| Accessibility addressed | Are a11y requirements documented? | UX-DECISIONS.md, REQUIREMENTS.md |
| Visual hierarchy is clear | Is there visual spec with hierarchy? | UI-SPEC.md |
| Components are specified | Are key components detailed? | UI-SPEC.md |

**Output:**
```
TRUTH VERIFICATION
────────────────────────────────────────────────────────────────────────────────
✓ Problem clearly defined
  "Marketing team needs a way to schedule campaigns..."

✓ Primary user documented
  Campaign Manager with goals: [list]

✗ MISSING: All states not specified
  Found: default, hover, error
  Missing: focus, loading, empty, success

◐ PARTIAL: Accessibility addressed
  Keyboard navigation: documented
  Color contrast: not specified
```

### Step 4: Run Artifact Verification

**Design Artifacts** — What must EXIST:

**Skipped-phase exemption — optional phases only.** The skip exemption applies **only to optional phases**: `prd`, `journey`, `roadmap`, `color_system`. Main phases (`discovery`, `ux`, `ui`, `review`) are **never** exempt from the required-artifact gate. Rationale:

- **Main phases** are the core deliverables. Downstream consumers (`/dsp:execute`, handoff, audit) depend on `DISCOVERY.md` / `UX-DECISIONS.md` / `UI-SPEC.md` / `REVIEW.md` existing. A workflow with any of these missing cannot legitimately be `complete`, regardless of user intent. Skipping them is allowed via `/dsp:skip`, but it leaves the workflow permanently at `workflow_status = "gaps"` until the artifact exists.
- **Optional phases** add value when they run but aren't part of the core contract. Skipping them is a legitimate path to `complete`.

Processing rule: before applying the required-artifact table, intersect each **optional** row with `workflow.phases_skipped`. If an optional phase name appears there, record it as `⊘ skipped (intentional)` instead of `✗ missing`. Do NOT apply this exemption to any main-phase row.

| Kind | Phase names | Exemptable by `phases_skipped`? |
|---|---|---|
| Main | `discovery`, `ux`, `ui`, `review` | **Never.** Missing artifact → `workflow_status = "gaps"`. |
| Optional | `prd`, `journey`, `roadmap`, `color_system` | Yes. Skipped → `⊘` in report, does not block `complete`. |

Phase-name mapping for the skip exemption (match against `workflow.phases_skipped`):
- `DISCOVERY.md` ← `"discovery"`
- `PRD.md` ← `"prd"`
- `JOURNEY-MAP.md` ← `"journey"`
- `ROADMAP.md` ← `"roadmap"`
- `UX-DECISIONS.md` ← `"ux"`
- `COLOR-SYSTEM.md` ← `"color_system"`
- `UI-SPEC.md` ← `"ui"`
- `REVIEW.md` ← `"review"`

| Artifact | Required | Location |
|----------|----------|----------|
| DISCOVERY.md | For any phase beyond discovery (**main phase — never exempt**). Missing → `workflow_status = "gaps"`. | .design/phases/ |
| PRD.md | If `optional_phases.prd.enabled` AND `"prd"` not in `phases_skipped` | .design/phases/ |
| JOURNEY-MAP.md | If `optional_phases.journey.enabled` AND `"journey"` not in `phases_skipped` | .design/phases/ |
| ROADMAP.md | If `optional_phases.roadmap.enabled` AND `"roadmap"` not in `phases_skipped` | .design/phases/ |
| UX-DECISIONS.md | For UI or Review phases (**main phase — never exempt**). Missing → `workflow_status = "gaps"`. | .design/phases/ |
| COLOR-SYSTEM.md | If `optional_phases.color_system.enabled` AND `"color_system"` not in `phases_skipped` | .design/phases/ |
| UI-SPEC.md | For Review phase (**main phase — never exempt**). Missing → `workflow_status = "gaps"`. | .design/phases/ |
| REVIEW.md | For workflow completion (**main phase — never exempt**). Missing → `workflow_status = "gaps"`. | .design/phases/ |

**Ancillary artifacts (reported, never gating):** these are produced on demand and should NOT block workflow completion. Report their presence/absence and validate opportunistically, but do not fail verification when they're missing:

| Ancillary Artifact | Location | Notes |
|--------------------|----------|-------|
| PRESENTATION-*.md | .design/phases/ | `/dsp:storytell` is a cross-phase skill; multiple presentations may coexist, or none. Never required for completion. |

For each artifact, also verify it's substantive:
- Not just placeholder text
- Contains actual decisions/specs
- Has required sections populated

**Output:**
```
ARTIFACT VERIFICATION
────────────────────────────────────────────────────────────────────────────────
✓ DISCOVERY.md exists and substantive
  Sections: Problem Statement ✓, Users ✓, Requirements ✓, Journey Map ✓

✓ UX-DECISIONS.md exists and substantive
  Sections: User Flow ✓, States ◐, Accessibility ✓

✗ UI-SPEC.md missing
  Expected at: .design/phases/UI-SPEC.md
  Needed for: Review phase

○ REVIEW.md not yet expected
  Current phase: UI
```

### Step 5: Run Wiring Verification

**Design Wiring** — What must CONNECT:

| Connection | From | To | Check |
|------------|------|-----|-------|
| Requirements trace | DISCOVERY.md | UX-DECISIONS.md | Do UX decisions reference requirements? |
| Discovery to PRD | DISCOVERY.md | PRD.md | Does PRD align with discovery findings? (if prd enabled) |
| Discovery to Journey | DISCOVERY.md | JOURNEY-MAP.md | Does journey map use the primary persona + scenario from discovery? (if journey enabled) |
| Journey to UX | JOURNEY-MAP.md | UX-DECISIONS.md | Are top opportunities from journey map addressed in UX? (if journey enabled) |
| Discovery to Roadmap | DISCOVERY.md | ROADMAP.md | Do roadmap themes reference discovery problem/goals? (if roadmap enabled) |
| Roadmap to UX | ROADMAP.md | UX-DECISIONS.md | Do UX decisions address "Now" themes from the roadmap? (if roadmap enabled) |
| PRD to UX | PRD.md | UX-DECISIONS.md | Do UX decisions address PRD requirements? (if prd enabled) |
| Color to UI | COLOR-SYSTEM.md | UI-SPEC.md | Does UI spec reference color tokens from color system? (if color_system enabled) |
| UX to UI | UX-DECISIONS.md | UI-SPEC.md | Does UI spec cover all components from UX? |
| States coverage | UX-DECISIONS.md | UI-SPEC.md | Are all UX states visually specified? |
| Spec to Review | All phases | REVIEW.md | Does review check against specs? |
| Source to Presentation | Any phase | PRESENTATION-*.md | *Opportunistic only.* If any PRESENTATION-*.md exists, check that `source_phases` frontmatter references at least one real phase artifact. Never a blocker for overall verification. |

**Output:**
```
WIRING VERIFICATION
────────────────────────────────────────────────────────────────────────────────
✓ Requirements → UX Decisions
  8 of 8 must-have requirements addressed in UX phase

◐ UX Decisions → UI Spec
  5 of 7 components have visual specs
  Missing: ErrorToast, EmptyState

✗ State Coverage Gap
  UX defined 9 states, UI spec covers 6
  Missing visual specs: focus, disabled, empty
```

### Step 6: Generate Summary Report

```
═══════════════════════════════════════════════════════════════════════════════
DSP VERIFICATION REPORT: [Project Name]
═══════════════════════════════════════════════════════════════════════════════

Overall Status: GAPS FOUND

SUMMARY
────────────────────────────────────────────────────────────────────────────────
│ Category   │ Pass │ Partial │ Fail │
│────────────│──────│─────────│──────│
│ Truths     │  6   │    1    │   1  │
│ Artifacts  │  2   │    0    │   1  │
│ Wiring     │  1   │    1    │   1  │
│────────────│──────│─────────│──────│
│ TOTAL      │  9   │    2    │   3  │

CRITICAL GAPS (must fix)
────────────────────────────────────────────────────────────────────────────────
1. UI-SPEC.md missing
   → Run /dsp:ui to generate visual specifications

2. State coverage incomplete
   → Update UX-DECISIONS.md with: focus, disabled, empty states
   → Then update UI-SPEC.md with visual specs for these states

RECOMMENDATIONS
────────────────────────────────────────────────────────────────────────────────
1. [ ] Complete /dsp:ui phase to generate UI-SPEC.md
2. [ ] Add missing states to UX-DECISIONS.md
3. [ ] Re-run /dsp:verify after addressing gaps

═══════════════════════════════════════════════════════════════════════════════
```

### Step 7: Update State

Update `.design/STATE.md` with verification results:
- If all pass: workflow_status → `complete` (the workflow is verified-complete)
- If gaps found: workflow_status → `gaps`, list blockers

Update `.design/config.json`:
- `workflow.workflow_status` → `complete` or `gaps`

**Do not invent new enum values.** The allowed set is defined by `/dsp:start` and `/dsp:progress`: `not_started | ready | in_progress | blocked | complete | gaps`. If verification passes, the workflow is `complete`; the fact that it has been verified is captured in the verification report / STATE.md activity log, not in `workflow_status`.

## Verification Thresholds

| Result | Criteria |
|--------|----------|
| PASSED | All truths verified, all required artifacts exist, wiring complete |
| GAPS FOUND | Any truth false, artifact missing, or wiring broken |
| CANNOT VERIFY | Missing critical files to even check |

## When to Run

- After completing all phases (before handoff)
- After fixing gaps from previous verification
- When uncertain about design completeness
- Before starting implementation

## Agent Integration

This command can spawn the `dsp-verifier` agent for deeper analysis if needed.

---

## Workflow Navigation

| | |
|---|---|
| **This command** | `/dsp:verify` — Goal-backward verification |
| **Previous** | `/dsp:eng_review` — Code review (Phase 4) |
| **If gaps found** | `/dsp:back` — Return to the phase that needs fixing |
| **If passed** | Workflow complete — ready for handoff |
| **Related** | `/dsp:progress` — Quick status overview |
