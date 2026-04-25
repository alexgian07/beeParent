# Typography

> **When to load this reference:** when setting type scales, selecting fonts, defining hierarchy, specifying body/heading styles, or auditing text density. Complements `visual-design-principles.md` (which covers typography at principle level) with deep craft guidance.
>
> **DSP wiring:** Font selection must align with the aesthetic direction from `aesthetic-archetypes.md`. This file provides the *craft rigor* — selection process, modular scale, anti-defaults — that prevents the archetype from collapsing into an AI cliché.

---

## Classic Typography Principles

### Vertical Rhythm

Your line-height should be the base unit for ALL vertical spacing. If body text has `line-height: 1.5` on `16px` type (= 24px), spacing values should be multiples of 24px. This creates subconscious harmony — text and space share a mathematical foundation.

**Leading is not a constant — it scales with line length and x-height.** Long lines need more leading (eyes travel further between lines). Fonts with large x-heights (Inter, Söhne, system-ui) need more leading than fonts with small x-heights (Garamond, Baskerville) at the same size, because the letters occupy more vertical space.

**Rule of thumb:**
- Short lines (40–50ch), small x-height serif: `1.3`
- Standard body (60–70ch), mixed fonts: `1.4–1.5`
- Long lines or tall x-height sans at 65ch+: `1.5–1.65`

### Typographic Color — the evenness test

Well-set text has an even gray tone from corner to corner. Uneven word spacing, rivers of whitespace running vertically through a paragraph, widows and orphans, or inconsistent leading break that evenness and create a subconscious *"something's off"* even when every individual element looks correct.

**The blur test** — the single most diagnostic check in typography:

1. Screenshot a text block
2. Apply a heavy Gaussian blur (or squint hard)
3. The result should look like a **uniform gray rectangle**

If lighter and darker patches show through the blur, the color is broken. Fix word-spacing, hyphenation settings, and line length *before* fixing anything else — because no amount of font selection or scale tuning will hide uneven color.

### Modular Scale & Hierarchy

The common mistake: too many font sizes that are too close together (14px, 15px, 16px, 18px...). This creates muddy hierarchy.

**Use fewer sizes with more contrast.** A 5-size system covers most needs:

| Role | Typical Ratio | Use Case |
|------|---------------|----------|
| xs | 0.75rem | Captions, legal |
| sm | 0.875rem | Secondary UI, metadata |
| base | 1rem | Body text |
| lg | 1.25-1.5rem | Subheadings, lead text |
| xl+ | 2-4rem | Headlines, hero text |

Popular ratios: 1.25 (major third), 1.333 (perfect fourth), 1.5 (perfect fifth). Pick one and commit.

### Readability & Measure

Line length is not a single "right number" — it's a range with an ideal inside it:

| Context | Range | Target |
|---|---|---|
| **Body, long-form reading** | 45–75 characters | **66 characters** |
| **Multi-column layouts, sidebars, captions** | 40–50 characters | 45 characters |
| **Marketing hero / large display** | 25–45 characters | 35 characters |

Wider than 75ch and readers lose their place returning to the next line. Narrower than 45ch and they never settle into rhythm. Use `ch` units for character-based measure — they scale with the chosen font.

```css
.prose        { max-width: 66ch; }   /* body, long-form */
.prose-narrow { max-width: 45ch; }   /* captions, sidebars */
.prose-wide   { max-width: 75ch; }   /* exceptions only */
```

**Non-obvious:** Increase line-height for light text on dark backgrounds. The perceived weight is lighter, so text needs more breathing room. Add 0.05–0.1 to your normal line-height.

### Paragraph Style — indent OR gap, never both

Paragraph separation is either a first-line indent (book tradition, continuous reading) or a vertical gap (web tradition, scannable content). **Using both** is the most common typographic mistake in long-form web content — it double-signals paragraph breaks and weakens both cues.

Pick one per context and hold it:
- **Indent only** — continuous reading (essays, fiction, long-form journalism styled like a book). Indent = `1em` or current leading.
- **Gap only** — scannable content (articles, docs, blog posts, UI copy). Gap = leading or a multiple of it.

```css
/* Continuous long-form */
.prose p + p { text-indent: 1em; }

/* Scannable content (default for most web) */
.prose p { margin-block: 1em; }
```

### Long-form concerns: widows, orphans, rivers

For reading-first content (articles, marketing long-form), modern CSS gives you tools the print world used to hand-set:

```css
.prose p {
  /* Prevent a single word on the last line */
  text-wrap: pretty;

  /* Avoid a lone line of a paragraph at top/bottom of column */
  orphans: 3;
  widows: 3;

  /* Better hyphenation to kill rivers */
  hyphens: auto;
  hyphenate-limit-chars: 6 3 3;
}
```

These are invisible until they're not. One widow at the top of a column in a hero section can undermine an entire landing page.

## Font Selection & Pairing

### Choosing Distinctive Fonts

**Avoid the invisible defaults**: Inter, Roboto, Open Sans, Lato, Montserrat. These are everywhere, making your design feel generic. They're fine for documentation or tools where personality isn't the goal — but if you want distinctive design, look elsewhere.

**Pick the font from the brief, not from a category preset.** The most common AI typography failure is reaching for the same "tasteful" font for every editorial brief, the same "modern" font for every tech brief, the same "elegant serif" for every premium brief. Those reflexes produce monoculture across projects. The right font is one whose physical character matches *this specific* brand, audience, and moment.

### The serif/sans binary is nearly useless

When a brief asks for "serif" or "sans-serif," translate to a more useful dimension: **what historical voice fits the brand?**

| Voice | Signals | Example families |
|---|---|---|
| **Renaissance** | Warm, humanist, calligraphic, bookish | Garamond, Bembo, Sabon |
| **Neoclassical** | Rational, formal, balanced, 18th-century | Baskerville, Bulmer |
| **Romantic** | Dramatic, high-contrast, fashion-forward | Didot, Bodoni |
| **Realist** | Sturdy, plain, newspaper-era | Clarendon, Century |
| **Geometric Modernist** | Machined, neutral, rational, 1920s-Bauhaus | Futura, Avenir, DIN |
| **Humanist Sans** | Warm but clean, readable sans with calligraphic roots | Gill Sans, Frutiger, Optima |
| **Neo-Grotesque** | Neutral workhorse, Swiss-modernist | Helvetica, Akzidenz, Söhne |
| **Contemporary/Postmodern** | Self-aware, mixed-historical, internet-era | FF Meta, Scala, Proxima |

Pick the *voice* before you pick a face. This cuts the selection problem from ten thousand fonts to one voice's worth.

### Font selection process

1. Read the brief once. Write down three concrete words for the brand voice. Not "modern" or "elegant" — those are dead categories. Try "warm and mechanical and opinionated" or "calm and clinical and careful" or "fast and dense and unimpressed" or "handmade and a little weird."
2. Now imagine the font as a physical object the brand could ship: a typewriter ribbon, a hand-lettered shop sign, a 1970s mainframe terminal manual, a fabric label on the inside of a coat, a museum exhibit caption, a tax form, a children's book printed on cheap newsprint. Whichever physical object fits the three words is pointing at the right *kind* of typeface.
3. Translate to a historical voice from the table above. The physical object usually points to one.
4. Browse a font catalog (Google Fonts, Pangram Pangram, Adobe Fonts, Future Fonts, ABC Dinamo) with that voice in mind. **Reject the first thing that "looks designy."** That's your trained-everywhere reflex. Keep looking.
5. Avoid your defaults from previous projects. If you find yourself reaching for the same display font you used last time, make yourself pick something else.

**Anti-reflexes worth defending against**:
- A technical/utilitarian brief does NOT need a serif "for warmth." Most tech tools should look like tech tools.
- An editorial/premium brief does NOT need the same expressive serif everyone is using right now. Premium can be Swiss-modern, can be neo-grotesque, can be a literal monospace, can be a quiet humanist sans.
- A children's product does NOT need a rounded display font. Kids' books use real type.
- A "modern" brief does NOT need a geometric sans. The most modern thing you can do in 2026 is not use the font everyone else is using.

**System fonts are underrated**: `-apple-system, BlinkMacSystemFont, "Segoe UI", system-ui` looks native, loads instantly, and is highly readable. Consider this for apps where performance > personality.

### Learn to see the letterform

Before picking fonts, learn to *see* them. Two fonts in the same historical voice can feel radically different because of their anatomy:

- **x-height** — height of lowercase letters relative to uppercase. Tall = contemporary, screen-friendly, confident. Short = traditional, bookish, reserved.
- **Counter** — the enclosed white space in letters like `o`, `p`, `e`, `a`. Open counters = friendly, readable at small sizes. Closed = formal, tighter.
- **Aperture** — the opening in letters like `c`, `e`, `s`, `a`. Open aperture = humanist, warmer. Closed aperture = geometric, more neutral.
- **Stroke contrast** — thick-to-thin variation. High contrast = elegant, fashion-forward, harder to read small. Low contrast = sturdy, workhorse.
- **Terminal style** — how stroke ends are drawn (serifs, slabs, bracketed, unbracketed, rounded). This is most of a font's "personality" in one detail.

Designers who can't see these pick the wrong font every time. The fastest way to train the eye: put two candidate fonts at the same size in the same setting and name the difference in one of these five dimensions.

### Pairing Principles

**The non-obvious truth**: You often don't need a second font. One well-chosen font family in multiple weights creates cleaner hierarchy than two competing typefaces. Only add a second font when you need genuine contrast (e.g., display headlines + body serif).

When pairing, contrast on multiple axes:
- Serif + Sans (structure contrast)
- Geometric + Humanist (personality contrast)
- Condensed display + Wide body (proportion contrast)
- Different historical voices (voice contrast)

**Never pair fonts that are similar but not identical** (e.g., two geometric sans-serifs). They create visual tension without clear hierarchy.

### Typography is a system, not a choice

You don't pick "a font." You pick a **system**: the face plus its scale, hierarchy, rhythm, weight palette, and relationship to grid and color. A great font in a broken system looks worse than a generic font in a coherent system.

Always design the system first: lock the scale (how many sizes, what ratio), the weight palette (2 or 3 weights, not 6), the rhythm (leading, spacing units). *Then* drop candidate fonts into that system and see which one *completes* it.

### Web Font Loading

The layout shift problem: fonts load late, text reflows, and users see content jump. Here's the fix:

```css
/* 1. Use font-display: swap for visibility */
@font-face {
  font-family: 'CustomFont';
  src: url('font.woff2') format('woff2');
  font-display: swap;
}

/* 2. Match fallback metrics to minimize shift */
@font-face {
  font-family: 'CustomFont-Fallback';
  src: local('Arial');
  size-adjust: 105%;        /* Scale to match x-height */
  ascent-override: 90%;     /* Match ascender height */
  descent-override: 20%;    /* Match descender depth */
  line-gap-override: 10%;   /* Match line spacing */
}

body {
  font-family: 'CustomFont', 'CustomFont-Fallback', sans-serif;
}
```

Tools like [Fontaine](https://github.com/unjs/fontaine) calculate these overrides automatically.

**Always test candidate fonts at 14–16px on a non-Retina display** before committing. A font that looks great at 32px on Retina can be unreadable at 14px on a 1080p laptop — where a large share of real users live. Screen rendering is not print.

## Modern Web Typography

### Fluid Type

Fluid typography via `clamp(min, preferred, max)` scales text smoothly with the viewport. The middle value (e.g., `5vw + 1rem`) controls scaling rate — higher vw = faster scaling. Add a rem offset so it doesn't collapse to 0 on small screens.

**Use fluid type for**: Headings and display text on marketing/content pages where text dominates the layout and needs to breathe across viewport sizes.

**Use fixed `rem` scales for**: App UIs, dashboards, and data-dense interfaces. No major app design system (Material, Polaris, Primer, Carbon) uses fluid type in product UI — fixed scales with optional breakpoint adjustments give the spatial predictability that container-based layouts need. Body text should also be fixed even on marketing pages, since the size difference across viewports is too small to warrant it.

### OpenType Features

Most developers don't know these exist. Use them for polish:

```css
/* Tabular numbers for data alignment */
.data-table { font-variant-numeric: tabular-nums; }

/* Proper fractions */
.recipe-amount { font-variant-numeric: diagonal-fractions; }

/* Small caps for abbreviations */
abbr { font-variant-caps: all-small-caps; }

/* Disable ligatures in code */
code { font-variant-ligatures: none; }

/* Enable kerning (usually on by default, but be explicit) */
body { font-kerning: normal; }
```

Check what features your font supports at [Wakamai Fondue](https://wakamaifondue.com/).

### Optical alignment — hanging punctuation

Traditional typesetting hangs quotation marks, bullets, and other punctuation into the margin so the *letters* align optically, not the *characters*. Modern CSS supports this:

```css
.prose {
  hanging-punctuation: first last allow-end;
}
```

- `first` — hangs opening quotes into the left margin
- `last` — hangs closing quotes into the right margin
- `allow-end` — hangs stops and commas at line ends if it improves rag

This is subtle. Most people won't name what changed. But pull quotes and long-form prose look noticeably more refined with it on.

## Typography System Architecture

Name tokens semantically (`--text-body`, `--text-heading`), not by value (`--font-size-16`). Include font stacks, size scale, weights, line-heights, and letter-spacing in your token system.

## Accessibility Considerations

Beyond contrast ratios (which are well-documented), consider:

- **Never disable zoom**: `user-scalable=no` breaks accessibility. If your layout breaks at 200% zoom, fix the layout.
- **Use rem/em for font sizes**: This respects user browser settings. Never `px` for body text.
- **Minimum 16px body text**: Smaller than this strains eyes and fails WCAG on mobile.
- **Adequate touch targets**: Text links need padding or line-height that creates 44px+ tap targets.

---

**Avoid**: More than 2-3 font families per project. Skipping fallback font definitions. Ignoring font loading performance (FOUT/FOIT). Using decorative fonts for body text. Using both first-line indent AND paragraph gap. Setting long lines with tight leading. Picking fonts without squinting at them at 14px first.

---

## DSP Integration Notes

- **Aesthetic archetype alignment**: Cross-reference `aesthetic-archetypes.md` for archetype-specific type philosophy. This file provides the *rigor* to execute those directions without AI defaults.
- **Color interaction**: For text color, contrast, and dark-mode adjustments, see the `/dsp:color` skill.
- **Token system**: Typography tokens live alongside spatial and color tokens — see `design-tokens-and-theming.md`.

---

*Adapted from [Impeccable](https://github.com/pbakaus/impeccable) by Paul Bakaus — Apache 2.0. See NOTICE.md. Additional craft principles synthesized from the typography canon.*
