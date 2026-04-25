---
name: dsp-journey
description: Build a customer journey map, service blueprint, or omnichannel experience map using NNGroup methodology. Optional phase 1.5b in DSP workflow, runs between Discovery and UX.
---

# /dsp:journey — Journey Mapping Phase

You are running the journey-mapping phase of the DSP workflow. Invoke the `dsp-journey` skill immediately.

## What this command does

Produces one of five journey artifacts using NNGroup's methodology:
- Customer journey map (default)
- Experience map
- Day-in-the-life map
- Service blueprint
- Omnichannel journey map

Each artifact follows Kaplan's 5 principles: establish why/what, base on truth, collaborate, don't rush to visualization, engage others.

## Workflow

### Step 1 — Detect workflow mode

```bash
ls .design/config.json 2>/dev/null
```

If found → workflow mode. Load:
- `.design/config.json`
- `.design/phases/DISCOVERY.md`
- `.design/phases/PRD.md` (if exists)
- Any research artifacts under `.design/research/` (e.g., `RESEARCH-PLAN.md`, `FINDINGS-*.md`)
- `.design/phases/01.5b-CONTEXT.md` (if `/dsp:discuss` was run)

If not found → standalone mode. Ask the user whether they want to run the full DSP workflow with `/dsp:start`, or proceed standalone.

### Step 2 — Invoke the skill

Invoke the `dsp-journey` skill. The skill drives:
- Artifact type selection
- Scope definition (actor + scenario)
- Research basis check (→ hypothesis mode if missing)
- Six-step synthesis process
- Output generation

### Step 3 — Write output

**Workflow mode:** writes `.design/phases/JOURNEY-MAP.md` and updates STATE.md, config.json.

**Standalone mode:** outputs inline; offers to save.

### Step 4 — Handoff

After completion, suggest next step:
- `/dsp:ux` — continue to UX phase (most common)
- `/dsp:research` — if hypothesis-mode map needs validation
- `/dsp:prd` — if PRD hasn't been generated and journey insights should feed it

## When to use this command

- Multi-step customer experiences (onboarding, purchase, renewal, support)
- Omnichannel experiences (web + mobile + email + in-person)
- Services with backstage operations (service blueprint)
- Pre-product discovery (experience map, day-in-the-life)
- Diagnosing why a journey is underperforming

## When NOT to use this command

- Single-screen UX decisions → use `/dsp:ux`
- Feature specs → use `/dsp:prd`
- Requirements gathering → use `/dsp:discovery`
- Research interviews → use `/dsp:research`

## Workflow Navigation

| | |
|---|---|
| **Previous** | `/dsp:discovery` — Discovery (Phase 1) |
| **Parallel** | `/dsp:prd` — PRD (Phase 1.5a) |
| **This** | `/dsp:journey` — Journey mapping (Phase 1.5b) |
| **Next** | `/dsp:ux` — UX phase (Phase 2) |
| **Related** | `/dsp:research` — Validate hypotheses |
| | `/dsp:discuss` — Capture context first |
| | `/dsp:back` — Return to discovery |
