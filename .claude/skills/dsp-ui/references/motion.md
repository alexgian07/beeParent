# Motion

> **When to load this reference:** when specifying transitions, hover/focus feedback, modal/drawer entry, page transitions, or any interactive state change. Covers CSS-level motion — the foundation every polished UI needs.
>
> **DSP wiring:** Motion tokens (durations, easings) belong in the design-token system defined in `design-tokens-and-theming.md`. Motion must also respect `prefers-reduced-motion` and the accessibility baseline enforced by `/dsp:eng_review`.
>
> **Scope boundary:** This file covers *UI motion* (state transitions, micro-interactions, entry/exit). For *illustrative motion* (hero animations, delight moments, mascot loops, onboarding illustrations), use the `lottiefiles-creator` MCP to generate `.lottie` files. The two complement each other — use both when the design calls for it.

---

## Motion must communicate — the first test

Before timing or easing, answer this: **what is the motion telling the user?** If it doesn't answer one of these four, cut it.

| Purpose | What the motion says | Example |
|---|---|---|
| **Relationship** | "This came from here" / "These belong together" | Dropdown opening from its trigger; shared-element transition between list and detail |
| **Causality** | "Your action caused this" | Button compression on click → submission confirmation sliding in |
| **Feedback** | "The system registered that" | Hover lift, focus ring appearing, toggle flipping |
| **Continuity** | "It's the same object, now in a new place" | Card expanding into a modal; sidebar collapsing into an icon |

**Decorative motion fails this test.** A card that floats for no reason, a hero that fades in just because, a spinner that spins after the content is already there — these train users to ignore animation, which makes the *useful* motion weaker. Every animation costs attention; spend it on communication.

## Duration: The 100/300/500 Rule

Timing matters more than easing. These durations feel right for most UI:

| Duration | Use Case | Examples |
|----------|----------|----------|
| **100-150ms** | Instant feedback | Button press, toggle, color change |
| **200-300ms** | State changes | Menu open, tooltip, hover states |
| **300-500ms** | Layout changes | Accordion, modal, drawer |
| **500-800ms** | Entrance animations | Page load, hero reveals |

**Exit animations are faster than entrances** — use ~75% of enter duration.

**The 1-second ceiling.** Any UI animation that lasts over 1 second feels broken unless it's explicitly entertainment or a loading state. Users get restless at ~400ms; tolerance drops hard past that.

## Easing: Pick the Right Curve

**Don't use `ease`.** It's a compromise that's rarely optimal. Instead:

| Curve | Use For | CSS |
|-------|---------|-----|
| **ease-out** | Elements entering | `cubic-bezier(0.16, 1, 0.3, 1)` |
| **ease-in** | Elements leaving | `cubic-bezier(0.7, 0, 0.84, 0)` |
| **ease-in-out** | State toggles (there → back) | `cubic-bezier(0.65, 0, 0.35, 1)` |

The reason ease-out is right for entries: **real objects have mass.** Things arriving at rest decelerate — they don't instantly stop and they don't linearly slow down. Ease-out mimics that physical intuition, which is why an entering modal with ease-out feels like it "lands" while a linear or ease-in modal feels synthetic.

**For micro-interactions, use exponential curves** — they feel natural because they mimic real physics (friction, deceleration):

```css
/* Quart out - smooth, refined (recommended default) */
--ease-out-quart: cubic-bezier(0.25, 1, 0.5, 1);

/* Quint out - slightly more dramatic */
--ease-out-quint: cubic-bezier(0.22, 1, 0.36, 1);

/* Expo out - snappy, confident */
--ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);
```

**Avoid bounce and elastic curves.** They were trendy in 2015 but now feel tacky and amateurish. Real objects don't bounce when they stop — they decelerate smoothly. Overshoot effects draw attention to the animation itself rather than the content.

## Anticipation — the before-the-motion motion

A foundational animation principle: big motion should be preceded by a small opposite cue. A character about to jump crouches first. A car about to accelerate rocks back on its suspension. The anticipation is what makes the main motion feel *caused* rather than arbitrary.

In UI this is subtle but powerful:

- Before a modal expands open at `300ms`, the trigger element can compress for `80ms`
- Before a drawer slides in from the right, it can peek `4px` then slide full
- Before a list item expands into a detail view, it can briefly lift `2px`

Anticipation is rarely longer than 100ms and rarely more than a few pixels of movement. Done right, users never consciously see it — they just feel that actions have weight.

**Don't anticipate everything.** Reserve it for significant state changes (modal, drawer, navigation). Applying anticipation to hover or toggle creates latency.

## Spatial continuity — preserve identity across state changes

When an object is "the same thing" before and after a transition, the motion should preserve that identity. The user's mental model tracks the object through space; breaking continuity forces them to re-acquire it.

Examples:
- A list item that opens into a detail view should **grow** from its list position, not fade in as a new panel
- A sidebar that collapses should **morph** into its collapsed icon, not vanish and be replaced
- A tab change should have an indicator that **travels** between tabs, not appears and disappears

The CSS tool for this in 2026 is view transitions:

```css
.card { view-transition-name: card-hero; }

/* On navigation, the browser animates between the two matching names */
::view-transition-old(card-hero),
::view-transition-new(card-hero) {
  animation-duration: 400ms;
  animation-timing-function: var(--ease-out-quart);
}
```

## Choreography — not everything at once

When multiple elements animate, they should have a narrative order, not all fire simultaneously. Staggering creates a sense of flow; simultaneity feels chaotic and makes the scene harder to parse.

**Hierarchy for choreography:**
1. **Primary** — the element the user's attention is on (the trigger, the new content). Moves first, moves most.
2. **Secondary** — supporting context (background overlay, sibling items, related chrome). Moves after, subtler.
3. **Tertiary** — ambient details (decorative elements, peripheral indicators). Barely moves, often just fades.

Use CSS custom properties for cleaner stagger: `animation-delay: calc(var(--i, 0) * 50ms)` with `style="--i: 0"` on each item. **Cap total stagger time** — 10 items at 50ms = 500ms total. For many items, reduce per-item delay or cap the staggered count (e.g., only stagger the first 5, reveal the rest together).

## Spatial grammar — direction has meaning

Motion direction is not neutral. Users read spatial cues whether designers intend them or not.

| Direction | Reads as |
|---|---|
| **Down** | Arrival, dropdown, additional detail being exposed |
| **Up** | Dismissal, promotion, modal summoning (especially on mobile) |
| **Left → Right** | Forward, progress, next |
| **Right → Left** | Back, undo, dismiss (in LTR locales) |
| **Expand from origin** | "This came from here" (maintain spatial link to trigger) |
| **Fade in place** | Neutral, ambient, non-narrative |

Violating these (e.g., a dropdown that slides up, a "back" gesture that moves right) creates subconscious friction. If you deliberately break a convention, make the break so complete it reads as intentional.

## The Only Two Properties You Should Animate

**transform** and **opacity** only — everything else causes layout recalculation. For height animations (accordions), use `grid-template-rows: 0fr → 1fr` instead of animating `height` directly.

## Reduced Motion

This is not optional. Vestibular disorders affect ~35% of adults over 40.

```css
/* Define animations normally */
.card {
  animation: slide-up 500ms ease-out;
}

/* Provide alternative for reduced motion */
@media (prefers-reduced-motion: reduce) {
  .card {
    animation: fade-in 200ms ease-out;  /* Crossfade instead of motion */
  }
}

/* Or disable entirely */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

**What to preserve**: Functional animations like progress bars, loading spinners (slowed down), and focus indicators should still work — just without spatial movement.

**The reduced-motion alternative should carry the same communication.** If the normal motion communicates "this came from here," the reduced version should preserve that link — e.g., swap a slide-and-grow for a quick crossfade *from the same origin*. Don't just remove the animation; replace it with a lower-motion version that still does the job.

## Perceived Performance

**Nobody cares how fast your site is — just how fast it feels.** Perception can be as effective as actual performance.

**The 80ms threshold**: Our brains buffer sensory input for ~80ms to synchronize perception. Anything under 80ms feels instant and simultaneous. This is your target for micro-interactions.

**Active vs passive time**: Passive waiting (staring at a spinner) feels longer than active engagement. Strategies to shift the balance:

- **Preemptive start**: Begin transitions immediately while loading (iOS app zoom, skeleton UI). Users perceive work happening.
- **Early completion**: Show content progressively — don't wait for everything. Video buffering, progressive images, streaming HTML.
- **Optimistic UI**: Update the interface immediately, handle failures gracefully. Instagram likes work offline — the UI updates instantly, syncs later. Use for low-stakes actions; avoid for payments or destructive operations.

**Easing affects perceived duration**: Ease-in (accelerating toward completion) makes tasks feel shorter because the peak-end effect weights final moments heavily. Ease-out feels satisfying for entrances, but ease-in toward a task's end compresses perceived time.

**Caution**: Too-fast responses can decrease perceived value. Users may distrust instant results for complex operations (search, analysis). Sometimes a brief delay signals "real work" is happening.

## Performance

Don't use `will-change` preemptively — only when animation is imminent (`:hover`, `.animating`). For scroll-triggered animations, use Intersection Observer instead of scroll events; unobserve after animating once. Create motion tokens for consistency (durations, easings, common transitions).

---

**Avoid**: Animating everything (animation fatigue is real). Using >500ms for UI feedback. Ignoring `prefers-reduced-motion`. Using animation to hide slow loading. Bounce/elastic easing. Animating properties other than `transform` and `opacity`. Motion that doesn't communicate (decorative animation). Simultaneous animation of unrelated elements. Direction that contradicts spatial grammar.

---

## When to reach for Lottie instead

Use the `lottiefiles-creator` MCP to generate illustrative motion when:

- Empty states need a small animated character or scene to reinforce the moment
- Hero sections want an expressive, non-UI animation (not a state transition)
- Onboarding wants a short explanatory illustration that loops
- Celebratory moments (first-time success, milestone) want more than a CSS fade

Everything else — transitions, hovers, modals, drawers, loading spinners, focus rings, layout changes — belongs in CSS. Lottie is powerful but heavier; don't reach for it when `transform` and `opacity` will do.

---

## DSP Integration Notes

- **Motion tokens** belong in `design-tokens-and-theming.md`. Define `--duration-*` and `--ease-*` custom properties once; reference them everywhere.
- **Accessibility**: `/dsp:eng_review` checks for `prefers-reduced-motion` handling. If this reference's rules aren't followed, review will flag it.
- **Cross-skill**: For advanced motion (scroll-triggered, view transitions, container queries), see `modern-css-2026.md`.

---

*Adapted from [Impeccable](https://github.com/pbakaus/impeccable) by Paul Bakaus — Apache 2.0. See NOTICE.md. Additional motion-design principles synthesized from animation craft canon (Disney's 12 principles, Val Head, Material Motion spec).*
