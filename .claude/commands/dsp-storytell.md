---
name: dsp-storytell
description: Transform design phase output into a ready-to-deliver presentation outline for a specific audience. Cross-phase skill — can be invoked after any DSP phase.
---

# /dsp:storytell — Build a Presentation

You are running the storytelling skill. Invoke the `dsp-storytell` skill immediately.

## What this command does

Packages design work (from any DSP phase or pasted content) into a structured narrative for a specific audience. Produces a complete presentation outline with opening hook, narrative arc, evidence package, Q&A prep, and delivery notes.

## Workflow

### Step 1 — Detect workflow mode

```bash
ls .design/config.json 2>/dev/null
```

If found → workflow mode. Ask which phase output to build the story from:
- Discovery (`.design/phases/DISCOVERY.md`)
- PRD (`.design/phases/PRD.md`)
- Journey map (`.design/phases/JOURNEY-MAP.md`)
- Roadmap (`.design/phases/ROADMAP.md`)
- Research findings (anything under `.design/research/` — e.g., `RESEARCH-PLAN.md`, `FINDINGS-*.md`)
- UX decisions (`.design/phases/UX-DECISIONS.md`)
- UI spec (`.design/phases/UI-SPEC.md`)
- Engineering review (`.design/phases/REVIEW.md`)
- Full workflow (all artifacts)

If not found → standalone mode. Ask the user to describe or paste the content.

### Step 2 — Gather framing

Ask:
- **Audience**: executive / peer / engineering / product / customer
- **Content type**: proposal / review / research readout / one-pager / demo / postmortem
- **Delivery mode**: live presentation / deck to read / memo / demo
- **Duration**: 5 / 15 / 30 / 60 min
- **Stakeholder starting position**: supportive / neutral / skeptical / hostile
- **Worst-case objection**: (to pre-empt in Q&A prep)

### Step 3 — Invoke the skill

The `dsp-storytell` skill drives:
- Narrative framework selection (SCR, NABC, Minto, STAR, 3-Act, Story Spine, etc.)
- Beat-by-beat outline construction
- Evidence package assembly from source artifacts
- Q&A preparation
- SUCCESs check (Simple, Unexpected, Concrete, Credible, Emotional, Story)

### Step 4 — Write output

**Workflow mode:** writes `.design/phases/PRESENTATION-[topic].md` and updates config.json (array — multiple presentations allowed).

**Standalone mode:** outputs inline; offers to save.

### Step 5 — Handoff

After completion, suggest:
- Review and edit the narrative arc
- Build deck/prototype/memo from the outline
- Rehearse with a trusted reviewer
- Run `/dsp:storytell` again for a different audience (same source) if needed

## When to use this command

- Pitching new design work to leadership
- Design reviews with cross-functional teams
- Research findings readouts
- Executive one-pagers
- Prototype demos (internal or external)
- Postmortems / retrospectives
- Preparing responses to design critique

## When NOT to use this command

- Writing requirements → `/dsp:prd`
- Making design decisions → `/dsp:ux`, `/dsp:ui`
- Sprint / standup updates — too tactical for narrative structure

## Workflow Navigation

| | |
|---|---|
| **Cross-phase** | Invokable from any DSP phase |
| **Input** | Any phase artifact (or pasted content) |
| **This** | `/dsp:storytell` — build a presentation |
| **Output** | PRESENTATION-[topic].md — beat-by-beat outline + Q&A prep |
| **Related** | `/dsp:research` — add evidence if thin |
| | `/dsp:verify` — typical predecessor |
| | `/dsp:progress` — see other presentations |
