# Visual Design Principles

The foundational rules behind polished, professional interfaces. For design tokens and theming, see `design-tokens-and-theming.md`. For color palettes and OKLCH, use `/dsp:color`.

---

## Grid Systems

### The 8px Grid

All spacing, sizing, and positioning should use multiples of 8px:

- **4px** — Fine adjustments only (icon padding, border radius)
- **8px** — Minimum spacing unit
- **16px** — Common element padding
- **24px** — Section spacing
- **32px** — Major section breaks
- **48px, 64px** — Large spacing, hero areas

**Why 8px?** Divisible by 2 and 4, scales well, aligns with common screen sizes.

### Column Grids

```
┌──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┐
│1 │2 │3 │4 │5 │6 │7 │8 │9 │10│11│12│  ← 12-column grid
└──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┘
```

- **12 columns** — Most flexible (divisible by 2, 3, 4, 6)
- **Gutters** — 16px (compact), 24px (standard), 32px (spacious)
- **Margins** — Consistent left/right page margins (16-64px depending on viewport)

### Responsive Breakpoints

```
Mobile:     320px - 767px    (1-4 columns)
Tablet:     768px - 1023px   (6-8 columns)
Desktop:    1024px - 1439px  (12 columns)
Large:      1440px+          (12 columns, max-width container)
```

**Modern approach**: Use container queries for component-level responsiveness, viewport breakpoints for page layout only. See `modern-css-2026.md`.

---

## Visual Hierarchy

### Creating Hierarchy

Establish clear importance through layered signals:

```
┌─────────────────────────────────────┐
│  MOST IMPORTANT                     │  ← Largest, boldest, highest contrast
│                                     │
│  Secondary Information              │  ← Medium size, regular weight
│                                     │
│  Supporting details and metadata    │  ← Smaller, lighter, lower contrast
└─────────────────────────────────────┘
```

| Signal | More Important | Less Important |
|--------|---------------|----------------|
| **Size** | Larger | Smaller |
| **Weight** | Bolder (600-700) | Regular (400) |
| **Contrast** | Higher (fg-default) | Lower (fg-muted) |
| **Position** | Top-left (LTR scans first) | Bottom-right |
| **Whitespace** | More isolation | Denser packing |
| **Color** | Saturated, primary | Desaturated, muted |

**The Squint Test**: Blur your vision — can you still identify what's most important? If not, your hierarchy needs work.

### One Primary Action Per Screen

Each view should have exactly one visually dominant call-to-action. Secondary actions must be visually subordinate (outline, ghost, or text variant). If you have two equal-weight buttons, the user has to think instead of act.

---

## The Gestalt Principles

### Proximity

Items close together are perceived as related. Space is the strongest grouping signal.

```
Good:                          Bad:
┌────────┐ ┌────────┐         ┌────────┐
│ Name   │ │ Email  │         │ Name   │
│ [____] │ │ [____] │         │ [____] │
└────────┘ └────────┘         │        │
                              │ Email  │
┌────────┐ ┌────────┐         │ [____] │
│ City   │ │ Zip    │         │        │
│ [____] │ │ [____] │         │ City   │
└────────┘ └────────┘         └────────┘
```

### Similarity

Similar-looking elements are perceived as related:
- Same color = same category/type
- Same shape = same function
- Same size = same importance

### Continuity

Eyes follow lines, edges, and curves:
- Align elements to create visual flow
- Use consistent left edges for forms
- Horizontal rules guide scanning

### Closure

The mind completes incomplete shapes:
- Progress indicators don't need full circles
- Icons can be simplified/abstracted
- Negative space can define shapes

### Figure/Ground

Clear separation between content (figure) and background (ground):
- Sufficient contrast between layers
- Cards/panels create figure separation
- Elevation (shadows/borders) establishes depth hierarchy

---

## Typography

### Type Scale

Define a systematic scale — never pick arbitrary font sizes:

```
Display:    32-48px  — Hero headlines (use display font)
H1:         24-32px  — Page titles
H2:         20-24px  — Section headers
H3:         16-18px  — Subsection headers
Body:       14-16px  — Primary content
Caption:    12-13px  — Secondary info, metadata
Micro:      10-11px  — Badges, labels (use sparingly)
```

Use `clamp()` for fluid scaling between breakpoints. See `modern-css-2026.md` for fluid typography patterns.

### Weight Hierarchy

- **Bold (600-700)** — Headlines, emphasis, primary labels
- **Medium (500)** — Subheadings, interactive elements, labels
- **Regular (400)** — Body text, secondary content
- **Light (300)** — Avoid for small text; only for large display text

### Display vs Body Fonts

Premium interfaces use a **display** variant for headlines (more expression, tighter tracking) and a regular variant for body text. Example: Inter Display for headings, Inter for body.

### Font Pairing

- **One family** is often enough — use weight/size for variation
- **Two families max** — Display + body (e.g., serif headlines + sans body)
- **Contrast is key** — if pairing, make them clearly different
- **Variable fonts** — single file, continuous weight axis, better performance

### Line Length

- **Optimal**: 50-75 characters per line
- **Minimum**: 45 characters
- **Maximum**: 90 characters
- Too wide = hard to track back to next line
- Too narrow = choppy, awkward breaks

### Line Height

- **Body text**: 1.4-1.6 (1.5 is safe default)
- **Headlines**: 1.1-1.3 (tighter for large text)
- **Dense UI**: 1.3-1.4 (tables, lists)

### Letter Spacing

- **Body**: 0 (default)
- **Headlines**: -0.5 to -2% (slightly tighter)
- **ALL CAPS**: +2 to +5% (more spacing needed)
- **Small text**: +1 to +2% (improves legibility)

### Tabular Numbers

For data columns, prices, and timers, use monospaced figures to prevent layout shift:

```css
.data-value { font-variant-numeric: tabular-nums; }
```

---

## Whitespace & Spacing

- **Whitespace is a design element** — not empty space
- More whitespace around important elements draws attention
- Consistent spacing creates visual rhythm
- **Dense does not mean cluttered** — Bloomberg and Linear show extreme information density while maintaining clarity
- Use spacing tokens from `design-tokens-and-theming.md` to enforce consistency

### Spacing Hierarchy

```
Within component:     4-8px   (icon gaps, input padding)
Between components:   12-16px (card padding, form field gaps)
Between sections:     24-32px (related groups)
Between major areas:  48-64px (page sections)
```

---

## Alignment & Consistency

### The Alignment Principle

**Everything should align to something.**

- Create invisible lines that elements follow
- Left-align most content (for LTR languages)
- Center only for short, focused content (hero sections, empty states)
- Right-align numbers in data columns

### Consistency Rules

- Same spacing for same relationships
- Same size for same importance level
- Same color for same meaning
- Same component for same function
- **If breaking consistency, do it dramatically** (not subtly) — a subtle break looks like a mistake, a dramatic one looks intentional

---

## Motion & Interaction

### Timing Guidelines

| Category | Duration | Use For |
|----------|----------|---------|
| **Instant** | 0-100ms | Hovers, micro-interactions, state toggles |
| **Fast** | 150-200ms | Dropdowns, tooltips, color transitions |
| **Natural** | 200-300ms | Panel open/close, page transitions |
| **Complex** | 300-500ms | Full-page transitions, large movements |

### Easing Functions

| Easing | CSS | Use For |
|--------|-----|---------|
| **Ease-out** (decelerate) | `cubic-bezier(0, 0, 0.2, 1)` | Elements entering view |
| **Ease-in** (accelerate) | `cubic-bezier(0.4, 0, 1, 1)` | Elements exiting view |
| **Ease-in-out** | `cubic-bezier(0.4, 0, 0.2, 1)` | Elements changing state |
| **Spring** | `cubic-bezier(0.34, 1.56, 0.64, 1)` | Playful, bouncy interactions |
| **Linear** | `linear` | Loaders and color changes only |

### Advanced Motion Principles

- **Exit faster than enter** — exit animations should be 60-70% of enter duration. Feels responsive.
- **Spring physics** — prefer spring/physics-based curves over linear for natural feel (Framer Motion, CSS spring())
- **Stagger** — list/grid item entrance: 30-50ms delay per item. Avoid all-at-once or too-slow.
- **Shared element transitions** — use View Transitions API for visual continuity between screens (thumbnail → detail)
- **Interruptible** — animations must be interruptible. User interaction cancels in-progress animation immediately.
- **No blocking** — never block user input during animation. UI must stay interactive.
- **Spatial continuity** — forward navigation animates left/up, backward animates right/down. Keep direction logical.
- **Hierarchy through motion** — translate/scale direction expresses hierarchy: enter from below = deeper, exit upward = back.

### prefers-reduced-motion

Always respect the user's motion preference:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## Elevation & Depth

### Surface Layering

Create depth through layered surfaces, not just shadows:

```
Light mode:                    Dark mode:
Layer 0: white     (page)      Layer 0: gray-950  (page)
Layer 1: gray-50   (surface)   Layer 1: gray-900  (surface)
Layer 2: white     (raised)    Layer 2: gray-800  (raised)
Layer 3: white     (popover)   Layer 3: gray-800  (popover)
```

In dark mode, **higher elevation = lighter surface** (opposite of light mode shadow approach).

### Shadow Scale

Use consistent elevation levels:

| Level | Shadow | Use For |
|-------|--------|---------|
| **0** | none | Flat elements, inline content |
| **1** | `shadow-xs` | Cards, controls, pressed state |
| **2** | `shadow-sm` | Default card elevation |
| **3** | `shadow-md` | Dropdowns, popovers |
| **4** | `shadow-lg` | Sticky headers, floating elements |
| **5** | `shadow-xl` | Modals, dialogs |

### Glassmorphism (Subtle)

Modern glassmorphism is subtle — not the heavy frosted glass of 2021. Use for overlays and floating panels in dark interfaces:

```css
.glass-panel {
  background: oklch(0.2 0.02 250 / 0.6);
  backdrop-filter: blur(16px) saturate(1.2);
  border: 1px solid oklch(1 0 0 / 0.08);
  border-radius: var(--radius-lg);
}
```

Use sparingly. Effective for: modals on dark backgrounds, floating navigation, command palettes. Not for: content cards, data tables, forms.

---

## Premium Polish Techniques

These details separate professional-grade UI from generic output.

### Brand-Tinted Neutrals

Never use pure gray (`oklch(L 0 0)`). Add a trace of your brand hue (chroma 0.005-0.02) to all neutral surfaces. This creates a cohesive, premium feel used by Linear, Stripe, and Vercel. See `design-tokens-and-theming.md` for implementation.

### Physically Modeled Light

Instead of static gradients, model a physical light source:
- Highlights shift as elements change state
- Top edges of cards catch light (lighter border-top)
- Shadows follow a consistent light direction (top-left typically)
- Inner shadows on inputs suggest depth

### Ambient Status Indicators

Replace intrusive alerts with subtle, persistent indicators:
- Thin color bar at top of page for system status
- Dot indicators on nav items for unread/pending
- Background color tint for status (green tint for success state)
- Toast notifications for actions, not status

### Display Font for Headlines

Use a display variant of your body font (e.g., Inter Display) for headings 20px+. Display fonts have:
- Tighter default letter-spacing
- Alternates optimized for large sizes
- More personality than text-optimized variants

---

## Accessibility Visual Standards

### WCAG 2.2 Updates (2024+)

| Criterion | Level | Visual Requirement |
|-----------|-------|--------------------|
| **Target Size** (2.5.8) | AA | Pointer targets >= 24x24 CSS px (44px preferred) |
| **Focus Not Obscured** (2.4.11) | AA | Focused element not entirely hidden by sticky/fixed elements |
| **Focus Appearance** (2.4.13) | AAA | Focus indicator has sufficient contrast and minimum 2px outline |
| **Dragging Movements** (2.5.7) | AA | All drag operations have single-pointer alternatives |

### Contrast Requirements

- **Normal text**: 4.5:1 minimum (WCAG AA)
- **Large text** (18px+ or 14px bold): 3:1 minimum
- **UI components** (borders, icons): 3:1 minimum
- **Disabled elements**: contrast exempted (but still perceivable)

For APCA-based contrast checking and perceptual uniformity, use `/dsp:color`.

### Color Independence

Never use color as the sole indicator:
- Status: color + icon + text label
- Form errors: red border + error icon + error message text
- Charts: color + pattern/texture + direct labels
- Links: color + underline (or other non-color differentiator)
