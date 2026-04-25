---
name: dsp-roadmap
description: Build a theme-based UX roadmap using NNGroup's Now/Next/Future methodology. Optional Phase 1.5c in DSP workflow, between Discovery and UX.
---

# /dsp:roadmap — UX Roadmap Phase

You are running the UX roadmapping phase of the DSP workflow. Invoke the `dsp-roadmap` skill immediately.

## What this command does

Produces a strategic, living roadmap artifact using Sarah Gibbons' (NNGroup) methodology:
- Theme-based, not feature-based
- Now / Next / Future time horizons (default), or outcome-based / theme-ranked / lean variants
- Every theme includes Beneficiary, Need, Business Objective, Confidence, Disclaimers
- Integrates prioritization (RICE, ICE, MoSCoW, Kano, Value/Effort, or Opportunity Scoring)

## Workflow

### Step 1 — Detect workflow mode

```bash
ls .design/config.json 2>/dev/null
```

If found → workflow mode. Load:
- `.design/config.json`
- `.design/phases/DISCOVERY.md`
- `.design/phases/JOURNEY-MAP.md` (if exists)
- Any research artifacts under `.design/research/` (e.g., `RESEARCH-PLAN.md`, `FINDINGS-*.md`)
- `.design/phases/PRD.md` (if exists)
- `.design/phases/01.5c-CONTEXT.md` (if `/dsp:discuss` was run)

If not found → standalone mode. Ask scope (team / product / portfolio) and whether new vs. updating.

### Step 2 — Invoke the skill

Invoke the `dsp-roadmap` skill. The skill drives:
- Roadmap type selection (4 options)
- High-level goals and scope definition
- 6-step process: Goals → Inputs → Themes → Prioritize → Visualize → Revisit
- Prioritization framework selection
- Output generation

### Step 3 — Write output

**Workflow mode:** writes `.design/phases/ROADMAP.md` and updates STATE.md, config.json.

**Standalone mode:** outputs inline; offers to save.

### Step 4 — Handoff

After completion, suggest next step:
- `/dsp:ux` — continue to UX phase (work on Now themes)
- `/dsp:prd` — generate PRD for highest-priority theme
- `/dsp:research` — validate low-confidence Future themes

## When to use this command

- Quarterly / annual UX planning at team or org level
- Aligning stakeholders around a problem portfolio
- Converting journey-map opportunities into prioritized work
- Replacing a feature-based roadmap with a theme-based one
- Re-planning after a strategic shift

## When NOT to use this command

- Sprint planning or backlog — too tactical, use a PM tool
- Feature specs — use `/dsp:prd`
- Individual feature design — use `/dsp:ux` + `/dsp:ui`
- Engineering/tech-debt roadmaps — different audience and criteria

## Workflow Navigation

| | |
|---|---|
| **Previous** | `/dsp:discovery` — Discovery (Phase 1) |
| **Parallel** | `/dsp:prd` (1.5a), `/dsp:journey` (1.5b) |
| **This** | `/dsp:roadmap` — Roadmap (Phase 1.5c) |
| **Next** | `/dsp:ux` — UX phase (Phase 2) |
| **Related** | `/dsp:research` — Close open questions |
| | `/dsp:discuss` — Capture context first |
| | `/dsp:back` — Return to discovery |
