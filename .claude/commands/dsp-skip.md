---
name: dsp-skip
description: Skip the current DSP workflow phase and move to the next one. Use when a phase is not needed or already addressed elsewhere.
---

# /dsp:skip — Skip Current Phase

You are skipping the current phase in the DSP workflow. This moves to the next phase without running the current phase's skill.

## When to Use

- Phase requirements already met elsewhere
- Time constraints require skipping
- Phase not applicable for this project
- User wants to focus on specific phases only

## Workflow

### Step 1: Check Workflow Exists

```bash
ls .design/config.json 2>/dev/null
```

### Step 2: Get Current Phase

Read `.design/STATE.md` and `.design/config.json` to determine:
- Current phase number
- Current phase name
- Next phase

### Step 3: Confirm Skip

Ask for confirmation:

```
You're about to skip: Phase [N] — [Phase Name]

This phase would normally produce: [output file]

Skipping means:
• No [output] will be generated
• Next phase won't have this context
• /dsp:verify may flag missing artifacts

Are you sure? (y/n)

If you want to provide minimal context instead, use /dsp:discuss first.
```

### Step 4: Handle Skip Options

**If confirmed:** branch on whether a subphase or a main phase is being skipped. STATE.md, config.json, and the success output must all agree on which happened — never advance the main phase in one file and leave it unchanged in another.

1. Update `.design/STATE.md`:
   - **If `workflow.current_optional_phase` is set** (skipping a subphase):
     - Mark the subphase row as `⊘ skipped` in the Optional table
     - Clear the "Optional subphase" line to `none` in Current Position
     - Do NOT change the main-phase rows or numeric Current Position
     - Add skip entry to activity log (mention it was the optional subphase)
   - **Otherwise** (skipping a main phase):
     - Mark current main phase as `⊘ skipped`
     - Set next main phase as current
     - Advance the numeric Current Position
     - Add skip to activity log
   - Note reason if provided in both branches

2. Update `.design/config.json` (must match the STATE.md branch above). **Schema invariant:** `workflow.phases_completed` stays a homogeneous array of phase-name strings — existing consumers like `/dsp:execute` use `.includes('ux')` membership checks and MUST keep working. Track skips in a separate `workflow.phases_skipped` array (create if missing).

   - **If `workflow.current_optional_phase` is set** (user is inside an optional subphase):
     - Append the subphase name (e.g., `"journey"`) to `workflow.phases_skipped` (string array; create the array if it doesn't exist)
     - Do NOT append to `workflow.phases_completed` — skipped ≠ completed
     - Clear `workflow.current_optional_phase` to `null`
     - Do NOT change `workflow.current_phase` — skipping a subphase doesn't advance the main workflow
   - **Otherwise** (skipping a main phase):
     - Append the main phase name (e.g., `"ux"`) to `workflow.phases_skipped`
     - Do NOT append to `workflow.phases_completed`
     - Increment `workflow.current_phase` (bounded by 4)
     - Leave `workflow.current_optional_phase` as-is (should already be `null`)

   Rationale: mixing `{status: "skipped"}` objects into the previously-string `phases_completed` array would break downstream membership checks. Two parallel string arrays keeps every existing consumer working and makes skip state trivially queryable.

3. Placeholder file — **never** create a canonical `{PHASE_NAME}.md` for a skip, regardless of main vs. subphase. Downstream skills and verification load canonical files unconditionally (e.g., `DISCOVERY.md`, `UX-DECISIONS.md`, `JOURNEY-MAP.md`, `ROADMAP.md`) and there is no guarantee every reader checks for a SKIPPED header before consuming. A canonical placeholder is indistinguishable from real input to those readers and would pollute every downstream phase.

   Instead, for both main-phase and subphase skips:
   - Append a row to `.design/phases/SKIPPED-LOG.md` (create the file with a header if absent). Example row:
     ```
     | 2026-04-19T10:00Z | ux | main | User chose to skip after review |
     | 2026-04-19T11:00Z | journey | subphase | Research already covered this |
     ```
   - This file is **informational only** — no skill or validator loads it at skill execution time. `/dsp:progress` and `/dsp:verify` read skip state from `workflow.phases_skipped` in `config.json`, which is the canonical source of truth.

**Output — must match whichever branch ran above.**

If a subphase was skipped, do NOT claim the main phase advanced:
```
✓ Skipped optional subphase: [prd | journey | roadmap | color_system]

Current Position:
  Phase: [N] of 4 ([Main Phase Name])   ← unchanged
  Optional subphase: none                 ← cleared
  Status: [unchanged]
  Progress: [unchanged]

Next: Continue the main workflow, or invoke another optional skill
      Or /dsp:back to re-enter the skipped subphase
```

If a main phase was skipped, advance:
```
✓ Skipped: Phase [N] — [Phase Name]

New Position:
  Phase: [N+1] of 4 ([Next Phase Name])
  Status: Ready
  Progress: [████░░░░░░] [X]%

Next: Run /[next-skill] to continue
      Or /dsp:back to return to [Phase Name]
```

### Step 5: Edge Cases

**Skipping Discovery (Phase 1):**
```
⚠️ Skipping Discovery is not recommended.

Discovery establishes:
• Problem statement
• User personas
• Requirements
• Constraints

Without discovery, subsequent phases lack critical context.

Alternatives:
1. /dsp:discuss — Provide minimal context without full interrogation
2. Paste existing brief — If you have a design brief, share it and I'll extract context

Still skip? (y/n)
```

**Skipping PRD (optional phase after Discovery):**
```
Skipping PRD generation.

This is fine if:
• You don't need formal stakeholder documentation
• Your team works directly from the design brief
• You'll create a PRD later outside the workflow

The UX phase will still have full context from Discovery.

Skip? (y/n)
```

**Skipping Review (Phase 4) — special, non-exempt gate:**
```
⚠️ Review is the end-of-process quality gate.

Skipping Review means:
• No quality score
• No accessibility audit
• No spec alignment check

The workflow will NOT reach workflow_status = "complete" with review skipped.
/dsp:verify forces workflow_status = "gaps" whenever review is in phases_skipped —
because "complete" means "reviewed and verified" for every downstream consumer.

You can still skip if you intend to run /dsp:eng_review later (or with another tool),
but do not expect this to close the workflow.

Skip anyway? (y/n)
```

If the user confirms, still append `"review"` to `workflow.phases_skipped` as usual (for audit trail). Do NOT modify the workflow_status here — `/dsp:verify` is the single source of truth for status transitions, and it will keep the workflow at `gaps` until `REVIEW.md` exists.

**Skipping when already on last phase:**
```
You're on the last phase (Review).

Options:
• Run /dsp:eng_review to complete the workflow (recommended)
• Run /dsp:verify to check deliverables
• Skip review (warning above applies — workflow stays in "gaps", not "complete")
```

Never present skipping review as a path to a complete workflow. `/dsp:verify` enforces `workflow_status = "gaps"` whenever review is missing; treating a skipped review as acceptable erodes the meaning of `complete` for every downstream consumer.

**All phases skipped:**
If user tries to skip all phases, warn that this defeats the purpose:
```
⚠️ All phases would be skipped.

If you don't need the DSP workflow, run skills standalone instead:
• /dsp:discovery — Discovery
• /dsp:ux — Usability
• /dsp:ui — Visual design
• /dsp:eng_review — Code review

These work without the workflow infrastructure.
```

## State Updates

**STATE.md updates:**
```markdown
## Phase Status

| # | Phase | Status | Completed | Output |
|---|-------|--------|-----------|--------|
| 1 | Discovery | ⊘ skipped | 2024-01-15 | — |
| 2 | UX | ◐ current | — | UX-DECISIONS.md |
```

**Activity log entry:**
```markdown
### Last Activity
- **Date:** [TIMESTAMP]
- **Action:** Skipped Discovery phase
- **Reason:** [User provided reason]
```

## Recovery

To undo a skip:
1. `/dsp:back` returns to the skipped phase
2. Running the phase skill will generate the output
3. Phase status changes from "skipped" to "completed"

---

## Workflow Navigation

| | |
|---|---|
| **This command** | `/dsp:skip` — Skip current phase |
| **Use when** | Phase output already exists or isn't needed |
| **Advances to** | The next phase in sequence |
| **Undo with** | `/dsp:back` — Return to the skipped phase |
| **Related** | `/dsp:progress` — See current phase and what was skipped |
