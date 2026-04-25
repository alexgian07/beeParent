# Pre-Delivery Visual Checklist

Run through this checklist before shipping any UI work. Items are grouped by category and marked with severity. Fix all CRITICAL items. Fix SERIOUS items unless there's a documented reason not to. MODERATE items improve polish but are not blockers.

---

## 1. Visual Quality (CRITICAL)

- [ ] All elements align to the 8px grid (4px for fine adjustments)
- [ ] Spacing is consistent — same relationship = same spacing everywhere
- [ ] Type scale is defined and followed (no arbitrary font sizes)
- [ ] No emoji used as icons — use SVG/vector icons only (Lucide, Heroicons)
- [ ] Icon set is consistent — same family, same stroke width, same sizing tokens
- [ ] Colors come from semantic tokens, not hardcoded hex/rgb values
- [ ] Neutrals are brand-tinted (not pure gray) for premium feel
- [ ] Visual hierarchy is clear — squint test passes (blur your vision, can you tell what's important?)
- [ ] Related items are grouped by proximity (Gestalt)
- [ ] Primary action is visually dominant on every screen — one primary CTA max

---

## 2. Interaction States (CRITICAL)

- [ ] Every interactive element has hover, active, focus, and disabled states defined
- [ ] Focus rings are visible on all focusable elements (2px+ ring, `:focus-visible`)
- [ ] Focus is never obscured by sticky headers, footers, or overlays (WCAG 2.4.11)
- [ ] Disabled elements use 50% opacity + `pointer-events: none` + `aria-disabled`
- [ ] Transitions use 150ms for color/bg changes, 200ms for transforms
- [ ] Loading states use skeleton screens for content areas, spinners for actions
- [ ] Buttons show loading state during async operations (spinner, disabled)
- [ ] State priority is respected: disabled > loading > active > focus > hover > default
- [ ] Scale feedback on press: `scale(0.98)` for buttons/cards, restored on release
- [ ] Cursor changes appropriately: `pointer` for clickable, `not-allowed` for disabled

---

## 3. Light/Dark Mode (SERIOUS)

- [ ] Both themes are designed and tested (not one derived from the other)
- [ ] Primary text contrast >= 4.5:1 in both modes (WCAG AA)
- [ ] Secondary text contrast >= 3:1 in both modes
- [ ] Borders and dividers are visible in both modes (test explicitly)
- [ ] Shadows are replaced with borders/elevation in dark mode
- [ ] Dark mode uses lighter surfaces for elevation (not darker)
- [ ] Colors are desaturated 1-2 stops in dark mode
- [ ] Modal/drawer scrim is strong enough: 50% (light), 70% (dark)
- [ ] Status colors (success, warning, error) maintain contrast in both modes
- [ ] No pure white text on pure black background (use gray-50 on gray-950)

---

## 4. Layout & Responsive (SERIOUS)

- [ ] No horizontal scroll on any viewport width
- [ ] Tested at 3 breakpoints minimum: 375px (phone), 768px (tablet), 1280px (desktop)
- [ ] Tested in landscape orientation
- [ ] Safe areas respected (notch, gesture bar, Dynamic Island)
- [ ] Container queries used for reusable components in different contexts
- [ ] Fixed/sticky elements reserve padding for content underneath
- [ ] Content width is capped on large screens (max-w-7xl or similar)
- [ ] Spacing rhythm is maintained: 4/8px system at component level, 24/32/48px at section level
- [ ] Bento/card grids collapse gracefully (4 cols → 2 → 1)
- [ ] Mobile: core content visible first, secondary content folded or deferred

---

## 5. Accessibility Visual (CRITICAL)

- [ ] Touch/click targets are at least 24x24px (WCAG 2.2 AA) — 44x44px preferred
- [ ] Color is never the only indicator — always paired with icon, text, or pattern
- [ ] Focus indicator is not entirely hidden by any overlay or sticky element
- [ ] Heading hierarchy is sequential (h1 → h2 → h3, no skipping)
- [ ] Meaningful images have descriptive alt text
- [ ] Decorative images have `alt=""`
- [ ] Sufficient contrast: 4.5:1 for normal text, 3:1 for large text and UI components
- [ ] Dragging interactions have single-pointer alternatives (WCAG 2.5.7)
- [ ] Animations respect `prefers-reduced-motion` (reduce or disable)
- [ ] No flashing content (3 flashes per second max)

---

## 6. Typography (MODERATE)

- [ ] Fluid type scale uses `clamp()` — no hard breakpoint jumps
- [ ] Body text: 45-75 characters per line (desktop), 35-60 (mobile)
- [ ] Line height: 1.4-1.6 for body, 1.1-1.3 for headings
- [ ] Display font used for headings, body font for text (if using a pair)
- [ ] Font weight hierarchy: 600-700 headings, 500 labels, 400 body
- [ ] ALL CAPS text has increased letter-spacing (+2-5%)
- [ ] Numbers in data columns use `font-variant-numeric: tabular-nums`
- [ ] Variable font loaded with `font-display: swap` to avoid FOIT
- [ ] Truncation uses ellipsis with tooltip/expand for full text

---

## 7. Data Display (SERIOUS)

- [ ] Tables: numbers right-aligned, text left-aligned, actions right-aligned
- [ ] Tables: header is sticky with proper z-index
- [ ] Tables: empty state shows helpful message + action (not blank)
- [ ] Tables: loading state uses skeleton rows (not spinner)
- [ ] Tables: supports at least one density option (compact/default/comfortable)
- [ ] Lists with 50+ items use virtual scrolling
- [ ] KPI/metric cards show: label, value, comparison/trend
- [ ] Charts have legends, axis labels, and tooltips
- [ ] Charts don't rely on color alone (use patterns or direct labels)
- [ ] Number formatting is consistent (thousand separators, decimal places, abbreviations)

---

## 8. Animation & Performance (MODERATE)

- [ ] Micro-interactions: 150-300ms duration, ease-out for enter, ease-in for exit
- [ ] Exit animations are 60-70% of enter duration (feels responsive)
- [ ] Only animate `transform` and `opacity` (never width, height, top, left)
- [ ] Staggered list animations: 30-50ms per item
- [ ] No animation blocks user input — UI stays interactive during motion
- [ ] Skeleton screens shown only when load > 500ms (avoid flash)
- [ ] Above-fold content prioritized (progressive loading)
- [ ] `content-visibility: auto` used for long off-screen sections
- [ ] Images use `loading="lazy"` for below-fold content
- [ ] No layout shift from async content (reserve space with aspect-ratio or fixed dimensions)

---

## Quick Pass (Top 5 — check these first)

1. Squint test — can you identify what's important?
2. Focus ring — tab through the page, are rings always visible?
3. Dark mode — switch themes, is everything readable?
4. Mobile — resize to 375px, any horizontal scroll or overlap?
5. Loading — slow network, do skeletons show instead of blank space?
