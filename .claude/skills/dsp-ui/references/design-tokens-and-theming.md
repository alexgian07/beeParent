# Design Tokens & Theming

The foundation layer for consistent, themeable UI. Defines the token architecture, semantic naming, dark mode system, and Tailwind v4 integration.

> **Color values**: For generating actual OKLCH palettes, shade ramps, and contrast checking, use `/dsp:color`. This file defines the *architecture* — how tokens are named, layered, and consumed.

---

## Three-Layer Token Architecture

```
Primitive Tokens (raw values)
       ↓ referenced by
Semantic Tokens (purpose aliases)
       ↓ referenced by
Component Tokens (component-specific overrides)
```

### Why Three Layers?

- **Primitives** are the palette — change them to rebrand
- **Semantic** tokens map intent — change them to retheme (light/dark)
- **Component** tokens override per-component — change them to customize one element

### Naming Convention

```
--{category}-{item}-{variant}-{state}

Examples:
--color-blue-600          (primitive)
--color-primary            (semantic)
--color-primary-hover      (semantic + state)
--button-bg                (component)
--button-bg-hover          (component + state)
```

---

## Primitive Tokens

Raw design values with no semantic meaning. The full palette.

### Color Primitives

Use OKLCH for perceptual uniformity. Generate values with `/dsp:color`.

```css
/* Structure — actual values from /dsp:color */
--color-blue-50:  oklch(0.97 0.01 250);
--color-blue-100: oklch(0.93 0.03 250);
--color-blue-200: oklch(0.86 0.06 250);
--color-blue-300: oklch(0.76 0.10 250);
--color-blue-400: oklch(0.66 0.15 250);
--color-blue-500: oklch(0.55 0.20 250);
--color-blue-600: oklch(0.48 0.22 250);
--color-blue-700: oklch(0.40 0.19 250);
--color-blue-800: oklch(0.33 0.15 250);
--color-blue-900: oklch(0.27 0.11 250);
--color-blue-950: oklch(0.20 0.08 250);
```

### Spacing Primitives

Based on 4px/8px grid system:

```css
--space-0:   0px;
--space-px:  1px;
--space-0.5: 2px;
--space-1:   4px;
--space-1.5: 6px;
--space-2:   8px;
--space-3:   12px;
--space-4:   16px;
--space-5:   20px;
--space-6:   24px;
--space-8:   32px;
--space-10:  40px;
--space-12:  48px;
--space-16:  64px;
--space-20:  80px;
--space-24:  96px;
```

### Typography Primitives

```css
/* Font families */
--font-sans:    'Inter', system-ui, sans-serif;
--font-display: 'Inter Display', 'Inter', system-ui, sans-serif;
--font-mono:    'JetBrains Mono', ui-monospace, monospace;

/* Font sizes — use fluid clamp() (see modern-css-2026.md) */
--text-xs:   clamp(0.6875rem, 0.65rem + 0.15vw, 0.75rem);    /* 11-12px */
--text-sm:   clamp(0.8125rem, 0.78rem + 0.15vw, 0.875rem);   /* 13-14px */
--text-base: clamp(0.875rem, 0.83rem + 0.2vw, 1rem);          /* 14-16px */
--text-lg:   clamp(1rem, 0.95rem + 0.25vw, 1.125rem);         /* 16-18px */
--text-xl:   clamp(1.125rem, 1.05rem + 0.35vw, 1.25rem);      /* 18-20px */
--text-2xl:  clamp(1.375rem, 1.25rem + 0.5vw, 1.5rem);        /* 22-24px */
--text-3xl:  clamp(1.75rem, 1.55rem + 0.8vw, 1.875rem);       /* 28-30px */
--text-4xl:  clamp(2rem, 1.7rem + 1.2vw, 2.25rem);            /* 32-36px */

/* Font weights */
--font-light:    300;
--font-regular:  400;
--font-medium:   500;
--font-semibold: 600;
--font-bold:     700;

/* Line heights */
--leading-none:    1;
--leading-tight:   1.25;
--leading-snug:    1.375;
--leading-normal:  1.5;
--leading-relaxed: 1.625;

/* Letter spacing */
--tracking-tighter: -0.05em;
--tracking-tight:   -0.025em;
--tracking-normal:  0em;
--tracking-wide:    0.025em;
--tracking-wider:   0.05em;
```

### Border Radius Primitives

```css
--radius-none: 0px;
--radius-sm:   4px;
--radius-md:   6px;
--radius-lg:   8px;
--radius-xl:   12px;
--radius-2xl:  16px;
--radius-full: 9999px;
```

### Shadow Primitives

```css
--shadow-xs:  0 1px 2px 0 oklch(0 0 0 / 0.05);
--shadow-sm:  0 1px 3px 0 oklch(0 0 0 / 0.1), 0 1px 2px -1px oklch(0 0 0 / 0.1);
--shadow-md:  0 4px 6px -1px oklch(0 0 0 / 0.1), 0 2px 4px -2px oklch(0 0 0 / 0.1);
--shadow-lg:  0 10px 15px -3px oklch(0 0 0 / 0.1), 0 4px 6px -4px oklch(0 0 0 / 0.1);
--shadow-xl:  0 20px 25px -5px oklch(0 0 0 / 0.1), 0 8px 10px -6px oklch(0 0 0 / 0.1);
--shadow-2xl: 0 25px 50px -12px oklch(0 0 0 / 0.25);
```

### Z-Index Scale

```css
--z-base:     0;
--z-raised:   10;
--z-dropdown: 20;
--z-sticky:   30;
--z-overlay:  40;
--z-modal:    50;
--z-popover:  60;
--z-toast:    70;
--z-tooltip:  80;
--z-max:      9999;
```

---

## Semantic Tokens

Map primitives to design intent. This is the layer that changes between light/dark modes.

### Light Mode (Default)

```css
:root {
  /* Backgrounds */
  --bg-page:       var(--color-white);
  --bg-surface:    var(--color-gray-50);
  --bg-raised:     var(--color-white);
  --bg-popover:    var(--color-white);
  --bg-overlay:    oklch(0 0 0 / 0.5);

  /* Foregrounds */
  --fg-default:    var(--color-gray-950);
  --fg-secondary:  var(--color-gray-600);
  --fg-muted:      var(--color-gray-400);
  --fg-inverse:    var(--color-white);

  /* Primary action */
  --color-primary:          var(--color-blue-600);
  --color-primary-hover:    var(--color-blue-700);
  --color-primary-active:   var(--color-blue-800);
  --color-primary-fg:       var(--color-white);

  /* Secondary action */
  --color-secondary:        var(--color-gray-100);
  --color-secondary-hover:  var(--color-gray-200);
  --color-secondary-active: var(--color-gray-300);
  --color-secondary-fg:     var(--color-gray-900);

  /* Destructive */
  --color-destructive:        var(--color-red-600);
  --color-destructive-hover:  var(--color-red-700);
  --color-destructive-active: var(--color-red-800);
  --color-destructive-fg:     var(--color-white);

  /* Muted */
  --color-muted:     var(--color-gray-100);
  --color-muted-fg:  var(--color-gray-500);

  /* Status */
  --color-success:     var(--color-green-600);
  --color-success-bg:  var(--color-green-50);
  --color-success-fg:  var(--color-green-700);
  --color-warning:     var(--color-amber-500);
  --color-warning-bg:  var(--color-amber-50);
  --color-warning-fg:  var(--color-amber-700);
  --color-error:       var(--color-red-600);
  --color-error-bg:    var(--color-red-50);
  --color-error-fg:    var(--color-red-700);
  --color-info:        var(--color-blue-500);
  --color-info-bg:     var(--color-blue-50);
  --color-info-fg:     var(--color-blue-700);

  /* Borders & inputs */
  --border-default:  var(--color-gray-200);
  --border-hover:    var(--color-gray-300);
  --border-focus:    var(--color-blue-500);
  --border-error:    var(--color-red-500);
  --input-bg:        var(--color-white);
  --input-border:    var(--color-gray-300);

  /* Focus ring */
  --ring-color:   var(--color-blue-500);
  --ring-width:   2px;
  --ring-offset:  2px;

  /* Interaction */
  --disabled-opacity: 0.5;
}
```

### Dark Mode

Design dark mode first, then derive light. Override semantic tokens only — primitives and components stay the same.

```css
.dark {
  /* Backgrounds — lighter surfaces = higher elevation */
  --bg-page:       var(--color-gray-950);
  --bg-surface:    var(--color-gray-900);
  --bg-raised:     var(--color-gray-800);
  --bg-popover:    var(--color-gray-800);
  --bg-overlay:    oklch(0 0 0 / 0.7);

  /* Foregrounds */
  --fg-default:    var(--color-gray-50);
  --fg-secondary:  var(--color-gray-400);
  --fg-muted:      var(--color-gray-500);
  --fg-inverse:    var(--color-gray-950);

  /* Primary — desaturate slightly for dark backgrounds */
  --color-primary:          var(--color-blue-500);
  --color-primary-hover:    var(--color-blue-400);
  --color-primary-active:   var(--color-blue-300);
  --color-primary-fg:       var(--color-white);

  /* Secondary */
  --color-secondary:        var(--color-gray-800);
  --color-secondary-hover:  var(--color-gray-700);
  --color-secondary-active: var(--color-gray-600);
  --color-secondary-fg:     var(--color-gray-100);

  /* Destructive */
  --color-destructive:        var(--color-red-500);
  --color-destructive-hover:  var(--color-red-400);
  --color-destructive-active: var(--color-red-300);
  --color-destructive-fg:     var(--color-white);

  /* Muted */
  --color-muted:     var(--color-gray-800);
  --color-muted-fg:  var(--color-gray-400);

  /* Status — use darker backgrounds, lighter foregrounds */
  --color-success-bg:  var(--color-green-950);
  --color-success-fg:  var(--color-green-400);
  --color-warning-bg:  var(--color-amber-950);
  --color-warning-fg:  var(--color-amber-400);
  --color-error-bg:    var(--color-red-950);
  --color-error-fg:    var(--color-red-400);
  --color-info-bg:     var(--color-blue-950);
  --color-info-fg:     var(--color-blue-400);

  /* Borders — use borders instead of shadows for depth */
  --border-default:  var(--color-gray-800);
  --border-hover:    var(--color-gray-700);
  --input-bg:        var(--color-gray-900);
  --input-border:    var(--color-gray-700);
}
```

### Dark Mode Design Rules

1. **Don't invert** — redesign surfaces. Elevation = lighter, not darker
2. **Reduce contrast slightly** — avoid pure white (#fff) on pure black (#000). Use gray-50 on gray-950
3. **Desaturate colors** — shift primary 1-2 stops lighter (blue-600 → blue-500)
4. **Replace shadows with borders** — shadows are invisible on dark backgrounds
5. **Test independently** — don't assume light mode contrast ratios transfer
6. **Scrim opacity** — increase to 60-70% black (vs 50% in light mode)

---

## Spacing Semantics

Map spacing primitives to usage intent:

```css
:root {
  /* Component internal spacing */
  --spacing-component-xs:  var(--space-1);    /* 4px — tight icon gaps */
  --spacing-component-sm:  var(--space-2);    /* 8px — compact padding */
  --spacing-component-md:  var(--space-3);    /* 12px — default padding */
  --spacing-component-lg:  var(--space-4);    /* 16px — spacious padding */

  /* Section spacing */
  --spacing-section-sm:    var(--space-6);    /* 24px — related sections */
  --spacing-section-md:    var(--space-8);    /* 32px — distinct sections */
  --spacing-section-lg:    var(--space-12);   /* 48px — major page breaks */

  /* Page margins */
  --spacing-page-x:        var(--space-4);    /* 16px mobile, scale up */
  --spacing-page-y:        var(--space-6);    /* 24px */
}
```

---

## Tailwind v4 Integration

Tailwind v4 uses CSS-first configuration — no `tailwind.config.js` needed.

### Basic Setup

```css
@import "tailwindcss";

@theme {
  /* Map semantic tokens to Tailwind utilities */
  --color-primary: oklch(0.48 0.22 250);
  --color-primary-hover: oklch(0.40 0.19 250);
  --color-primary-fg: oklch(1 0 0);

  --color-secondary: oklch(0.96 0.01 250);
  --color-secondary-fg: oklch(0.15 0.02 250);

  --color-destructive: oklch(0.55 0.22 25);
  --color-destructive-fg: oklch(1 0 0);

  --color-muted: oklch(0.96 0.01 250);
  --color-muted-fg: oklch(0.55 0.02 250);

  /* Surfaces */
  --color-background: oklch(1 0 0);
  --color-foreground: oklch(0.15 0.02 250);
  --color-surface: oklch(0.98 0.005 250);
  --color-border: oklch(0.90 0.01 250);
  --color-input: oklch(0.90 0.01 250);
  --color-ring: oklch(0.48 0.22 250);

  /* Radius */
  --radius-lg: 8px;
  --radius-md: 6px;
  --radius-sm: 4px;

  /* Fonts */
  --font-sans: 'Inter', system-ui, sans-serif;
  --font-display: 'Inter Display', 'Inter', system-ui, sans-serif;
  --font-mono: 'JetBrains Mono', ui-monospace, monospace;
}
```

### Dark Mode with Class Toggle

```css
@layer base {
  .dark {
    --color-background: oklch(0.15 0.02 250);
    --color-foreground: oklch(0.98 0.005 250);
    --color-surface: oklch(0.20 0.02 250);
    --color-border: oklch(0.28 0.02 250);
    --color-input: oklch(0.25 0.02 250);

    --color-primary: oklch(0.55 0.20 250);
    --color-muted: oklch(0.22 0.02 250);
    --color-muted-fg: oklch(0.60 0.02 250);
  }
}
```

### Usage in Components

```tsx
<div className="bg-background text-foreground">
  <div className="bg-surface border border-border rounded-lg p-4">
    <h2 className="font-display text-xl font-semibold">Title</h2>
    <p className="text-muted-fg text-sm">Description</p>
    <button className="bg-primary text-primary-fg hover:bg-primary-hover rounded-md px-4 py-2">
      Action
    </button>
  </div>
</div>
```

### HSL Fallback for Opacity Modifiers

When you need opacity control (`bg-primary/50`), use HSL format alongside OKLCH:

```css
@theme {
  /* HSL format enables Tailwind opacity modifiers */
  --color-primary: hsl(221 83% 53%);
}
```

```tsx
<div className="bg-primary/10">Tinted background</div>
```

---

## Component Tokens

Per-component overrides that reference semantic tokens. Used when a component needs to deviate from the semantic defaults.

```css
:root {
  /* Button */
  --button-radius: var(--radius-md);
  --button-font-weight: var(--font-medium);

  /* Card */
  --card-radius: var(--radius-lg);
  --card-padding: var(--spacing-component-lg);
  --card-shadow: var(--shadow-sm);

  /* Input */
  --input-radius: var(--radius-md);
  --input-height: 40px;
  --input-padding-x: var(--spacing-component-md);

  /* Dialog */
  --dialog-radius: var(--radius-xl);
  --dialog-padding: var(--spacing-section-sm);

  /* Table */
  --table-row-height: 40px;
  --table-cell-padding-x: var(--spacing-component-lg);
  --table-cell-padding-y: var(--spacing-component-md);
}
```

---

## W3C Design Tokens Format (DTCG 2025.10)

For cross-tool interchange (Figma, Style Dictionary, Tokens Studio), use the W3C stable format:

```json
{
  "$name": "brand-tokens",
  "color": {
    "primary": {
      "$value": "oklch(0.48 0.22 250)",
      "$type": "color",
      "$description": "Primary brand action color"
    },
    "primary-hover": {
      "$value": "oklch(0.40 0.19 250)",
      "$type": "color"
    }
  },
  "spacing": {
    "component-md": {
      "$value": "12px",
      "$type": "dimension"
    }
  }
}
```

**Key conventions**:
- All spec properties use `$` prefix (`$value`, `$type`, `$description`)
- File extension: `.tokens.json`
- Aliases via `{token.path}` syntax
- Full CSS Color Module 4 support (OKLCH, P3)

---

## Multi-Brand Theming

For products serving multiple brands, use the `$extends` pattern or CSS data attributes:

```css
/* Base brand */
:root {
  --color-primary: oklch(0.48 0.22 250);  /* blue */
  --color-primary-fg: oklch(1 0 0);
}

/* Brand override via data attribute */
[data-brand="sunset"] {
  --color-primary: oklch(0.55 0.20 25);   /* coral */
}

[data-brand="forest"] {
  --color-primary: oklch(0.50 0.15 155);  /* green */
}
```

```html
<body data-brand="sunset">
  <!-- All components automatically use sunset colors -->
</body>
```

---

## Brand-Tinted Neutrals

Premium interfaces never use pure gray. Tint all neutral surfaces with the brand hue:

```css
/* Instead of pure gray-100: oklch(0.96 0 0) */
/* Use brand-tinted:         oklch(0.96 0.01 250) — trace of blue */

--color-gray-50:  oklch(0.98 0.005 var(--brand-hue));
--color-gray-100: oklch(0.96 0.01  var(--brand-hue));
--color-gray-200: oklch(0.90 0.01  var(--brand-hue));
--color-gray-300: oklch(0.83 0.01  var(--brand-hue));
--color-gray-400: oklch(0.70 0.01  var(--brand-hue));
--color-gray-500: oklch(0.55 0.01  var(--brand-hue));
--color-gray-600: oklch(0.45 0.01  var(--brand-hue));
--color-gray-700: oklch(0.37 0.02  var(--brand-hue));
--color-gray-800: oklch(0.28 0.02  var(--brand-hue));
--color-gray-900: oklch(0.20 0.02  var(--brand-hue));
--color-gray-950: oklch(0.15 0.02  var(--brand-hue));
```

This technique is used by Linear, Stripe, and Vercel. The chroma values (0.005–0.02) are subtle enough to feel neutral but cohesive enough to create a premium, unified feel.
