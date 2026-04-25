# Modern CSS (2026)

Production-ready CSS features that enable premium UI polish without JavaScript. All features listed here have broad browser support (95%+ or all major engines).

---

## Container Queries

**What**: Responsive styles based on *parent container* size, not viewport. Essential for reusable components that live in different layout contexts.

**Browser support**: Baseline 2023. All major browsers.

```css
/* Define a containment context */
.card-grid {
  container-type: inline-size;
  container-name: card-grid;
}

/* Respond to container width */
@container card-grid (min-width: 600px) {
  .card {
    grid-template-columns: 200px 1fr;
  }
}

@container card-grid (min-width: 900px) {
  .card {
    grid-template-columns: 250px 1fr auto;
  }
}
```

**When to use**:
- Cards/widgets that appear in different-width containers (sidebar vs main area)
- Dashboard panels that reflow based on their own space
- Component libraries where components must be context-agnostic

**Tailwind v4**: First-class support, no plugin needed.

```html
<div class="@container">
  <div class="flex flex-col @md:flex-row @lg:grid @lg:grid-cols-3">
    <!-- Responds to container, not viewport -->
  </div>
</div>
```

---

## :has() Selector

**What**: Style a parent based on the state or presence of its children. The "parent selector" CSS never had before.

**Browser support**: Baseline 2023. All major browsers.

```css
/* Style form group when input has an error */
.form-group:has(input:invalid) {
  border-color: var(--color-error);
}

/* Style card differently when it contains an image */
.card:has(img) {
  padding-top: 0;
}

/* Style label when its checkbox is checked */
label:has(input:checked) {
  background: var(--color-primary)/10;
  border-color: var(--color-primary);
}

/* Style nav when search is focused */
nav:has(.search:focus) {
  background: var(--bg-raised);
}
```

**When to use**:
- Form validation styling without JavaScript
- Conditional layouts based on content presence
- State-dependent parent styling (selected items, active filters)

---

## CSS Subgrid

**What**: Child elements inherit their parent's grid tracks, solving the longstanding problem of aligning content across sibling cards.

**Browser support**: 97%+ (Baseline 2023).

```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}

.card {
  display: grid;
  /* Inherit parent's row tracks */
  grid-template-rows: subgrid;
  grid-row: span 3; /* card spans 3 row tracks: header, body, footer */
}
```

**Before subgrid**:
```
┌──────────┐ ┌──────────┐ ┌──────────┐
│ Short    │ │ Long     │ │ Medium   │
│ Title    │ │ Title    │ │ Title    │
│          │ │ Here     │ │          │
├──────────┤ │          │ ├──────────┤  ← misaligned
│ Body     │ ├──────────┤ │ Body     │
└──────────┘ │ Body     │ └──────────┘
             └──────────┘
```

**With subgrid**:
```
┌──────────┐ ┌──────────┐ ┌──────────┐
│ Short    │ │ Long     │ │ Medium   │
│ Title    │ │ Title    │ │ Title    │
│          │ │ Here     │ │          │
├──────────┤ ├──────────┤ ├──────────┤  ← aligned!
│ Body     │ │ Body     │ │ Body     │
├──────────┤ ├──────────┤ ├──────────┤
│ Footer   │ │ Footer   │ │ Footer   │
└──────────┘ └──────────┘ └──────────┘
```

**When to use**: Any grid of cards where headers, content, and footers must align across columns.

---

## Scroll-Driven Animations

**What**: Animate elements based on scroll position — zero JavaScript. Replaces JS-heavy parallax, progress bars, and reveal animations.

**Browser support**: Chrome 115+, Edge 115+, Safari 26+. Use `@supports` for progressive enhancement.

```css
/* Progress bar that fills as user scrolls */
.reading-progress {
  animation: fill-bar linear;
  animation-timeline: scroll();
}

@keyframes fill-bar {
  from { width: 0%; }
  to   { width: 100%; }
}

/* Element fades in when it enters the viewport */
.reveal {
  animation: fade-in linear;
  animation-timeline: view();
  animation-range: entry 0% entry 100%;
}

@keyframes fade-in {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}
```

**When to use**:
- Reading progress indicators
- Scroll-triggered reveal animations
- Parallax backgrounds
- Sticky element transitions

**Important**: Always respect `prefers-reduced-motion`:
```css
@media (prefers-reduced-motion: reduce) {
  .reveal { animation: none; opacity: 1; }
}
```

---

## View Transitions API

**What**: Smooth animated transitions between page states or navigations. Works for both SPA state changes and multi-page navigations.

**Browser support**: Same-document (Baseline 2024). Cross-document expanding in 2026.

### Same-Document Transitions (SPA)

```javascript
document.startViewTransition(() => {
  // Update the DOM
  updateContent();
});
```

```css
/* Default crossfade */
::view-transition-old(root),
::view-transition-new(root) {
  animation-duration: 200ms;
}

/* Named transitions for specific elements */
.hero-image {
  view-transition-name: hero;
}

::view-transition-old(hero) {
  animation: slide-out 200ms ease-in;
}

::view-transition-new(hero) {
  animation: slide-in 200ms ease-out;
}
```

### Cross-Document Transitions (MPA)

```css
/* Enable in both pages */
@view-transition {
  navigation: auto;
}

/* Match elements across pages by name */
.product-image {
  view-transition-name: product-hero;
}
```

**When to use**:
- Page-to-page navigation (list → detail)
- Tab/panel switching
- Modal open/close
- Shared element transitions (thumbnail → full image)
- Route changes in SPAs

---

## Anchor Positioning

**What**: Position elements relative to other elements purely in CSS. Eliminates Popper.js, Floating UI, and similar JS libraries for tooltips, popovers, and dropdowns.

**Browser support**: Chrome 127+, Safari 26.1+, Firefox 149+ (Baseline 2026).

```css
/* The anchor element */
.trigger {
  anchor-name: --my-trigger;
}

/* The positioned element */
.tooltip {
  position: fixed;
  position-anchor: --my-trigger;

  /* Position below the trigger, centered */
  top: anchor(bottom);
  left: anchor(center);
  translate: -50% 8px;

  /* Flip to top if no space below */
  position-try-fallbacks: flip-block;
}
```

**When to use**:
- Tooltips
- Dropdown menus
- Popovers
- Chart data labels
- Footnotes and annotations

---

## @starting-style

**What**: Define the initial style for elements that are being inserted or shown, enabling clean enter transitions without FOUC.

**Browser support**: Chrome 117+, Safari 17.5+, Firefox 129+.

```css
/* Dialog with smooth enter transition */
dialog[open] {
  opacity: 1;
  transform: scale(1);
  transition: opacity 200ms ease-out, transform 200ms ease-out;

  @starting-style {
    opacity: 0;
    transform: scale(0.95);
  }
}

/* Popover entrance */
[popover]:popover-open {
  opacity: 1;
  transform: translateY(0);
  transition: opacity 150ms, transform 150ms;

  @starting-style {
    opacity: 0;
    transform: translateY(-8px);
  }
}
```

**When to use**: Any element that transitions from `display: none` to visible (dialogs, popovers, dynamically inserted content).

---

## @scope

**What**: Scope styles to a DOM subtree without BEM naming conventions or CSS modules.

**Browser support**: Chrome 118+, Safari 17.4+, Firefox (in progress).

```css
@scope (.card) {
  .title { font-weight: 600; }
  .body  { color: var(--fg-secondary); }

  /* Donut scope — excludes nested cards */
  @scope (.card) to (.card) {
    .title { font-size: var(--text-lg); }
  }
}
```

**When to use**: Component-scoped styles without CSS-in-JS overhead. Particularly useful for nested component hierarchies.

---

## Popover API

**What**: Native browser popover behavior — light-dismiss, top-layer rendering, focus management — without JavaScript.

**Browser support**: Baseline 2024. All major browsers.

```html
<button popovertarget="my-popover">Open</button>

<div id="my-popover" popover>
  <p>Popover content</p>
</div>
```

```css
[popover] {
  padding: 16px;
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-lg);
  border: 1px solid var(--border-default);

  /* Entrance animation via @starting-style */
  opacity: 1;
  transition: opacity 150ms;

  @starting-style {
    opacity: 0;
  }
}

/* Style the backdrop */
[popover]::backdrop {
  background: oklch(0 0 0 / 0.1);
}
```

**When to use**: Tooltips, menus, and non-modal overlays that should close when clicking outside.

---

## Fluid Typography with clamp()

**What**: Font sizes that scale smoothly between viewport sizes without media query breakpoints.

```css
/* Formula: clamp(min, preferred, max) */
/* preferred = min + (max - min) * viewport-based value */

:root {
  --text-base: clamp(0.875rem, 0.83rem + 0.2vw, 1rem);      /* 14-16px */
  --text-lg:   clamp(1.125rem, 1rem + 0.4vw, 1.25rem);       /* 18-20px */
  --text-xl:   clamp(1.25rem, 1.1rem + 0.6vw, 1.5rem);       /* 20-24px */
  --text-2xl:  clamp(1.5rem, 1.2rem + 1vw, 2rem);             /* 24-32px */
  --text-3xl:  clamp(1.875rem, 1.5rem + 1.5vw, 2.5rem);       /* 30-40px */
  --text-4xl:  clamp(2.25rem, 1.7rem + 2.2vw, 3rem);           /* 36-48px */
}
```

**Important for accessibility**:
- Never set a fixed base `font-size` on `html` — let the browser default (usually 16px) be authoritative
- Test with browser zoom at 200% — text must still be readable (WCAG 1.4.4)
- Don't use `vw` alone (won't scale with zoom) — always combine with `rem`

### Variable Fonts

Single font file with continuous weight/width axes. Better performance + smoother design.

```css
@font-face {
  font-family: 'Inter';
  src: url('/fonts/Inter-Variable.woff2') format('woff2');
  font-weight: 100 900;  /* full weight range */
  font-display: swap;
}

/* Use any weight, not just 400/700 */
.heading   { font-weight: 650; }
.body      { font-weight: 400; }
.caption   { font-weight: 450; }
.emphasis  { font-weight: 550; }

/* Optical size axis — auto-adjusts for text size */
.small-text { font-variation-settings: 'opsz' 12; }
.display    { font-variation-settings: 'opsz' 48; }
```

---

## Bento Grid Layouts

Modular, asymmetric card grids for information-dense pages. Named after Japanese bento boxes.

```css
.bento {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(180px, auto);
  gap: 16px;
}

/* Feature card — spans 2 columns and 2 rows */
.bento-feature {
  grid-column: span 2;
  grid-row: span 2;
}

/* Wide card */
.bento-wide {
  grid-column: span 2;
}

/* Tall card */
.bento-tall {
  grid-row: span 2;
}
```

```
┌────────────────────┬──────────┬──────────┐
│                    │          │          │
│  Feature (2×2)     │  Card    │  Card    │
│                    │          │          │
│                    ├──────────┴──────────┤
│                    │                     │
├──────────┬─────────┤    Wide (2×1)       │
│  Card    │  Card   │                     │
└──────────┴─────────┴─────────────────────┘
```

**Responsive**: Collapse to 2 columns on tablet, 1 column on mobile.

**When to use**: Feature pages, dashboards, marketing landing pages, portfolio grids.

---

## Performance CSS

### content-visibility

Defer rendering of off-screen content for faster initial paint:

```css
.section {
  content-visibility: auto;
  contain-intrinsic-size: auto 500px; /* estimated height */
}
```

### CSS contain

Tell the browser what will NOT affect surrounding layout:

```css
.card {
  contain: layout style; /* card changes don't reflow siblings */
}

.virtualized-row {
  contain: strict; /* full containment for virtualized lists */
}
```

### will-change (use sparingly)

```css
/* Only add before animation starts, remove after */
.about-to-animate {
  will-change: transform, opacity;
}

/* NEVER do this globally */
/* * { will-change: transform; }  ← kills performance */
```

---

## Cascade Layers

Control specificity ordering declaratively:

```css
@layer base, tokens, components, utilities;

@layer base {
  /* Reset, typography defaults */
}

@layer tokens {
  /* Design token definitions */
}

@layer components {
  /* Component styles — always beat tokens */
}

@layer utilities {
  /* Utility overrides — always win */
}
```

**Why it matters**: Eliminates specificity wars. A utility in `@layer utilities` always beats a component in `@layer components`, regardless of selector specificity. Tailwind v4 uses this natively.
