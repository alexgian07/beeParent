---
name: dsp-discuss
description: Capture design decisions and context before running a phase skill. Creates a CONTEXT.md file with resolved gray areas, assumptions, and direction.
---

# /dsp:discuss — Pre-Phase Decision Capture

You are facilitating a discussion to capture context before running a phase skill. This ensures decisions are documented and the skill has clear direction.

## When to Use

- Before any phase when there are unclear aspects
- When multiple valid approaches exist
- When stakeholder input needs documenting
- When assumptions need explicit acknowledgment

## Workflow

### Step 1: Check Workflow Exists

```bash
ls .design/config.json 2>/dev/null
```

**If not found:**
```
No DSP workflow found. Start one with /dsp:start first.
```

### Step 2: Determine Target Phase

`/dsp:discuss` can capture context for any phase, including optional ones. Don't assume the target is the next pending main phase — prompt the user.

1. Read `.design/STATE.md` and `.design/config.json` to compute the **default target**: if `workflow.current_optional_phase` is set, default to that optional subphase; otherwise default to the next pending main phase (from `workflow.current_phase` and `phases_completed`).
2. Show the user the computed default and the full list of enabled phases they can target:

```
Which phase do you want to capture context for?

Default: [computed default, e.g., "UX (Phase 2)"]

Available phases:
  1. Discovery
  [1.5a. PRD]         (if optional_phases.prd.enabled)
  [1.5b. Journey]     (if optional_phases.journey.enabled)
  [1.5c. Roadmap]     (if optional_phases.roadmap.enabled)
  2. UX
  [2.5. Color System] (if optional_phases.color_system.enabled)
  3. UI
  4. Review
  Storytell: [topic]  (cross-phase; prompt for a topic slug if selected)

Enter a label (or press enter for the default):
```

3. Validate the selection against enabled phases (reject disabled optional targets with: *"Phase X is not enabled for this workflow. Enable it in `config.json` first, or pick another."*).
4. For storytelling, prompt for a topic slug (free text, kebab-case recommended) — this becomes the `STORYTELL-[topic]-CONTEXT.md` filename suffix.
5. The selected phase drives which question branch runs in Step 4 and which CONTEXT filename is written in Step 5.

### Step 3: Load Previous Context

If not the first phase, load relevant previous phase documents:
- For UX: Load DISCOVERY.md
- For UI: Load DISCOVERY.md + UX-DECISIONS.md
- For Review: Load all previous phases

Display summary:
```
CONTEXT FROM PREVIOUS PHASES
────────────────────────────────────────────────────────────────────────────────
From Discovery:
• Problem: [one-liner]
• Primary User: [role]
• Key Requirements: [top 3]

Ready to discuss [Phase Name] phase direction.
```

### Step 4: Facilitate Discussion

Ask questions based on the phase:

**For Discovery (`/dsp:discovery`):**
1. "What do you already know about this feature/problem?"
2. "Are there any assumptions you want me to challenge particularly hard?"
3. "Any areas I should NOT probe deeply? (sensitive topics, already decided, etc.)"
4. "What format would be most useful for the output?"

**For UX (`/dsp:ux`):**
1. "Any specific user flows you want to prioritize?"
2. "Are there UI patterns from elsewhere in the product we should match?"
3. "Any accessibility requirements beyond WCAG AA?"
4. "What states are most critical to define? (error handling, empty states, etc.)"

**For UI (`/dsp:ui`):**
1. "Any brand guidelines or design system constraints?"
2. "Should we optimize for information density or breathing room?"
3. "Are there specific components you need detailed specs for?"
4. "Any existing visual patterns we must match?"

**For Review (`/dsp:eng_review`):**
1. "What files/components should I review?"
2. "Any known issues you want me to focus on?"
3. "What's the quality threshold? (strict vs. pragmatic)"
4. "Should I include spec alignment check against the design phases?"

**For PRD (`/dsp:prd`):**
1. "What audience is the PRD primarily for? (executive, engineering, design, mixed, Claude Code)"
2. "What output format? (docx, HTML, markdown, Claude Code spec)"
3. "Do you want competitive research included?"
4. "Any stakeholder decisions already made that shouldn't be re-opened?"

**For Journey (`/dsp:journey`):**
1. "Which artifact type fits best? (customer journey, experience map, day-in-the-life, service blueprint, omnichannel journey)"
2. "Is this a current-state map (diagnose what exists) or future-state (vision)?"
3. "What research grounds this? (interviews, analytics, support tickets — or hypothesis mode)"
4. "Which persona and scenario are in scope?"

**For Roadmap (`/dsp:roadmap`):**
1. "Which roadmap type? (now/next/future, outcome-based, theme-based, lean)"
2. "What are the high-level goals this roadmap serves?"
3. "Which prioritization framework? (RICE, ICE, MoSCoW, Kano, Value/Effort, Opportunity Scoring)"
4. "Audience mix — who consumes this (executive, team, stakeholders)?"

**For Color System (`/dsp:color`):**
1. "Base hue or brand direction?"
2. "Accessibility level — WCAG AA or AAA?"
3. "Include dark mode?"
4. "Gamut target — sRGB only, or also P3?"

**For Storytelling (`/dsp:storytell`):**
1. "Which audience? (executive, peer, engineering, product, customer)"
2. "Which content type? (proposal, review, research readout, exec one-pager, demo, postmortem)"
3. "Delivery mode and duration? (live 15min, deck-to-read, memo, demo)"
4. "Stakeholder starting position — supportive, neutral, skeptical, hostile?"
5. "Worst-case objection to pre-empt in Q&A prep?"

### Step 5: Capture Decisions

Create `.design/phases/{NN}-CONTEXT.md` where NN is the phase number:
- `01-CONTEXT.md` for Discovery
- `01.5a-CONTEXT.md` for PRD (optional)
- `01.5b-CONTEXT.md` for Journey (optional)
- `01.5c-CONTEXT.md` for Roadmap (optional)
- `02-CONTEXT.md` for UX
- `02.5-CONTEXT.md` for Color System (optional)
- `03-CONTEXT.md` for UI
- `04-CONTEXT.md` for Review
- `STORYTELL-[topic]-CONTEXT.md` for Storytelling (cross-phase; topic suffix so multiple coexist — loaded opportunistically by `/dsp:storytell`)

Populate with:
- Timestamp
- Phase name
- Gray areas resolved
- Assumptions made
- Constraints acknowledged
- Preferred approach
- Rejected alternatives
- Open questions

### Step 6: Confirm and Handoff

```
✓ Context captured for [Phase Name] phase

Saved to: .design/phases/[NN]-CONTEXT.md

Summary:
• [Key decision 1]
• [Key decision 2]
• [Assumption acknowledged]

Ready to run /[skill-name] with this context.
Continue? (y/n)
```

If yes, provide instructions to run the phase skill.

## Context Template

Use this inline template, replacing placeholders with gathered information:

```markdown
# Phase Context: [PHASE_NAME]

> Captured: [TIMESTAMP]
> Phase: [PHASE_NUMBER] — [PHASE_NAME]

## Pre-Phase Discussion

> Decisions and context captured before running the phase skill

### What We're Working On
> Brief description of the focus for this phase

### Gray Areas Resolved

> Questions that were unclear before starting, now answered

| Question | Answer | Decided By |
|----------|--------|------------|
| | | |

### Assumptions Made

> Things we're assuming to be true for this phase

1.
2.
3.

### Constraints Acknowledged

> Limitations we're working within

-
-

---

## Design Direction

### Preferred Approach
> If multiple approaches exist, which direction are we taking?

### Rejected Alternatives
> What did we consider but decide against, and why?

| Alternative | Why Rejected |
|-------------|--------------|
| | |

---

## Inputs for This Phase

### From Previous Phase
> Key context carried forward

-

### New Information
> Anything new since the last phase

-

### User/Stakeholder Input
> Relevant feedback or requests

-

---

## Expected Outputs

### Must Produce
- [ ]

### Should Produce
- [ ]

### Nice to Have
- [ ]

---

## Open Questions

> Questions that remain unanswered going into this phase

1.
2.

---

## Notes

> Any additional context that might be helpful
```

## Phase Mapping

| Phase # | Phase Name | Skill | Context File |
|---------|------------|-------|--------------|
| 1 | Discovery | /dsp:discovery | 01-CONTEXT.md |
| 1.5a | PRD (optional) | /dsp:prd | 01.5a-CONTEXT.md |
| 1.5b | Journey (optional) | /dsp:journey | 01.5b-CONTEXT.md |
| 1.5c | Roadmap (optional) | /dsp:roadmap | 01.5c-CONTEXT.md |
| 2 | UX | /dsp:ux | 02-CONTEXT.md |
| 2.5 | Color System (optional) | /dsp:color | 02.5-CONTEXT.md |
| 3 | UI | /dsp:ui | 03-CONTEXT.md |
| 4 | Review | /dsp:eng_review | 04-CONTEXT.md |
| cross-phase | Storytelling | /dsp:storytell | STORYTELL-[topic]-CONTEXT.md (in `.design/phases/`) |

## Optional: Research Context

If user wants to discuss research before `/dsp:research`:

1. "What are the key unknowns you want to validate?"
2. "Who do you have access to for research?"
3. "What method are you leaning toward?"
4. "Timeline constraints for research?"

Save to: `.design/research/RESEARCH-CONTEXT.md`

## Integration Notes

- Skills will check for and load the context file when they run
- Context file is loaded AFTER the main phase document
- Decisions in context file take precedence over defaults
- Update STATE.md to note that discussion occurred

---

## Workflow Navigation

| | |
|---|---|
| **This command** | `/dsp:discuss` — Pre-phase decision capture |
| **Use before** | Any phase skill (`/dsp:discovery`, `/dsp:ux`, `/dsp:ui`, `/dsp:eng_review`) |
| **Then run** | The phase skill that discussion was preparing for |
| **Related** | `/dsp:progress` — See which phase is next |
