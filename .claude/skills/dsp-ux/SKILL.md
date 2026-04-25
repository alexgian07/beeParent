---
name: dsp-ux
description: Apply UX/UI design principles for intuitive, accessible interfaces. Use with "/dsp:ux", UX review, usability improvements, or user-centered design. Phase 2 of DSP workflow.
---

# UX/UI Design Excellence

Create interfaces that feel as polished as Linear, Stripe, Notion, and Vercel — products where every interaction feels intentional and effortless.

---

## DSP Workflow Integration

This skill is Phase 2 of the DSP (Design Shit Properly) workflow. It automatically detects and integrates with the workflow when present.

### Detecting Workflow Mode

At the start of any `/dsp:ux` invocation:

1. **Check for `.design/config.json`**
2. **If found** (workflow mode):
   - Load `.design/phases/DISCOVERY.md` for context
   - Load `.design/REQUIREMENTS.md` for requirements to address
   - Check for `02-CONTEXT.md` if `/dsp:discuss` was run first
   - Announce: "Loading context from discovery phase..."
   - Display summary: "Working on: [problem statement]"
3. **If not found** (standalone mode):
   - Run with default behavior
   - Ask: "What would you like me to review for UX principles?"

### Context Loading (Workflow Mode)

When `.design/phases/DISCOVERY.md` exists, extract and display:

```
DSP WORKFLOW ACTIVE
────────────────────────────────────────────────────────────────────────────────
Project: [name]
Phase: 2 of 4 (UX)
Previous: Discovery complete

From Discovery:
• Problem: [problem statement one-liner]
• User: [primary user role]
• Goals: [user goals]
• Key Requirements:
  - [Must-have 1]
  - [Must-have 2]
  - [Must-have 3]

Your Focus This Phase:
Apply UX principles to design user flows and component states.
────────────────────────────────────────────────────────────────────────────────

Ready to proceed? Or /dsp:back to revisit discovery.
```

### Context Sources

| From Discovery | Used For |
|----------------|----------|
| Problem statement | Focus area for UX work |
| Primary user | Persona context for decisions |
| User goals | Flow design targets |
| Requirements | Constraint checklist |
| Constraints | Design boundaries |
| Open questions | Items to address |
| Journey map | Starting point for flows |

---

## Workflow

1. **Always combine with frontend-design skill** — Read and apply both skills together
2. **Audit against UX principles** — Check references/usability-principles.md
3. **Verify accessibility** — Check references/accessibility-checklist.md
4. **Apply proven patterns** — Check references/ux-patterns.md for states, feedback, navigation
5. **Benchmark against excellence** — Check references/product-excellence.md for inspiration

---

## Cognitive Foundations

Before applying UX principles, understand *why* they work. Seven cognitive domains from human psychology directly govern how users perceive, process, and decide within interfaces — plus two well-documented biases that distort the designer's own judgment. Use these to explain decisions, not just make them.

### Memory & Knowledge
- People remember recognition cues better than raw recall — always show options, never ask users to type something they've seen elsewhere
- **Information scent:** strong visual and textual cues prime users toward the right path; weak scent causes disorientation and abandonment
- Present all information needed for a task on a single screen — context-switching kills working memory
- *Design implication:* menus, autocomplete, breadcrumbs, and in-context help are memory aids, not UX luxuries

### Attention
- Humans have a finite attentional budget — every element competes for it
- Attention snaps to contrast: things that differ from neighbors (color, size, motion, shape) win
- People only attend to stimuli relevant to their **current goal** — off-task information is invisible noise
- *Design implication:* use visual contrast to direct, not decorate; remove anything that doesn't serve the active task

### Mental Models
- Users arrive with pre-built mental models from past experiences (other apps, physical analogies)
- New information that cannot be integrated into an existing mental model cannot be comprehended or remembered
- Forcing users to rebuild their mental model = high drop-off
- *Design implication:* match familiar patterns for familiar tasks; only deviate when the benefit is obvious and immediately learnable

### Perception
- Context shapes what users see — the same element is perceived differently depending on what surrounds it
- Objects at the periphery are perceived with less detail than focal objects
- Experience (expertise) changes what users notice and what they filter out
- *Design implication:* high-priority actions belong at focal points; don't rely on peripheral placement for critical feedback

### Language & Reading
- Web users scan, they don't read — they look for signposts: headings, bold text, links, and short paragraphs
- **Context-first writing:** lead with the conclusion, put the qualification after — the opposite of how people instinctively write
- Cognitive load for reading increases with line length, jargon, and passive voice
- *Design implication:* IA and copywriting are UX, not afterthoughts; every label is a navigation decision

### Emotion
- "Attractive things work better." (Don Norman, 2004) — the **aesthetic-usability effect** (Kurosu & Kashimura, 1995): perceived beauty directly raises perceived usability and trust
- Emotional state affects cognitive performance: positive affect increases creative problem-solving, negative affect narrows focus
- Users form aesthetic judgments in milliseconds — before they've read anything
- *Design implication:* visual polish is not vanity; it directly influences whether users trust the product and persist through difficulty

### Thinking & Decision-Making
- People choose the option that maximizes **perceived** utility — not actual utility. How you present options changes what users pick
- **Framing effects** (Levin, 1987): "90% success rate" and "10% failure rate" are identical but drive different decisions
- **Loss aversion** (Kahneman & Tversky, 1979): users feel losses ~2× more strongly than equivalent gains — "don't miss out" > "gain this"
- **Paradox of choice** (Iyengar & Lepper, 2000): too many options cause decision paralysis and lower satisfaction with the chosen option
- *Design implication:* defaults, framing, and option count are decision-shaping tools — use them intentionally and ethically

### Designer Bias to Guard Against
Two biases will quietly corrupt UX work if left unchecked:
- **False consensus effect** (Ross et al., 1977): designers assume users share their mental models, vocabulary, and priorities. They don't. Validate with actual users before treating your intuition as truth.
- **Inattentional blindness** (Hyman et al., 2009): users focused on a task literally do not see elements outside that task's visual path — no matter how "obvious" the element seems to you. If a feature matters, it belongs in the task's attentional path.

---

## Core UX Principles

### Clarity Over Cleverness

- One primary action per screen/component
- Labels over icons alone (or icon + tooltip minimum)
- Show, don't tell — progressive disclosure over upfront complexity

### Respect Cognitive Load

- Chunk information (7±2 items max in lists/groups) — respects working memory limits (Miller, 1956)
- Sensible defaults — users shouldn't configure before starting
- Recognition over recall — show options, don't make users remember (see Memory above)
- Match existing mental models before introducing new patterns — new paradigms require explicit onboarding
- Strong information scent at every decision point — ambiguous labels cause dead ends

### Language & Content Hierarchy

- Write context-first: conclusion before qualification
- Structure content for scanning: headings, short paragraphs, bold key terms, meaningful links
- Use plain language — if a 12-year-old can't parse it, rewrite it
- Every UI label is a navigation decision; treat copywriting as part of the UX spec

### Feedback & Responsiveness

- Every action needs visible feedback (< 100ms perceived)
- Optimistic UI for common actions
- Clear system status — loading, success, error states always visible

### Error Prevention > Error Messages

- Disable invalid actions, don't just error on them
- Inline validation as users type, not on submit
- Undo over confirmation dialogs when possible

### Consistency & Predictability

- Same action = same result everywhere
- Standard patterns for standard tasks (don't reinvent navigation)
- Spatial consistency — elements don't jump between states

### Mobile-First Thinking

- Touch targets minimum 44×44px
- Thumb-zone awareness for primary actions
- Content hierarchy must work at 320px width

---

## State Coverage Matrix

**Every interactive component needs ALL of these states defined:**

| State | Description | Required? |
|-------|-------------|-----------|
| **Default** | Normal resting state | Yes |
| **Hover** | Mouse over (desktop) | Yes |
| **Focus** | Keyboard focus (visible ring) | Yes |
| **Active/Pressed** | Being clicked/tapped | Yes |
| **Disabled** | Cannot interact | When applicable |
| **Loading** | Async operation in progress | For async actions |
| **Error** | Something went wrong | For forms/inputs |
| **Empty** | No content/data | For lists/tables |
| **Success** | Operation completed | For confirmations |

**In workflow mode, explicitly document each state for key components.**

---

## Implementation Checklist

Before finalizing any UI:

- [ ] Can a new user complete the primary task without instructions?
- [ ] Are all interactive states covered? (see matrix above)
- [ ] Does it work with keyboard only?
- [ ] Is color not the only indicator of meaning?
- [ ] Are error messages actionable? (what happened + how to fix)
- [ ] Does it feel fast? (perceived performance)
- [ ] Is the most important action visually dominant?
- [ ] Does the design match users' existing mental models, or does it explain the departure?
- [ ] Is all information needed for each task present on the same screen? (no context-switching required)
- [ ] Is the information hierarchy scannable — headings, bold, short paragraphs, meaningful links?
- [ ] Does contrast and visual weight guide attention to the primary action?
- [ ] Have option counts, defaults, and framing been chosen deliberately — not by accident?
- [ ] Has every "users will obviously notice this" claim been validated, not assumed? (guard against false consensus)

---

## Output Structure (Workflow Mode)

When in workflow mode, produce structured output:

### UX-DECISIONS.md Structure

```yaml
---
phase: ux
skill: dsp-ux
completed: YYYY-MM-DDTHH:MM:SSZ
context_loaded:
  - DISCOVERY.md
  - 02-CONTEXT.md (if existed)
requirements_addressed:
  - REQ-01
  - REQ-02
components_specified:
  - ComponentName1
  - ComponentName2
---

# UX Decisions: [Feature Name]

## User Flow

[Flow diagram or description]

```
[Entry Point] → [Step 1] → [Decision Point] → [Step 2a/2b] → [Success State]
                              ↓
                         [Error Path]
```

## Usability Principles Applied

| Principle | Application | Cognitive Basis | Rationale |
|-----------|-------------|-----------------|-----------|
| Clarity | [How applied] | [Memory / Attention / Mental Models / Perception / Language / Emotion / Thinking] | [Why] |
| Cognitive Load | [How applied] | [Memory / Mental Models / Thinking] | [Why] |
| ... | ... | ... | ... |

## Key UX Decisions

| Decision | Options Considered | Chosen | Rationale |
|----------|-------------------|--------|-----------|
| [Decision 1] | A, B, C | B | [Why B was chosen] |
| ... | ... | ... | ... |

## Component Behaviors

### [Component 1]

**Purpose:** [What it does]

**States:**
| State | Behavior | Visual Indicator |
|-------|----------|------------------|
| Default | [behavior] | [visual] |
| Hover | [behavior] | [visual] |
| Focus | [behavior] | [visual] |
| Active | [behavior] | [visual] |
| Disabled | [behavior] | [visual] |
| Loading | [behavior] | [visual] |
| Error | [behavior] | [visual] |
| Empty | [behavior] | [visual] |
| Success | [behavior] | [visual] |

**Interactions:**
- Click: [what happens]
- Keyboard: [what keys do what]
- Touch: [mobile behavior]

### [Component 2]
...

## Accessibility Requirements

| Requirement | WCAG | Implementation |
|-------------|------|----------------|
| Keyboard navigation | 2.1.1 | [How] |
| Focus visibility | 2.4.7 | [How] |
| Color independence | 1.4.1 | [How] |
| Label association | 1.3.1 | [How] |
| Touch targets | 2.5.5 | [How] |

## Error Handling

| Error Scenario | User Message | Recovery Action |
|----------------|--------------|-----------------|
| [Scenario 1] | [Message] | [What user can do] |
| ... | ... | ... |

## Empty States

| Empty State | What Shows | User Action |
|-------------|------------|-------------|
| No data | [Message + visual] | [CTA] |
| No results | [Message] | [Suggestions] |
| First use | [Onboarding] | [Getting started] |

## Loading States

| Operation | Loading Indicator | Placement |
|-----------|-------------------|-----------|
| Page load | [Skeleton/Spinner] | [Where] |
| Form submit | [Button state] | [In button] |
| Data fetch | [Placeholder] | [In container] |

## Requirements Coverage

| Requirement | Addressed By | Notes |
|-------------|--------------|-------|
| REQ-01: [text] | [Component/Flow] | |
| REQ-02: [text] | [Component/Flow] | |

## Handoff Notes for UI Phase

- Key visual direction: [guidance]
- Critical interactions: [list]
- Accessibility priorities: [list]
- Open visual questions: [list]
```

---

## State Updates (Workflow Mode)

After completing UX analysis:

1. **Write output to `.design/phases/UX-DECISIONS.md`**

2. **Update `.design/STATE.md`**:
```markdown
## Current Position
Phase: 2 of 4 (UX)
Status: completed
Progress: [█████░░░░░] 50%

### Last Activity
- **Date:** [TIMESTAMP]
- **Action:** Completed UX phase with /dsp:ux
- **User:** [session user]

### Accumulated Context
...
#### Major Decisions Made
| Phase | Decision | Impact |
|-------|----------|--------|
| Discovery | [decision] | [impact] |
| UX | [key UX decision 1] | [impact] |
| UX | [key UX decision 2] | [impact] |
```

3. **Update `.design/config.json`**:
```json
{
  "workflow": {
    "current_phase": 3,
    "phases_completed": ["discovery", "ux"],
    "workflow_status": "in_progress"
  }
}
```

4. **Update `.design/REQUIREMENTS.md`** with addressed requirements

---

## Handoff (Workflow Mode)

```
═══════════════════════════════════════════════════════════════════════════════
UX DECISIONS COMPLETE
═══════════════════════════════════════════════════════════════════════════════

Output: .design/phases/UX-DECISIONS.md

Summary:
• User flow documented with [N] steps
• [M] components specified with full state coverage
• [P] accessibility requirements documented
• Requirements addressed: [list]

Progress: [█████░░░░░] 50%

Ready for Next Phase?
────────────────────────────────────────────────────────────────────────────────
→ /dsp:ui to continue to visual design (recommended)
→ /dsp:discuss to capture visual direction first

Or:
→ /dsp:progress to review full status
→ /dsp:back to revisit discovery

═══════════════════════════════════════════════════════════════════════════════
```

---

## Standalone Mode Behavior

When no `.design/` directory exists:

1. Output UX analysis inline (current behavior)
2. Ask standard handoff question:
   > "Would you like me to run `/dsp:ui` for visual design principles?"
3. Optionally offer: "Or start a full DSP workflow with `/dsp:start`?"

---

## Reference Files

Load these as needed for detailed guidance:

- **references/usability-principles.md** — Nielsen's heuristics + modern interpretations
- **references/accessibility-checklist.md** — Essential WCAG compliance
- **references/ux-patterns.md** — Patterns for states, feedback, navigation, forms
- **references/product-excellence.md** — What makes Linear, Stripe, Notion exceptional

---

## Config Integration

Respects these settings from `.design/config.json`:
```json
{
  "phases": {
    "ux": {
      "enabled": true,
      "includeAccessibility": true,
      "includeAllStates": true
    }
  }
}
```

When `includeAccessibility: true`, always include accessibility requirements section.
When `includeAllStates: true`, require full state matrix for all components.

---

## Workflow Navigation

```
                                         ┌─────────┐
/dsp:start    →    /dsp:discovery    →    │ YOU ARE  │    →    /dsp:execute    →    /dsp:ui    →    /dsp:execute    →    /dsp:eng_review    →    /dsp:verify
                    Phase 1               │  HERE   │         (wireframe)          Phase 3       (polished)          Phase 4
                                         │ Phase 2 │
                                         └─────────┘
```

| | |
|---|---|
| **Previous** | `/dsp:discovery` — Discovery & requirements (Phase 1) |
| **Current** | `/dsp:ux` — UX principles & states (Phase 2) |
| **Next** | `/dsp:execute` — Generate wireframe implementation |
| **Related** | `/dsp:discuss` — Capture context before this phase |
| | `/dsp:back` — Return to discovery if requirements changed |
