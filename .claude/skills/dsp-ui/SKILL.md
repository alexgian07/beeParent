---
name: dsp-ui
description: >
  Apply visual design principles, design tokens, component specs, and modern CSS to create
  polished, professional interfaces at the level of Linear, Stripe, and Vercel. Trigger with
  "/dsp:ui" or when designing dashboards, data-dense interfaces, enterprise applications, or any
  visual UI work. Covers grids, typography, animation, dark mode, B2B patterns, data
  visualization, and pre-delivery quality gates. Phase 3 of the DSP workflow.
---

# UI Design Excellence

Create interfaces with the visual rigor of Stripe, the data density of Bloomberg, and the clarity of Linear — polished, professional, and built for real users.

## How to Use This Skill

1. **Check references as needed** — load the relevant reference file for depth
2. **Apply priority rules below** — work top-down through the tiers
3. **Delegate to sibling skills** — color to `/dsp:color`, interaction to `/dsp:ux`, code review to `/dsp:eng_review`
4. **Run the checklist** — check `references/pre-delivery-checklist.md` before shipping

## Reference Files

| File | When to Load |
|------|-------------|
| `references/design-tokens-and-theming.md` | Setting up tokens, theming, dark mode, Tailwind v4 |
| `references/component-visual-specs.md` | Specifying component dimensions, states, transitions |
| `references/modern-css-2026.md` | Container queries, scroll animations, view transitions, fluid type |
| `references/visual-design-principles.md` | Grids, hierarchy, Gestalt, typography, motion, elevation |
| `references/typography.md` | Deep type craft — modular scale, font selection, anti-defaults, OpenType, fluid vs fixed |
| `references/motion.md` | UI motion — durations, easing curves, reduced-motion, perceived performance, Lottie callout |
| `references/ux-writing.md` | Copy rigor — button labels, error message formula, empty states, voice/tone, i18n |
| `references/b2b-enterprise-patterns.md` | Dashboards, tables, filters, forms, navigation, bulk ops |
| `references/data-visualization.md` | Charts, real-time data, domain-specific displays |
| `references/aesthetic-archetypes.md` | Applying aesthetic direction, archetype-to-tokens mapping, standalone quick-start |
| `references/pre-delivery-checklist.md` | Final quality audit before shipping |

## Sibling Skill Cross-References

| Need | Use | Why |
|------|-----|-----|
| Color palettes, OKLCH values, shade ramps, contrast checking | `/dsp:color` | Owns color science and generation |
| Interaction design, cognitive load, navigation behavior | `/dsp:ux` | Owns usability and interaction patterns |
| Code review, WCAG compliance in code, React/Tailwind patterns | `/dsp:eng_review` | Owns implementation quality |

---

## Priority Design Rules

Work through these top-down. Higher tiers are non-negotiable; lower tiers add polish.

### Tier 1: Accessibility Visual (CRITICAL)

| Rule | Standard | Description |
|------|----------|-------------|
| `target-size` | WCAG 2.5.8 | Pointer targets >= 24x24px (44px preferred). Extend hit area with padding if visual is smaller |
| `focus-visible` | WCAG 2.4.11 | Visible focus ring on all focusable elements: 2px+ outline, `:focus-visible` only |
| `focus-not-obscured` | WCAG 2.4.11 | Focused element never entirely hidden by sticky headers, footers, or overlays |
| `color-not-only` | WCAG 1.4.1 | Never use color as sole indicator. Pair with icon, text, or pattern |
| `contrast-text` | WCAG 1.4.3 | 4.5:1 for normal text, 3:1 for large text (18px+ or 14px bold) |
| `contrast-ui` | WCAG 1.4.11 | 3:1 for UI component borders, icons, and graphical elements |
| `drag-alternative` | WCAG 2.5.7 | All drag operations must have single-pointer alternatives |
| `reduced-motion` | WCAG 2.3.3 | Respect `prefers-reduced-motion` — reduce or disable all animations |

### Tier 2: Layout & Spacing (CRITICAL)

| Rule | Description |
|------|-------------|
| `grid-8px` | All spacing uses 8px multiples (4px for fine adjustments) |
| `12-col-grid` | Use 12-column grid for complex layouts. Gutters: 16/24/32px |
| `container-width` | Cap content width on large screens (max-w-7xl or similar) |
| `no-horizontal-scroll` | Content fits viewport width at every breakpoint |
| `safe-areas` | Respect notch, gesture bar, Dynamic Island. No content behind system UI |
| `spacing-rhythm` | Same relationship = same spacing. Component: 4-16px. Section: 24-48px |
| `container-queries` | Use container queries for reusable components, viewport queries for page layout |

### Tier 3: Visual Hierarchy & Typography (HIGH)

| Rule | Description |
|------|-------------|
| `one-primary-cta` | Each screen has exactly one visually dominant action. Others are secondary/ghost |
| `squint-test` | Blur your vision — most important element should still be identifiable |
| `type-scale` | Use a systematic scale (not arbitrary sizes). Fluid `clamp()` for responsive |
| `weight-hierarchy` | Bold (600-700) headings, medium (500) labels, regular (400) body |
| `display-font` | Use display variant for headings 20px+ (Inter Display, etc.) |
| `line-length` | 50-75 chars desktop, 35-60 chars mobile |
| `tabular-nums` | Use `font-variant-numeric: tabular-nums` for data columns and prices |
| `variable-fonts` | Single file, continuous weight axis, `font-display: swap` |

### Tier 4: States & Interaction Visuals (HIGH)

| Rule | Description |
|------|-------------|
| `state-priority` | disabled > loading > active > focus > hover > default |
| `transition-timing` | Color/bg: 150ms ease-in-out. Transform: 200ms ease-out. Opacity: 150ms |
| `focus-ring` | 2px ring, 2px offset, primary color. `:focus-visible` only |
| `disabled-state` | 50% opacity, `pointer-events: none`, `cursor: not-allowed` |
| `loading-skeleton` | Skeleton screens for content areas, spinners for button actions |
| `press-feedback` | `scale(0.98)` on press for buttons/cards, restored on release |
| `exit-faster` | Exit animations 60-70% of enter duration |
| `spring-physics` | Prefer spring/physics-based easing for natural feel |

### Tier 5: Icons & Imagery (MEDIUM)

| Rule | Description |
|------|-------------|
| `no-emoji-icons` | Use SVG vector icons (Lucide, Heroicons). Never emoji for UI controls |
| `icon-consistency` | One icon family, same stroke width, same corner radius throughout |
| `icon-sizing-tokens` | Define icon sizes as tokens: sm (14px), md (18px), lg (24px) |
| `icon-contrast` | 4.5:1 for small icons, 3:1 for large UI glyphs |
| `icon-alignment` | Align icons to text baseline with consistent padding |

### Tier 6: Elevation & Depth (MEDIUM)

| Rule | Description |
|------|-------------|
| `shadow-scale` | Use consistent elevation levels (xs → sm → md → lg → xl) |
| `surface-layering` | Create depth through layered surfaces, not just shadows |
| `dark-elevation` | In dark mode: higher elevation = lighter surface (not darker) |
| `dark-borders` | Replace shadows with subtle borders in dark mode |
| `glassmorphism-subtle` | Use sparingly: overlays and floating panels in dark UIs only |
| `z-index-scale` | Defined scale: base(0) → raised(10) → dropdown(20) → modal(50) → toast(70) |

### Tier 7: Polish & Premium Feel (MEDIUM)

| Rule | Description |
|------|-------------|
| `brand-tinted-neutrals` | Never pure gray. Add trace of brand hue (chroma 0.005-0.02 in OKLCH) |
| `dark-mode-first` | Design dark mode as primary, derive light. Better dark interfaces |
| `ambient-indicators` | Subtle status: thin color bars, dot badges, tinted backgrounds. Not intrusive alerts |
| `physical-light` | Model consistent light source. Top edges catch light, shadows follow direction |
| `semantic-tokens-only` | No hardcoded hex/rgb in components. Always reference design tokens |
| `consistent-radius` | Use radius tokens. Same radius for same component type throughout |

---

## Token Architecture (Quick Reference)

Three layers — see `references/design-tokens-and-theming.md` for full specs.

```
Primitive (raw values)  →  Semantic (purpose)  →  Component (overrides)
--color-blue-600           --color-primary          --button-bg
--space-4                  --spacing-component-md   --card-padding
--text-base                --text-body              --input-font-size
```

- **Primitives**: change to rebrand
- **Semantic**: change to retheme (light/dark)
- **Component**: change to customize one element
- **Format**: OKLCH for colors, CSS custom properties, Tailwind v4 `@theme`

---

## Component Quick Reference

Full specs in `references/component-visual-specs.md`.

| Component | Variants | Sizes | Key Specs |
|-----------|----------|-------|-----------|
| **Button** | default, secondary, outline, ghost, link, destructive | sm(32), md(40), lg(48), icon(40) | 150ms transitions, scale(0.98) on press |
| **Input** | text, textarea, select, checkbox, radio, switch | 40px height | Focus ring, error state with red border |
| **Card** | default, elevated, outline, interactive | — | 24px padding, shadow-sm, radius-lg |
| **Badge** | default, secondary, outline, success, warning, destructive | 22px / 18px | Pill shape, 12px font, 500 weight |
| **Alert** | info, success, warning, destructive | — | Left border 3px, icon + title + description |
| **Dialog** | sm(400), md(500), lg(640), xl(780), full | — | 200ms scale+fade in, overlay 50-70% |
| **Table** | — | compact(32), default(40), comfortable(48) | Sticky header, right-align numbers |

---

## Implementation Checklist (Quick Pass)

Before finalizing any UI — see `references/pre-delivery-checklist.md` for the full audit.

1. [ ] Squint test — is the visual hierarchy clear?
2. [ ] Focus ring — tab through the page, are rings always visible?
3. [ ] Dark mode — switch themes, is everything readable?
4. [ ] Mobile — 375px viewport, any horizontal scroll or broken layout?
5. [ ] Loading — slow the network, do skeletons appear (not blank space)?
6. [ ] Grid — does everything align to 8px?
7. [ ] Tokens — are all colors from semantic tokens (no hardcoded hex)?
8. [ ] States — does every interactive element have hover/focus/disabled?
9. [ ] Density — can power users see enough data without excessive scrolling?
10. [ ] Motion — does `prefers-reduced-motion` work?

---

## DSP Workflow Integration

This skill integrates with the **DSP (Design Shit Properly)** workflow system.

### Detecting Workflow Mode

At the start of any `/dsp:ui` invocation:

1. **Check for `.design/config.json`**
2. **If found** (workflow mode):
   - Load `.design/phases/DISCOVERY.md` for problem context
   - Load `.design/phases/UX-DECISIONS.md` for interaction patterns
   - Load `.design/phases/COLOR-SYSTEM.md` if it exists — use pre-defined color tokens instead of generating from scratch
   - Announce: "Loading context from discovery and UX phases..."
   - If COLOR-SYSTEM.md exists: "Using color tokens from /dsp:color phase"
   - If not: "No color system found — will define colors during UI spec (or run /dsp:color first)"
   - Display: "Components to design: [list from UX-DECISIONS.md]"
   - **Load aesthetic direction** (check in order, first non-null wins):
     1. DISCOVERY.md frontmatter `aesthetic_direction.archetype` field
     2. `.design/config.json` → `phases.ui.aesthetic_direction`
   - If aesthetic direction found: announce "Aesthetic direction: [archetype/feel] — loading archetype reference..." and load `references/aesthetic-archetypes.md` for the matching archetype's principles
   - If no direction found: proceed without archetype constraints (existing behavior)
3. **If not found** (standalone mode):
   - Present aesthetic quick-start:
     > "What would you like me to help design visually?
     >
     > If you'd like a starting direction, pick an aesthetic archetype:
     > 1. **Dark & Dense** — Engineering-precision interfaces (think Linear, Raycast)
     > 2. **Light & Luxurious** — Elegant, light-weight design (think Stripe, Clerk)
     > 3. **Minimal & Stark** — Extreme whitespace, monochrome-forward (think Vercel)
     > 4. **Warm & Approachable** — Friendly, rounded, warm palette (think Airbnb, Notion)
     > 5. **Bold & Expressive** — Saturated colors, strong contrast (think Spotify)
     > 6. **Describe your own** — Tell me the vibe you're going for
     > 7. **Skip** — I'll design from first principles
     >
     > Pick a number or describe what you're building."
   - If archetype selected: load `references/aesthetic-archetypes.md`, extract principles for that archetype, use as design direction (NOT exact tokens)
   - If "describe your own": use user's description as direction, map to closest archetype or blend
   - If "skip": proceed as before with no archetype constraints

### Loading Previous Phase Context

| From Discovery | Used For |
|----------------|----------|
| Problem statement | Design context |
| Primary user | Audience consideration |
| Constraints | Technical/timeline boundaries |
| Aesthetic direction | Visual archetype guidance |

| From UX-DECISIONS | Used For |
|-------------------|----------|
| Component list | Design targets |
| State coverage needed | Required visual states |
| Accessibility constraints | A11y requirements |
| Visual direction hints | Style guidance |

| From COLOR-SYSTEM (if exists) | Used For |
|-------------------------------|----------|
| Color palette & shade ramps | Token values for all color references |
| Contrast matrix | Verified accessible pairings — skip re-checking |
| CSS custom properties | Ready-to-use token block for UI-SPEC.md |
| Dark mode mappings | Theme-aware token values |

### Workflow Mode Output

When UI specification is complete in workflow mode:

1. Write output to `.design/phases/UI-SPEC.md` using the standard template
2. Update `.design/STATE.md` with:
   - Phase status: "completed"
   - Completion timestamp
   - Context summary (visual direction, key components)
3. Ask workflow handoff question:
   > "UI spec complete. Ready for implementation review with /dsp:eng_review?"

### Standalone Mode Output

When no `.design/` directory exists:

1. Output UI specifications inline
2. Ask standard handoff question:
   > "Would you like me to run `/dsp:eng_review` to review the implementation?"

### UI-SPEC.md Output Includes

- Layout & grid system
- Design token definitions (from COLOR-SYSTEM.md if available, otherwise generated inline with `/dsp:color`)
- Visual hierarchy definitions
- Component specifications with Tailwind classes
- Typography scale
- Spacing scale
- Animation & transitions
- Dark mode specifications
- Pre-delivery checklist results
- Handoff notes for review phase

### Config Integration

Respects these settings from `.design/config.json`:
```json
{
  "phases": {
    "ui": {
      "enabled": true,
      "includeB2B": true,
      "includeDataViz": false
    }
  }
}
```

When `includeB2B: true`, automatically apply B2B/enterprise patterns.
When `includeDataViz: true`, include data visualization guidance.

### Aesthetic Direction Integration

When aesthetic direction is available (from discovery, config, or standalone selection):

1. **Load archetype from `references/aesthetic-archetypes.md`**
2. **Extract principles, NOT tokens** — the archetype informs direction, not exact values:
   - Typography approach (weight strategy, letter-spacing philosophy)
   - Color strategy (monochrome-forward, single-accent, warm spectrum, etc.)
   - Depth approach (shadow style, elevation strategy)
   - Radius philosophy (conservative, balanced, friendly)
   - Spacing approach (compact, balanced, generous)
   - Dark mode strategy (if applicable)
3. **Generate custom tokens** using the archetype's principles as constraints
4. **Never copy proprietary fonts** — suggest open alternatives that achieve the same feel
5. **Never copy exact brand colors** — generate a palette that evokes the same character
6. **Respect upstream context** — if discovery or color system established specific constraints (brand colors, required fonts), those override archetype defaults

The aesthetic direction is a compass, not a map. The final design system is always custom.
