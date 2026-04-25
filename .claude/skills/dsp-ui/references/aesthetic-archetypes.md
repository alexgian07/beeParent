# Aesthetic Archetypes — Real-World Design Patterns

Universal design patterns distilled from 55+ production interfaces. Use these archetypes as directional guidance — extract principles, not exact tokens. Every final design system should be custom.

> **Font licensing note:** Proprietary typefaces (sohne-var, Berkeley Mono, Geist, etc.) are cited as reference only. Always suggest open alternatives that achieve the same typographic feel — Inter, Plus Jakarta Sans, DM Sans, JetBrains Mono, Fira Code, or system fonts.

---

## The Five Archetypes

### 1. Dark & Dense

**Feel:** Engineering-precision, information-rich, tool-for-experts
**Reference products:** Linear, Raycast, Warp, GitHub (dark mode)

| Dimension | Principles |
|-----------|-----------|
| Typography | Medium-weight default (500-510), aggressive negative letter-spacing at display sizes, monospace for technical content. Inter or system sans. |
| Color | Near-black canvas (#08-#0f range), single muted accent (indigo/violet), white-opacity hierarchy for text (100%/70%/50%/30%). Avoid pure white — use #f7-#f8. |
| Depth | Luminance stepping (lighter surface = higher elevation), not shadows. Subtle borders via `rgba(255,255,255,0.05-0.08)`. |
| Radius | Small and consistent: 4-6px for controls, 8px for cards. No pill shapes except badges. |
| Spacing | Compact — optimize for information density. 4-8px component gaps, 12-16px section gaps. Power users need data, not whitespace. |
| Dark mode | Dark-native — dark IS the design, not an afterthought. Light mode is the secondary derivation. |

**Token direction:**
- Background surfaces: 3-4 luminance steps from near-black to dark-gray
- Text: white at varying opacity levels, never solid gray hex values
- Accent: single hue reserved exclusively for interactive elements (CTAs, links, focus rings)
- Borders: always semi-transparent white, never solid dark colors
- Transitions: fast (100-150ms) — tool UIs should feel instant

---

### 2. Light & Luxurious

**Feel:** Technical sophistication, confident restraint, premium quality
**Reference products:** Stripe, Clerk, Lemon Squeezy, Resend

| Dimension | Principles |
|-----------|-----------|
| Typography | Light-weight headlines (300-400), heavier body (400), negative letter-spacing for display. Custom or premium sans-serif feel. |
| Color | White/near-white canvas, deep navy text (#06-#0a range, not pure black), single saturated brand color (purple, blue, or green). Blue-tinted neutrals. |
| Depth | Multi-layer blue-tinted shadows using `rgba(50,50,93,0.25)` style. Shadows convey luxury — not flat, not heavy. |
| Radius | Conservative: 4-8px. Precision over friendliness. No large radius on containers. |
| Spacing | Generous vertical padding (80-120px between sections). Whitespace communicates confidence and premium positioning. |
| Dark mode | Light-first with carefully crafted dark variant. Dark mode preserves the premium feel through elevated surfaces and reduced contrast. |

**Token direction:**
- Background: pure or near-white, with subtle warm or cool tinting
- Text: deep navy/charcoal (never pure black) — creates sophistication without harshness
- Accent: one saturated brand color used sparingly — CTAs, key highlights only
- Shadows: blue-tinted, multi-layered — the signature depth cue
- Headlines: lighter weight than expected — the signature typographic inversion

---

### 3. Minimal & Stark

**Feel:** Compiler-like reduction, every pixel justified, nothing decorative
**Reference products:** Vercel, Supabase, Tailwind UI, Cal.com

| Dimension | Principles |
|-----------|-----------|
| Typography | Tight display text with extreme negative letter-spacing (-2 to -3px at large sizes). Strict weight hierarchy: 400/500/600 only. No decorative weights. |
| Color | Monochrome-forward. Near-white bg (#fff), near-black text (#171717, not pure black). One functional accent color. Workflow-specific colors for status only. |
| Depth | Shadow-as-border technique: `box-shadow: 0 0 0 1px rgba(0,0,0,0.08)` instead of `border`. Minimal elevation — mostly flat. |
| Radius | Balanced: 6-8px default. Consistent across all components. Nothing draws attention to shape. |
| Spacing | Gallery emptiness — massive vertical padding (80-120px+) between major sections. Tight within components, generous between them. |
| Dark mode | Clean inversion with careful contrast tuning. Both modes feel equally intentional. |

**Token direction:**
- Background: pure white (light) or near-black (dark), nothing in between for page-level
- Text: exactly two text colors — primary and secondary (muted). No third level.
- Accent: one color, purely functional (focus rings, active states, links). Never decorative.
- Borders: shadow-as-border technique preferred over CSS border
- Everything: if it doesn't serve a function, remove it

---

### 4. Warm & Approachable

**Feel:** Friendly, human, inviting — technology that doesn't feel like technology
**Reference products:** Airbnb, Notion, Linear (marketing pages), Intercom

| Dimension | Principles |
|-----------|-----------|
| Typography | Rounded, friendly sans-serif. Regular to medium weights (400-500). Generous line-height. Comfortable reading experience over density. |
| Color | Warm neutrals (cream, warm-gray undertones). Coral, orange, or warm accent colors. Multiple secondary colors for categorization. Avoids cold blue-grays. |
| Depth | Soft, warm-tinted shadows. Comfortable elevation — cards feel like they float gently, not hover aggressively. |
| Radius | Friendly: 12-16px for cards and containers, 8-12px for inputs and buttons. Rounded shapes signal approachability. |
| Spacing | Comfortable — not dense, not extravagant. 16-24px component gaps. Content breathes but isn't swimming in whitespace. |
| Dark mode | Warm dark — dark surfaces with warm undertones (dark-brown or warm-gray, not cold blue-black). Maintains the cozy feeling. |

**Token direction:**
- Background: warm white or cream (#fafaf8 range), not clinical white
- Text: warm dark (#1a1a1a with warm undertones), never cool blue-black
- Accents: warm spectrum (coral, orange, rose, amber) — can use multiple colors
- Shadows: warm-tinted, soft spread — comfortable, not dramatic
- Radius: noticeably rounded — the single biggest visual signal of approachability
- Illustrations & imagery encouraged — the design makes room for human elements

---

### 5. Bold & Expressive

**Feel:** High-energy, distinctive, unapologetically branded
**Reference products:** Spotify, Discord, Figma, Framer

| Dimension | Principles |
|-----------|-----------|
| Typography | Large, bold display type (700-900). Tight letter-spacing. Display fonts or custom typefaces for impact. Body text still readable at 400-500. |
| Color | Saturated brand palette — multiple vivid colors used freely. High contrast between background and foreground. Gradient use is acceptable and encouraged. |
| Depth | Bold shadows or no shadows — nothing subtle. Flat bold surfaces or dramatic elevation. Glassmorphism and blur effects acceptable. |
| Radius | Variable — ranges from sharp (0-4px for technical elements) to fully rounded (pill buttons, circular avatars). Shape variety creates visual interest. |
| Spacing | Dynamic — tight where dense information matters, generous where impact matters. Visual rhythm varies intentionally. |
| Dark mode | Often dark-native with vibrant colors popping against dark backgrounds. The brand colors ARE the design. |

**Token direction:**
- Background: can be any color — dark, light, or brand-colored sections
- Text: high contrast against whatever background — not subtle
- Accents: multiple brand colors used as first-class citizens, not reserved for CTAs only
- Gradients: linear and radial gradients as design elements, not just backgrounds
- Motion: more aggressive — 300-500ms transitions, spring physics, staggered animations
- Shape: intentional variety — mixing sharp and rounded creates visual energy

---

## Design Dimensions — Cross-Archetype Comparison

### Typography Philosophies

| Approach | Weight Strategy | Letter-Spacing | When to Choose |
|----------|----------------|----------------|----------------|
| **Light-weight confidence** | 300-400 headlines, 400 body | Moderate negative (-0.5 to -1px) | Premium/luxury products, fintech |
| **Medium-weight precision** | 500-510 default, 600 emphasis | Aggressive negative (-1 to -2px) | Developer tools, productivity apps |
| **Balanced readability** | 400 body, 500-600 headings | Minimal (-0.2 to -0.5px) | Consumer apps, content platforms |
| **Bold impact** | 700-900 display, 400 body | Tight (-1.5 to -3px) at display only | Marketing, media, entertainment |

### Color Strategies

| Strategy | Characteristics | When to Choose |
|----------|----------------|----------------|
| **Monochrome-forward** | One text color, one accent. Everything else is grayscale. | Minimal products, developer tools |
| **Single-accent** | Neutral palette + one brand color reserved for CTAs | SaaS, enterprise, fintech |
| **Warm spectrum** | Warm neutrals + multiple warm accents (coral, amber, rose) | Consumer, community, hospitality |
| **Multi-color branded** | 3-5 vivid brand colors used as first-class elements | Creative tools, entertainment, social |
| **Dark-native opacity** | White text at 4-5 opacity levels on near-black canvas | Developer tools, command palettes |

### Shadow & Depth Approaches

| Approach | Technique | When to Choose |
|----------|-----------|----------------|
| **Flat / borderless** | No shadows, minimal borders. Depth from background color stepping | Minimal products, dark UIs |
| **Shadow-as-border** | `box-shadow: 0 0 0 1px rgba(...)` replaces CSS border | Clean interfaces that need subtle structure |
| **Soft layered** | Multi-layer shadows with increasing spread | Cards, dialogs, floating elements in light UIs |
| **Blue-tinted premium** | Shadow layers using `rgba(50,50,93,0.25)` style | Premium/luxury products |
| **Warm soft** | Warm-tinted shadows with generous spread | Friendly, approachable products |

### Border-Radius Philosophies

| Philosophy | Range | Signals | When to Choose |
|------------|-------|---------|----------------|
| **Sharp** | 0-2px | Technical, serious, data-focused | Enterprise dashboards, financial tools |
| **Conservative** | 4-6px | Professional, precise, trustworthy | Fintech, SaaS, developer tools |
| **Balanced** | 8-10px | Modern, clean, versatile | General-purpose apps, content platforms |
| **Friendly** | 12-16px | Approachable, warm, consumer-friendly | Consumer apps, onboarding flows |
| **Pill / fully rounded** | 9999px on buttons | Playful, bold, distinctive | Creative tools, social platforms |

### Spacing Philosophies

| Philosophy | Component Gap | Section Gap | When to Choose |
|------------|--------------|-------------|----------------|
| **Compact / Dense** | 4-8px | 12-16px | Data-heavy tools, productivity apps, power users |
| **Balanced** | 8-16px | 24-48px | Most SaaS applications, dashboards |
| **Comfortable** | 12-24px | 32-64px | Consumer apps, content-first products |
| **Editorial / Generous** | 16-32px | 64-120px+ | Marketing pages, premium products, portfolios |

### Dark Mode Strategies

| Strategy | Characteristics | When to Choose |
|----------|----------------|----------------|
| **Dark-native** | Dark is primary. Near-black (#08-#0f) canvas, luminance hierarchy, opacity-based text. Light mode is derived. | Developer tools, command-line apps, technical products |
| **Light-first with dark** | Light is primary and carefully designed. Dark mode is a thoughtful adaptation — surfaces shift, shadows become borders, elevation uses lighter surfaces. | Most SaaS, enterprise, consumer apps |
| **Warm dark** | Dark surfaces with warm undertones (dark-brown, warm-charcoal). Avoids cold blue-black. Maintains approachable feel. | Consumer apps, community platforms |
| **High-contrast dark** | Deep black backgrounds (#000-#0a) with vivid accent colors. Maximum contrast for visual impact. | Entertainment, gaming, creative tools |

---

## Application Guidance

### From Archetype to Custom Tokens

When a user selects or describes an aesthetic direction:

1. **Identify the closest archetype** (or blend of two)
2. **Extract the 6 dimensional principles** — these are your constraints, not your final values
3. **Generate custom primitive tokens** that follow those principles:
   - Typography: choose an open font that matches the weight/spacing philosophy
   - Colors: generate an OKLCH palette that follows the color strategy (monochrome, warm, multi-color, etc.)
   - Shadows: define elevation scale matching the depth approach
   - Radius: pick a base radius matching the philosophy, derive scale from it
   - Spacing: set component/section gaps matching the density philosophy
4. **Build semantic tokens** from primitives following the standard three-layer architecture
5. **Validate** — does the result feel like the intended archetype? Apply the squint test.

### Blending Archetypes

Users often want combinations: "Like Linear but warmer" or "Stripe's typography with Vercel's spacing." This is valid:

- **Pick a primary archetype** — this sets the dominant feel (60-70% of decisions)
- **Apply selective overrides** from the secondary — only specific dimensions, not wholesale
- **Common blends:**
  - Dark & Dense + Warm = "cozy engineering tool" (Notion dark mode)
  - Minimal & Stark + Light & Luxurious = "premium minimal" (Cal.com)
  - Bold & Expressive + Dark & Dense = "vibrant tool" (Figma, Discord)

### Default When No Direction Given

If no aesthetic direction is specified, default to **Minimal & Stark** with balanced radius and comfortable spacing. This is the most versatile starting point — it's easy to add warmth, boldness, or density later, but hard to remove excessive decoration.

### What NOT to Do

- Do not copy exact hex values from reference products — generate your own palette
- Do not use proprietary fonts — find open alternatives with matching characteristics
- Do not reproduce a brand's entire visual identity — extract principles only
- Do not force an archetype that contradicts the product's purpose (e.g., Bold & Expressive for a medical records system)
- Do not ignore upstream context — if discovery established brand constraints, those override archetype defaults
