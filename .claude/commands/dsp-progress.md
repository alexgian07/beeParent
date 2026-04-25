---
name: dsp-progress
description: Display current DSP workflow status, phase progress, and accumulated context. Shows where you are in the design process and what comes next.
---

# /dsp:progress — View Workflow Status

You are displaying the current DSP workflow status. This command reads state and provides a clear picture of progress.

## Workflow

### Step 1: Check for Workflow

Check if DSP workflow exists:

```bash
ls .design/config.json 2>/dev/null
```

**If not found:**
```
No DSP workflow found in this directory.

Start one with: /dsp:start
Or run skills standalone: /dsp:discovery, /dsp:ux, /dsp:ui, /dsp:eng_review
```

### Step 2: Load and Validate State Files

Read these files:
- `.design/config.json` — Settings and workflow metadata
- `.design/STATE.md` — Current position and context
- `.design/PROJECT.md` — Project overview

**Validate config.json** after reading:

Required fields — if any are missing, warn the user and offer to repair:
- `version` — must be a string (e.g., `"2.1"`)
- `workflow.current_phase` — must be a number 0-4
- `workflow.current_optional_phase` — optional; `null` or one of: `"prd"`, `"journey"`, `"roadmap"`, `"color_system"` (set by `/dsp:back` when the user returns to an optional subphase)
- `workflow.phases_completed` — must be an array of phase-name strings (no mixed objects). Downstream consumers use `.includes('<name>')`.
- `workflow.phases_skipped` — parallel string array (optional; treat absence as `[]`). `/dsp:skip` appends phase names here instead of marking them as completed.
- `workflow.workflow_status` — must be one of: `not_started`, `ready`, `in_progress`, `blocked`, `complete`, `gaps`
- `phases` — must be an object with `discovery`, `ux`, `ui`, `review` keys

If JSON parsing fails entirely:
```
⚠ .design/config.json is corrupted or not valid JSON.

Options:
1. Rebuild from phase files (I'll scan .design/phases/ for completed work)
2. Reset to defaults (keeps your phase output files intact)
```

If fields are missing but JSON is valid:
```
⚠ config.json is missing required fields: {list}
  Adding defaults for missing fields...
```
Then fill in the missing fields with defaults from the config template and write the repaired file.

**Cross-validate state consistency:**
- If `phases_completed` includes a phase but the output file is missing, flag it:
  `⚠ Phase "ux" marked complete but UX-DECISIONS.md not found`
- If a phase output file exists but the phase isn't in `phases_completed` **and is not in `phases_skipped`**, flag it:
  `⚠ DISCOVERY.md exists but discovery is neither completed nor skipped — marking it completed`
- **Never reclassify a skipped phase as completed just because a file exists.** A phase name in `workflow.phases_skipped` stays skipped regardless of on-disk artifacts (earlier iteration output may still live at `{BASE}.vN.md`, and skipped phases don't create canonical files anyway).
- If an optional artifact file exists (`JOURNEY-MAP.md`, `ROADMAP.md`, `PRESENTATION-*.md`) but the corresponding `optional_phases.*.enabled` is `false`, flag it:
  `⚠ JOURNEY-MAP.md exists but optional_phases.journey.enabled is false — setting enabled: true`

### Step 3: Calculate Progress

Compute completion percentage as `completed_main / 4`, where:
- `completed_main` = count of `["discovery", "ux", "ui", "review"]` present in `workflow.phases_completed`

Base the calculation on `workflow.phases_completed`, not on `workflow.current_phase` (which is a pointer and runs ahead of completions). **Skipped main phases do NOT count toward progress** — they are core deliverables and every downstream consumer (`/dsp:execute`, handoff, audit) assumes they exist.

| completed_main | Percentage |
|---|---|
| 0 | 0% |
| 1 | 25% |
| 2 | 50% |
| 3 | 75% |
| 4 | 100% |

**Main phases are non-exemptable for completion.** Skipping Discovery / UX / UI / Review via `/dsp:skip` leaves the workflow permanently at `workflow_status = "gaps"` until the artifact is produced. This matches `/dsp:verify`, which treats all four main phases as never-exempt. The only way to reach 100% is for all four names to appear in `phases_completed`.

**Optional phases** (PRD / Journey / Roadmap / Color System) never contribute to the percentage; they are surfaced as a subphase label, not as progress. Their skipped status is tracked in `phases_skipped` and surfaced with a `⊘` icon, but it neither advances nor blocks progress.

Surface skipped main phases with a clear warning — a skipped main phase is an outstanding debt, not a resolved step. Example: `⊘ review skipped — workflow stays in gaps until /dsp:eng_review runs`, `⊘ ux skipped — downstream implementation will lack UX-DECISIONS.md`.

Optional subphases (PRD / Journey / Roadmap / Color System) never contribute to the percentage; they are surfaced as a subphase label, not as progress.

**If `workflow.current_optional_phase` is non-null, display the optional subphase as the active step** without altering the percentage. Map:
- `"prd"` → "Phase 1.5a (PRD)"
- `"journey"` → "Phase 1.5b (Journey)"
- `"roadmap"` → "Phase 1.5c (Roadmap)"
- `"color_system"` → "Phase 2.5 (Color System)"

Use the subphase label for the Current Position line and the NEXT ACTIONS line (point at the relevant skill, e.g., `/dsp:journey` if `current_optional_phase` is `"journey"`).

Generate progress bar:
- 0%: `[░░░░░░░░░░]`
- 25%: `[██░░░░░░░░]`
- 50%: `[█████░░░░░]`
- 75%: `[███████░░░]`
- 100%: `[██████████]`

### Step 4: Display Status

Output this format:

```
═══════════════════════════════════════════════════════════════════════════════
DSP WORKFLOW STATUS: [Project Name]
═══════════════════════════════════════════════════════════════════════════════

Progress: [██░░░░░░░░] 25%

PHASE STATUS
────────────────────────────────────────────────────────────────────────────────
│ # │ Phase     │ Status    │ Completed  │ Output            │
│───│───────────│───────────│────────────│───────────────────│
│ 1 │ Discovery │ ● complete│ 2024-01-15 │ DISCOVERY.md      │
│ 2 │ UX        │ ◐ current │ —          │ UX-DECISIONS.md   │
│ 3 │ UI        │ ○ pending │ —          │ UI-SPEC.md        │
│ 4 │ Review    │ ○ pending │ —          │ REVIEW.md         │

Optional: PRD — not enabled
Optional: Research — not enabled
Optional: Color System — not enabled
Optional: Journey — not enabled
Optional: Roadmap — not enabled
Optional: Storytelling — [N presentations | not enabled]

CURRENT POSITION
────────────────────────────────────────────────────────────────────────────────
Phase: 2 of 4 (UX)
Status: In Progress
Last Activity: [timestamp] — Completed discovery phase

CONTEXT SUMMARY
────────────────────────────────────────────────────────────────────────────────
Problem: [One-liner from discovery]
User: [Primary user role]
Key Requirement: [Most important must-have]

RECENT DECISIONS
────────────────────────────────────────────────────────────────────────────────
• Discovery: [Key decision 1]
• Discovery: [Key decision 2]

NEXT ACTIONS
────────────────────────────────────────────────────────────────────────────────
→ Run /dsp:ux to continue with usability principles
  Or /dsp:discuss to capture decisions first

═══════════════════════════════════════════════════════════════════════════════
QUICK COMMANDS
═══════════════════════════════════════════════════════════════════════════════
/dsp:discovery  — Discovery (Phase 1)      /dsp:discuss  — Capture decisions
/dsp:prd        — PRD (optional)           /dsp:skip     — Skip current phase
/dsp:journey    — Journey map (optional)   /dsp:back     — Go to previous phase
/dsp:roadmap    — UX roadmap (optional)    /dsp:verify   — Check deliverables
/dsp:ux         — UX principles (Phase 2)  /dsp:research  — Research (any phase)
/dsp:ui         — Visual design (Phase 3)  /dsp:storytell — Presentation (any phase)
/dsp:color      — Color system (optional)
/dsp:eng_review — Review (Phase 4)
═══════════════════════════════════════════════════════════════════════════════
```

### Step 5: Show Phase Details (Optional)

If user asks for more detail on a specific phase, read the corresponding file:
- Discovery: `.design/phases/DISCOVERY.md`
- PRD: `.design/phases/PRD.md`
- Journey: `.design/phases/JOURNEY-MAP.md`
- Roadmap: `.design/phases/ROADMAP.md`
- UX: `.design/phases/UX-DECISIONS.md`
- Color System: `.design/phases/COLOR-SYSTEM.md`
- UI: `.design/phases/UI-SPEC.md`
- Review: `.design/phases/REVIEW.md`
- Storytelling: `.design/phases/PRESENTATION-*.md` (may be multiple)

Display a summary of that phase's output.

## Status Icons

- `●` complete — Phase finished
- `◐` current — Currently active phase
- `○` pending — Not yet started
- `⊘` skipped — Explicitly skipped
- `✕` blocked — Cannot proceed

## Workflow Status Values

From `config.json`:
- `not_started` — Project initialized but no phase started
- `in_progress` — Actively working through phases
- `blocked` — Cannot proceed (show blockers from STATE.md)
- `complete` — All phases done
- `gaps` — Review found issues needing iteration

## Error Handling

- If STATE.md is corrupted, offer to rebuild from phase files
- If phase files are missing but marked complete, flag inconsistency
- Always show what commands are available regardless of state

---

## Workflow Navigation

| | |
|---|---|
| **This command** | `/dsp:progress` — View workflow status |
| **Useful anytime** | Works at any point in the workflow |
| **Related** | `/dsp:start` — Initialize if no workflow found |
| | `/dsp:verify` — Detailed quality check at end of workflow |
