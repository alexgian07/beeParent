# Component Visual Specifications

Measurement specs for core UI components — dimensions, tokens, states, and transitions. These are *visual design specs*, not code review criteria (that's `/dsp:eng_review`).

---

## Universal State System

### State Priority Hierarchy

When multiple states apply, higher priority wins:

```
disabled > loading > active > focus > hover > default
```

### Transition Defaults

All interactive elements share these transition specs:

| Property | Duration | Easing | Use For |
|----------|----------|--------|---------|
| `color`, `background`, `border-color`, `box-shadow` | 150ms | ease-in-out | Most state changes |
| `transform` | 200ms | ease-out | Scale, translate |
| `opacity` | 150ms | ease | Fade in/out |

```css
/* Standard transition shorthand */
transition: color 150ms ease-in-out,
            background-color 150ms ease-in-out,
            border-color 150ms ease-in-out,
            box-shadow 150ms ease-in-out;
```

### Focus Ring Standard

All focusable elements must show a visible focus indicator:

```css
/* Standard focus ring */
outline: 2px solid var(--ring-color);
outline-offset: 2px;

/* Tailwind */
focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2
```

- Width: **2px** (minimum, 3px preferred for high visibility)
- Offset: **2px** from element edge
- Color: `var(--ring-color)` (primary blue by default)
- Only on `:focus-visible`, not `:focus` (avoids showing on click)

### Disabled State

```
opacity: 0.5
pointer-events: none
cursor: not-allowed
```

- Reduce opacity to 50% (0.38–0.5 acceptable range)
- Must include `aria-disabled="true"` attribute
- Text contrast may drop below 4.5:1 — this is acceptable per WCAG

### Loading State Patterns

| Context | Pattern | Overlay |
|---------|---------|---------|
| Button | Spinner replaces label text, button stays same width | None |
| Input | Spinner in trailing position | None |
| Card | Skeleton shimmer or spinner overlay | 70% opacity bg |
| Page section | Skeleton screens | None |
| Full page | Centered spinner or progress bar | Full viewport |

Loading spinner size: match the context's text size (14px text → 14px spinner).

---

## Button

### Variants

| Variant | Background | Text | Border | Use For |
|---------|-----------|------|--------|---------|
| **default** | `var(--color-primary)` | `var(--color-primary-fg)` | none | Primary actions |
| **secondary** | `var(--color-secondary)` | `var(--color-secondary-fg)` | none | Secondary actions |
| **outline** | transparent | `var(--fg-default)` | `var(--border-default)` | Tertiary actions |
| **ghost** | transparent | `var(--fg-default)` | none | In toolbars, low-emphasis |
| **link** | transparent | `var(--color-primary)` | none | Inline navigation |
| **destructive** | `var(--color-destructive)` | `var(--color-destructive-fg)` | none | Delete, remove |

### Sizes

| Size | Height | Padding X | Font Size | Icon Size | Border Radius |
|------|--------|-----------|-----------|-----------|---------------|
| **sm** | 32px | 12px | 13px (text-sm) | 14px | var(--radius-md) |
| **default** | 40px | 16px | 14px (text-sm) | 16px | var(--radius-md) |
| **lg** | 48px | 24px | 16px (text-base) | 18px | var(--radius-md) |
| **icon** | 40px | 0 (square) | — | 18px | var(--radius-md) |

### States (default variant)

| State | Background | Shadow | Transform | Other |
|-------|-----------|--------|-----------|-------|
| **default** | `primary` | none | none | — |
| **hover** | `primary-hover` | none | none | cursor: pointer |
| **active** | `primary-active` | none | `scale(0.98)` | — |
| **focus** | `primary` | focus ring | none | 2px ring + 2px offset |
| **disabled** | `primary` | none | none | opacity: 0.5 |
| **loading** | `primary` | none | none | spinner replaces text |

### Icon Placement

```
Leading icon:          Trailing icon:
┌─────────────────┐    ┌─────────────────┐
│ [→] Label       │    │ Label       [→] │
└─────────────────┘    └─────────────────┘
Gap: 8px                Gap: 8px
```

---

## Input

### Variants

| Type | Height | Description |
|------|--------|-------------|
| **text** | 40px | Single-line text input |
| **textarea** | min 80px | Multi-line, resizable vertically |
| **select** | 40px | Dropdown with chevron indicator |
| **checkbox** | 20x20px | Square check toggle |
| **radio** | 20x20px | Circle option selector |
| **switch** | 24x44px | Boolean toggle |

### Text Input Anatomy

```
┌─ Label ──────────────────────────────────┐
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │ [icon] Placeholder text       [icon]│  │  ← 40px height
│  └─────────────────────────────────────┘  │
│  Helper text or error message             │
└───────────────────────────────────────────┘

Padding: 12px horizontal, centered vertical
Label to input gap: 6px
Input to helper gap: 4px
```

### Input States

| State | Border | Background | Label | Ring |
|-------|--------|-----------|-------|------|
| **default** | `var(--input-border)` | `var(--input-bg)` | `var(--fg-secondary)` | none |
| **hover** | `var(--border-hover)` | `var(--input-bg)` | `var(--fg-secondary)` | none |
| **focus** | `var(--border-focus)` | `var(--input-bg)` | `var(--color-primary)` | 2px ring |
| **error** | `var(--border-error)` | `var(--input-bg)` | `var(--color-error)` | 2px red ring |
| **disabled** | `var(--border-default)` | `var(--color-muted)` | `var(--fg-muted)` | none |

- Error messages: red text below input, linked via `aria-describedby`
- Required indicator: asterisk (*) after label
- Helper text: muted color below input

---

## Card

### Anatomy

```
┌─────────────────────────────────────────┐
│ ┌─── Header ──────────────────────────┐ │  padding: 24px 24px 0
│ │ Title                    [Action ▼] │ │
│ │ Description                         │ │
│ └─────────────────────────────────────┘ │
│ ┌─── Content ─────────────────────────┐ │  padding: 24px
│ │                                     │ │
│ │ Main content area                   │ │
│ │                                     │ │
│ └─────────────────────────────────────┘ │
│ ┌─── Footer ──────────────────────────┐ │  padding: 0 24px 24px
│ │ [Secondary]            [Primary]    │ │
│ └─────────────────────────────────────┘ │
└─────────────────────────────────────────┘
```

### Card Variants

| Variant | Border | Shadow | Background | Use For |
|---------|--------|--------|-----------|---------|
| **default** | `var(--border-default)` | `var(--shadow-sm)` | `var(--bg-raised)` | Standard content |
| **elevated** | none | `var(--shadow-md)` | `var(--bg-raised)` | Featured content |
| **outline** | `var(--border-default)` | none | transparent | Subtle grouping |
| **interactive** | `var(--border-default)` | `var(--shadow-sm)` | `var(--bg-raised)` | Clickable cards |

### Interactive Card States

| State | Shadow | Border | Transform |
|-------|--------|--------|-----------|
| **default** | `shadow-sm` | `border-default` | none |
| **hover** | `shadow-md` | `border-hover` | `translateY(-1px)` |
| **active** | `shadow-xs` | `border-default` | none |
| **focus** | `shadow-sm` | focus ring | none |

---

## Badge

### Variants

| Variant | Background | Text | Use For |
|---------|-----------|------|---------|
| **default** | `var(--color-primary)` | `var(--color-primary-fg)` | Counts, labels |
| **secondary** | `var(--color-secondary)` | `var(--color-secondary-fg)` | Low emphasis |
| **outline** | transparent | `var(--fg-default)` | Tags, categories |
| **success** | `var(--color-success-bg)` | `var(--color-success-fg)` | Active, complete |
| **warning** | `var(--color-warning-bg)` | `var(--color-warning-fg)` | Attention needed |
| **destructive** | `var(--color-error-bg)` | `var(--color-error-fg)` | Errors, critical |

### Size

- Height: 22px (default), 18px (small)
- Padding: 8px horizontal
- Font size: 12px, font-weight: 500
- Border radius: `var(--radius-full)` (pill shape)

---

## Alert

### Variants

| Variant | Icon | Border-left | Background | Text |
|---------|------|-------------|-----------|------|
| **info** | `InfoIcon` | `var(--color-info)` | `var(--color-info-bg)` | `var(--color-info-fg)` |
| **success** | `CheckCircle` | `var(--color-success)` | `var(--color-success-bg)` | `var(--color-success-fg)` |
| **warning** | `AlertTriangle` | `var(--color-warning)` | `var(--color-warning-bg)` | `var(--color-warning-fg)` |
| **destructive** | `AlertCircle` | `var(--color-error)` | `var(--color-error-bg)` | `var(--color-error-fg)` |

### Anatomy

```
┌───────────────────────────────────────────┐
│ ▌ [icon] Title                        [×] │
│ ▌        Description text                 │
│ ▌        [Action link]                    │
└───────────────────────────────────────────┘

Left border: 3px solid
Padding: 16px
Icon to text gap: 12px
Title to description gap: 4px
```

- Closeable variant: X button top-right, `aria-label="Dismiss"`
- Role: `role="alert"` for critical, `role="status"` for informational

---

## Dialog / Modal

### Sizes

| Size | Width | Use For |
|------|-------|---------|
| **sm** | 400px | Confirmations, simple forms |
| **default** | 500px | Standard dialogs |
| **lg** | 640px | Complex forms, previews |
| **xl** | 780px | Multi-step wizards |
| **full** | calc(100vw - 48px) | Immersive content |

### Anatomy

```
┌─ Overlay (bg-overlay) ─────────────────────────────┐
│                                                     │
│   ┌─ Dialog ──────────────────────────────────┐     │
│   │ ┌─ Header ─────────────────────────────┐  │     │
│   │ │ Title                            [×] │  │     │
│   │ │ Description                          │  │     │
│   │ └─────────────────────────────────────┘  │     │
│   │ ┌─ Content ────────────────────────────┐  │     │
│   │ │                                      │  │     │
│   │ │  Scrollable content area             │  │     │
│   │ │                                      │  │     │
│   │ └──────────────────────────────────────┘  │     │
│   │ ┌─ Footer ─────────────────────────────┐  │     │
│   │ │ [Cancel]                    [Confirm] │  │     │
│   │ └──────────────────────────────────────┘  │     │
│   └───────────────────────────────────────────┘     │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### Specs

- Border radius: `var(--radius-xl)` (12px)
- Padding: 24px
- Overlay: `var(--bg-overlay)` — 50% black (light), 70% black (dark)
- Header/footer border: `var(--border-default)` (optional, use for scrollable content)
- Max height: `calc(100vh - 96px)`, content scrolls

### Enter/Exit Animations

```css
/* Enter */
animation: dialog-in 200ms ease-out;

@keyframes dialog-in {
  from { opacity: 0; transform: scale(0.95) translateY(8px); }
  to   { opacity: 1; transform: scale(1) translateY(0); }
}

/* Exit — faster than enter */
animation: dialog-out 150ms ease-in;

@keyframes dialog-out {
  from { opacity: 1; transform: scale(1); }
  to   { opacity: 0; transform: scale(0.95); }
}
```

### Interaction

- Close on: Escape key, overlay click, X button
- Focus: trap inside dialog, return focus to trigger on close
- Prevent body scroll when open

---

## Table

### Row States

| State | Background | Border | Other |
|-------|-----------|--------|-------|
| **default** | transparent | bottom `border-default` | — |
| **hover** | `var(--color-muted)` | bottom `border-default` | cursor: pointer (if clickable) |
| **selected** | `var(--color-primary)/10` | bottom `border-default` | checkbox checked |
| **striped** (alt) | `var(--bg-surface)` | bottom `border-default` | every other row |

### Cell Alignment

| Content Type | Alignment | Example |
|-------------|-----------|---------|
| Text | left | Names, descriptions |
| Numbers | right | Prices, counts, percentages |
| Currency | right | $1,234.56 |
| Dates | left | Jan 15, 2026 |
| Status | left | Badge component |
| Actions | right | Icon buttons, menu |
| Checkbox | center | Selection column |

### Density Options

| Density | Row Height | Cell Padding | Font Size | Use For |
|---------|-----------|-------------|-----------|---------|
| **compact** | 32px | 4px 12px | 13px | Power users, dense data |
| **default** | 40px | 8px 16px | 14px | General use |
| **comfortable** | 48px | 12px 16px | 14px | Scanning, touch devices |

### Table Header

```
┌─────┬──────────────────┬────────────┬──────────┐
│ ☐   │ Name ↕           │ Status     │ Value ↕  │
├─────┼──────────────────┼────────────┼──────────┤
```

- Background: `var(--bg-surface)`
- Font: `var(--font-medium)`, `var(--text-sm)`, `var(--fg-secondary)`
- Sort indicator: ↑↓ arrows, active sort in `var(--fg-default)`
- Sticky header: `position: sticky; top: 0; z-index: var(--z-sticky)`
- Column resize: drag handle between columns (cursor: col-resize)

### Number Formatting in Tables

- Right-align all numeric columns
- Use tabular/monospaced figures: `font-variant-numeric: tabular-nums`
- Thousands separator: 1,234,567
- Consistent decimal places within a column
- Currency: symbol in header if uniform, per-cell if mixed
- Abbreviate large numbers: 1.2M, 45K (with tooltip for exact value)

---

## Skeleton Loading

Use skeletons instead of spinners for async content (perceived 50% faster loading).

### Skeleton Pattern

```
┌───────────────────────────────────────┐
│ ████████████████░░░░░░  ← Title      │
│ ██████████████████████████████░░░░░  │  ← Description line 1
│ █████████████████████░░░░░░░░░░░░░  │  ← Description line 2
│                                       │
│ ┌────────┐ ┌────────┐ ┌────────┐    │
│ │████████│ │████████│ │████████│    │  ← Cards
│ │████████│ │████████│ │████████│    │
│ └────────┘ └────────┘ └────────┘    │
└───────────────────────────────────────┘
```

### Rules

- Match the layout of the actual content (same dimensions)
- Use shimmer/pulse animation to indicate activity
- Fade transition from skeleton → real content (150ms)
- Show above-fold skeletons first (progressive)
- Only show skeleton when load time > 500ms (avoid flash for fast loads)
- Background: `var(--color-muted)` with animated lighter sweep

```css
@keyframes shimmer {
  0%   { background-position: -200% 0; }
  100% { background-position: 200% 0; }
}

.skeleton {
  background: linear-gradient(
    90deg,
    var(--color-muted) 25%,
    var(--color-muted-fg)/10 50%,
    var(--color-muted) 75%
  );
  background-size: 200% 100%;
  animation: shimmer 1.5s ease-in-out infinite;
  border-radius: var(--radius-sm);
}
```
