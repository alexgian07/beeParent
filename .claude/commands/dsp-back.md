---
name: dsp-back
description: Go back to a previous DSP workflow phase. Use to revisit decisions, update context, or re-run a phase with new information.
---

# /dsp:back — Return to Previous Phase

You are navigating back in the DSP workflow. This allows revisiting earlier phases to update or refine design work.

## When to Use

- New information invalidates earlier decisions
- Need to update discovery based on research findings
- Want to refine UX decisions after UI work revealed issues
- Stakeholder feedback requires earlier phase changes
- Review found issues that need design-level fixes

## Workflow

### Step 1: Check Workflow Exists

```bash
ls .design/config.json 2>/dev/null
```

### Step 2: Get Current Position

Read `.design/STATE.md` and `.design/config.json` to determine:
- Current phase number
- Completed phases
- Available phases to return to

### Step 3: Present Options

**Gating rule:** `/dsp:back` is available whenever the current position has *something earlier to go back to*. Compute the current position on the ordered axis `1 < 1.5a < 1.5b < 1.5c < 2 < 2.5 < 3 < 4`, combining `workflow.current_phase` with `workflow.current_optional_phase`. If the current position is strictly greater than `1`, offer the menu below. This includes subphases: a user at `1.5a` can go back to `1` (Discovery); a user at `2.5` can go back to `2`, `1.5c`, `1.5b`, `1.5a`, or `1`.

**If current position > `1`:**
```
CURRENT POSITION: Phase [N] — [Phase Name]  (Optional subphase: [none | prd | journey | roadmap | color_system])

Go back to:
  [filtered list — see rules below]

Enter a label — only labels shown above are valid.

Which phase? (label or cancel)
```

**Filtering rules — only show labels the user can legitimately return to:**

1. Compute the **current position key** using both `workflow.current_phase` and `workflow.current_optional_phase`. Express current position on this ordered axis:

   `1  <  1.5a  <  1.5b  <  1.5c  <  2  <  2.5  <  3  <  4`

2. Show only labels **strictly earlier** than the current position.
3. Also hide labels corresponding to disabled optional phases — an optional label is only shown when:
   - It is strictly earlier than the current position, AND
   - Its `optional_phases.<name>.enabled` flag is `true` in `config.json`.
4. Always show main-phase labels (1, 2, 3) when they are earlier than the current position, regardless of optional-phase flags.

Example — if the user is currently at `2.5` (color_system) with `prd` and `journey` enabled but `roadmap` disabled, the valid labels are: `1`, `1.5a`, `1.5b`, `2`. Not `1.5c` (disabled), not `2.5` (current), not `3`.

**Input parsing rule:** accept only labels shown in the filtered list. Reject any other input with:
- For a disabled optional: *"Phase X is not enabled for this workflow. Enable it in config.json first, or pick another."*
- For a later-or-current label: *"Phase X is not earlier than your current position ([current-label]). /dsp:back only moves backward — use the appropriate skill to move forward."*

Note: `/dsp:research` and `/dsp:storytell` are cross-phase skills — you don't go back to them, you just invoke them again from the current phase. They produce their own artifacts without displacing the main workflow.

**If current position is `1` (Discovery with no active optional subphase):**
```
You're on Phase 1 (Discovery) — can't go back further.

Options:
• Run /dsp:discovery to start/redo discovery
• Run /dsp:progress to see status
```

### Step 4: Handle Navigation

**When user selects a phase:**

1. Confirm the action:
```
Going back to: Phase [X] — [Phase Name]

This will:
✓ Set Phase [X] as current
✓ Keep existing [Phase X] output for reference
○ Later phases remain but may need updates

Continue? (y/n)
```

2. Update `.design/STATE.md`:
   - **Keep the `Phase: [0-4]` line numeric** — write the main-phase number from the mapping below (never `1.5b`). The rest of the workflow reads this line as a number.
   - Update the `Optional subphase:` line to the subphase name (or `none`) per the mapping below.
   - Add navigation to activity log
   - Note any context about why going back

3. Update `.design/config.json`:
   - **`workflow.current_phase` must stay numeric** (0-4). Validators in `/dsp:progress` and `/dsp:start` reject non-numeric values. Map the selected label as follows:
     | Selected label | `workflow.current_phase` | `workflow.current_optional_phase` |
     |----------------|--------------------------|-----------------------------------|
     | `1`            | `1`                      | `null`                            |
     | `1.5a`         | `1`                      | `"prd"`                           |
     | `1.5b`         | `1`                      | `"journey"`                       |
     | `1.5c`         | `1`                      | `"roadmap"`                       |
     | `2`            | `2`                      | `null`                            |
     | `2.5`          | `2`                      | `"color_system"`                  |
     | `3`            | `3`                      | `null`                            |
   - If `workflow.current_optional_phase` key doesn't exist on the config yet, add it
   - **Prune later phases from `phases_completed` and `phases_skipped`.** Downstream consumers (e.g., `/dsp:execute` uses `phases_completed.includes('ux')`) must not keep treating rolled-back phases as still completed. Concretely:
     - For each phase name in `phases_completed` / `phases_skipped`, compute its position on the ordered axis `1 < 1.5a < 1.5b < 1.5c < 2 < 2.5 < 3 < 4`.
     - Keep only names whose position is **strictly earlier** than the target label.
     - Remove the rest.
   - **Archive the canonical artifact file for every phase being pruned.** If the file stays on disk with its canonical name, `/dsp:progress` will reclassify it as completed on the next run and silently undo the rollback. For each pruned phase, rename its canonical file (`DISCOVERY.md`, `PRD.md`, `JOURNEY-MAP.md`, `ROADMAP.md`, `UX-DECISIONS.md`, `COLOR-SYSTEM.md`, `UI-SPEC.md`, `REVIEW.md`) using the collision-safe `.vN.md` suffix defined in step 4. This applies in addition to the target phase's own archive.

4. Rename existing phase file (if exists), archiving to the **next unused** version suffix so repeat rollbacks never clobber prior history:
   - Find existing archives matching `{BASE}.v*.md` in `.design/phases/`
   - Compute `next_n = max(existing version numbers) + 1` (start at 1 if none exist)
   - Rename the current artifact to `{BASE}.v{next_n}.md`

   Applies to whichever of these files exists for the target phase:
   - `.design/phases/DISCOVERY.md`
   - `.design/phases/PRD.md`
   - `.design/phases/JOURNEY-MAP.md`
   - `.design/phases/ROADMAP.md`
   - `.design/phases/UX-DECISIONS.md`
   - `.design/phases/COLOR-SYSTEM.md`
   - `.design/phases/UI-SPEC.md`

   **Never overwrite an existing archive.** If the computed target somehow already exists, abort the rename and ask the user before proceeding.

**Output:**
```
✓ Returned to: Phase [X] — [Phase Name]

Previous output preserved as: [PHASE_NAME].v1.md

Current Position:
  Phase: [X] of 4 ([Phase Name])
  Status: Ready
  Progress: [██░░░░░░░░] [X]%

Note: Phases [X+1] through [N] may need updates after you modify this phase.

Next: Run /[skill] to redo this phase
      Or /dsp:discuss to capture new context first
```

### Step 5: Provide Context

When going back, remind user of relevant context:

**Going back to Discovery:**
```
CONTEXT FOR DISCOVERY REDO
────────────────────────────────────────────────────────────────────────────────
What triggered going back:
• [from user or inferred]

Existing outputs that may be affected:
• UX-DECISIONS.md — May need updates
• UI-SPEC.md — May need updates

Key questions to reconsider:
• Is the problem statement still accurate?
• Has our understanding of users changed?
• Do requirements need reprioritization?
```

**Going back to PRD (if enabled):**
```
CONTEXT FOR PRD REDO
────────────────────────────────────────────────────────────────────────────────
From Discovery (unchanged):
• Problem: [problem statement]
• User: [primary user]

What may need updating:
• Functional requirements
• Scope boundaries
• Success metrics
• Stakeholder-facing content

UX phase output may need realignment after PRD changes.
```

**Going back to UX:**
```
CONTEXT FOR UX REDO
────────────────────────────────────────────────────────────────────────────────
From Discovery (unchanged):
• Problem: [problem statement]
• User: [primary user]

What may need updating:
• User flows
• State definitions
• Accessibility requirements

Existing UI spec may need realignment after UX changes.
```

### Step 6: Edge Cases

**Going back with uncommitted changes:**
```
Note: You have changes in progress for Phase [N].

Options:
1. Go back anyway (current progress will be marked incomplete)
2. Complete current phase first, then go back
3. Cancel

Choice?
```

**Going back to a skipped phase:**
```
Phase [X] was previously skipped.

Going back will:
• Set Phase [X] as current
• Allow you to complete it properly

This is a good way to fill in a skipped phase.

Continue? (y/n)
```

**Multiple iterations:**

Track version history in STATE.md:
```markdown
## Iteration History

| Phase | Version | Date | Reason |
|-------|---------|------|--------|
| Discovery | v1 | 2024-01-15 | Initial |
| Discovery | v2 | 2024-01-18 | Updated after research |
| UX | v1 | 2024-01-16 | Initial |
```

## Version Management

When going back, preserve history by archiving to the next unused version suffix. Never overwrite an existing `.vN.md` archive:

```
.design/phases/
├── DISCOVERY.md        # Current version (after the latest rollback)
├── DISCOVERY.v1.md     # Archive from first rollback
├── DISCOVERY.v2.md     # Archive from second rollback
├── PRD.md              # Current (may be stale, if enabled)
├── UX-DECISIONS.md     # Current (may be stale)
└── UI-SPEC.md          # Current (may be stale)
```

**Archiving algorithm (must never clobber):**
1. Scan `.design/phases/` for files matching `{BASE}.v*.md`.
2. Extract the highest existing `N` from the matched filenames (default 0).
3. Rename the current `{BASE}.md` to `{BASE}.v{N+1}.md`.
4. If `{BASE}.v{N+1}.md` already exists (race or manual file), stop and ask the user.

**No automatic deletion of archives.** Every `.vN.md` archive is preserved indefinitely — the whole point of this flow is to keep rollback history for reference. If a project accumulates many archives and disk/noise matters, the user can delete `.vN.md` files manually; the workflow never does it for them. (An earlier draft of this command auto-deleted after N=3 archives, which silently destroyed rollback history once a phase was iterated four times. Removed.)

## State Updates

**STATE.md activity log:**
```markdown
### Last Activity
- **Date:** [TIMESTAMP]
- **Action:** Returned to Discovery phase (was on UI)
- **Reason:** Research findings changed user understanding
```

## Integration Notes

- Skills should check STATE.md to see if they're running as a redo
- If redoing, skill can offer to show diff from previous version
- Later phases should be flagged as "may need update" in progress view

---

## Workflow Navigation

| | |
|---|---|
| **This command** | `/dsp:back` — Return to previous phase |
| **Use when** | New information invalidates earlier decisions |
| **Returns to** | The previous phase (or a specific phase if specified) |
| **Then run** | The phase skill for that phase to redo it |
| **Related** | `/dsp:skip` — Skip forward if redo isn't needed after all |
