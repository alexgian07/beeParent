# UX Writing

> **When to load this reference:** when writing button labels, error messages, empty states, form instructions, loading states, or any in-product copy. This is the first home in DSP for copy rigor — previously copy decisions were scattered across UX and UI phases with no canonical guidance.
>
> **DSP wiring:** UX writing decisions should be captured in `UX-DECISIONS.md` during `/dsp:ux` (voice, tone, key error templates) and operationalized in `UI-SPEC.md` during `/dsp:ui` (specific button labels, empty-state copy). `/dsp:eng_review` audits final strings against this reference.

---

## The two laws of UX copy

Before any specific pattern, internalize these. Almost every UX writing failure is a violation of one of them.

### Law 1 — Users scan. Write for scanning.

Research across decades shows users **do not read UI copy**. They scan. They read the first two words of every element, the headers, and nothing else until something catches. Dense copy fails silently — users don't complain, they just miss it.

**Consequences that shape every decision:**
- The **first 1–3 words** of any label, button, heading, or error carry almost all the meaning. Front-load the verb and object.
- **Cut your copy in half. Then cut it again.** Whatever you wrote, shorter is almost always better. This applies to headings, body, buttons, errors, tooltips, everything.
- Use the **inverted pyramid** — most important info first, detail after. Opposite of how people learn to write in school.

| Bad (writerly) | Good (scannable) |
|---|---|
| "Please click here to create a new account" | "Create account" |
| "You have 3 unread messages waiting for you in your inbox" | "3 unread messages" |
| "We were unable to process your payment at this time" | "Payment failed. Try a different card." |
| "Please enter a valid email address in the field above" | "Email needs an @ symbol" |

### Law 2 — Write the copy before the layout

Most teams design the layout, then "fill in the words." This produces cramped copy, placeholder-driven design, and buttons that have to fit a pre-existing box.

**Write content first.** The words define the space; the layout serves the words. If you don't know what a button will say, you don't know how wide it should be. If you don't know what an empty state will explain, you don't know whether it needs an illustration.

When you inherit a design with lorem ipsum or stand-in copy, rewrite the real copy before refining the layout.

## Plain language

Every profession has jargon. Users don't share yours. Default to language a 12-year-old would understand, not because users are dumb, but because clarity is frictionless even for experts.

| Jargon | Plain |
|---|---|
| "Authentication failed" | "Wrong password" |
| "Initialize your profile" | "Set up your profile" |
| "Execute transaction" | "Pay $42" |
| "Utilize" | "Use" |
| "Terminate session" | "Sign out" |
| "Invalid credentials" | "That email or password didn't work" |

**Active voice over passive.** "We saved your changes" (3 words) beats "Your changes have been saved" (5 words, weaker agency).

**Specific verbs over generic ones.** "Create," "send," "delete," "publish" — not "process," "handle," "manage."

**Short sentences.** If a sentence has two clauses, it can usually be two sentences. If it has three, it must be.

## The Button Label Problem

**Never use "OK", "Submit", or "Yes/No".** These are lazy and ambiguous. Use specific verb + object patterns:

| Bad | Good | Why |
|-----|------|-----|
| OK | Save changes | Says what will happen |
| Submit | Create account | Outcome-focused |
| Yes | Delete message | Confirms the action |
| Cancel | Keep editing | Clarifies what "cancel" means |
| Click here | Download PDF | Describes the destination |

**For destructive actions**, name the destruction:
- "Delete" not "Remove" (delete is permanent, remove implies recoverable)
- "Delete 5 items" not "Delete selected" (show the count)

**For paired buttons in dialogs**, both labels should name their outcome:
- ✅ "Delete project" / "Keep project"
- ❌ "Delete" / "Cancel" (what does "cancel" mean here? cancel the deletion, or cancel my earlier intent?)

## Labels, instructions, placeholders — three distinct jobs

These are constantly confused. Each has a role; cramming multiple roles into one creates ambiguity.

| Element | Job | Example |
|---|---|---|
| **Label** | Names the field. Always visible. | "Email address" |
| **Instruction** | Explains format, constraints, or purpose. Visible before input. | "We'll use this to send your receipt." |
| **Placeholder** | Shows an example of the format. Disappears on input. | "name@company.com" |

**Common mistakes:**
- **Using a placeholder as the label** — once the user types, the label disappears. Inaccessible, confusing.
- **Using instructions to restate the label** — "Email address" followed by "Enter your email" is redundant.
- **Omitting labels entirely because placeholders "look cleaner"** — accessibility failure and usability failure.

Always: label visible. Instruction only if the field has non-obvious constraints or purpose. Placeholder only if showing format helps.

## Error Messages: The Formula

Every error message should answer: (1) What happened? (2) Why? (3) How to fix it? Example: "Email address isn't valid. Please include an @ symbol." not "Invalid input".

**Lead with the fix, not the blame.** Users are already stuck. The first words should move them toward resolution.

### Error Message Templates

| Situation | Template |
|-----------|----------|
| **Format error** | "[Field] needs to be [format]. Example: [example]" |
| **Missing required** | "Please enter [what's missing]" |
| **Permission denied** | "You don't have access to [thing]. [What to do instead]" |
| **Network error** | "We couldn't reach [thing]. Check your connection and [action]." |
| **Server error** | "Something went wrong on our end. We're looking into it. [Alternative action]" |

### Don't Blame the User

Reframe errors: "Please enter a date in MM/DD/YYYY format" not "You entered an invalid date".

## Empty States Are Opportunities

Empty states are onboarding moments. The three-part formula:

1. **Acknowledge briefly** — "No projects yet."
2. **Explain the value of filling it** — "Projects help you group related tasks and track progress."
3. **Provide a clear action** — "Create your first project."

**Don't say "No items" and stop.** That's a dead-end. Every empty state is a chance to teach the product and pull the user forward.

## Voice vs Tone

**Voice** is your brand's personality — consistent everywhere.
**Tone** adapts to the moment.

| Moment | Tone Shift |
|--------|------------|
| Success | Celebratory, brief: "Done! Your changes are live." |
| Error | Empathetic, helpful: "That didn't work. Here's what to try..." |
| Loading | Reassuring: "Saving your work..." |
| Destructive confirm | Serious, clear: "Delete this project? This can't be undone." |
| First-run | Welcoming, explanatory: "Let's set up your first project." |
| Routine | Invisible: no personality, just the job. |

**Never use humor for errors.** Users are already frustrated. Be helpful, not cute.

**Voice is best defined by contrast.** "We are warm, not cold. Direct, not verbose. Expert, not arrogant. Playful, not sarcastic." Four or five contrasts give writers a clearer compass than adjectives alone.

## Writing for Accessibility

**Link text** must have standalone meaning — "View pricing plans" not "Click here". Screen reader users often navigate by link list; decontextualized "Click here" links are useless.

**Alt text** describes information, not the image — "Revenue increased 40% in Q4" not "Chart". Use `alt=""` for decorative images.

**Icon buttons** need `aria-label` for screen reader context. An icon button with no accessible name is a button that doesn't exist for a blind user.

**Headings describe structure, not decoration.** Don't skip heading levels for visual effect. If the visual hierarchy needs an `h4`-sized heading as the page title, fix the visual hierarchy, not the semantic level.

## Writing for Translation

### Plan for Expansion

German text is ~30% longer than English. Allocate space:

| Language | Expansion |
|----------|-----------|
| German | +30% |
| French | +20% |
| Finnish | +30-40% |
| Chinese | -30% (fewer chars, but same width) |

### Translation-Friendly Patterns

Keep numbers separate ("New messages: 3" not "You have 3 new messages"). Use full sentences as single strings (word order varies by language). Avoid abbreviations ("5 minutes ago" not "5 mins ago"). Give translators context about where strings appear.

## Consistency: The Terminology Problem

Pick one term and stick with it:

| Inconsistent | Consistent |
|--------------|------------|
| Delete / Remove / Trash | Delete |
| Settings / Preferences / Options | Settings |
| Sign in / Log in / Enter | Sign in |
| Create / Add / New | Create |

Build a terminology glossary and enforce it. Variety creates confusion. A product that says "delete" in one place and "remove" in another has silently told the user that one of those actions is different from the other — even when they're identical.

## Avoid Redundant Copy

If the heading explains it, the intro is redundant. If the button is clear, don't explain it again. Say it once, say it well.

A rule of thumb: read any paragraph and ask "what happens if I delete this sentence?" If nothing happens, delete it. Repeat.

## Loading States

Be specific: "Saving your draft..." not "Loading...". For long waits, set expectations ("This usually takes 30 seconds") or show progress.

**Progress language beats spinner language.** "Uploading 3 of 12 files" tells the user they're 25% through. "Uploading files…" tells them nothing.

## Confirmation Dialogs: Use Sparingly

Most confirmation dialogs are design failures — consider undo instead. Every dialog is a tax on every successful action to prevent rare mistakes, and users stop reading them within a week.

When you must confirm:
- Name the action: "Delete project?"
- Explain consequences: "This can't be undone. Your 47 tasks will also be deleted."
- Use specific button labels: "Delete project" / "Keep project" — never "Yes" / "No"

## Form Instructions

Show format with placeholders, not instructions. For non-obvious fields, explain why you're asking — "We'll use this to send your receipt" beats a field that just says "Email" with no hint of what it's for.

---

**Avoid**: Jargon without explanation. Blaming users ("You made an error" → "This field is required"). Vague errors ("Something went wrong"). Varying terminology for variety. Humor for errors. Using placeholders as labels. Long sentences where short ones work. Passive voice where active works. "Click here" links. Copy written after the layout.

---

## DSP Integration Notes

- **`/dsp:ux` captures**: voice/tone profile (including voice contrasts), terminology glossary (entries in `UX-DECISIONS.md` frontmatter or a dedicated section), key error message templates for this product.
- **`/dsp:ui` applies**: specific button labels, empty-state copy, form instructions, loading messages in `UI-SPEC.md`. **Write copy before finalizing layout**, not after.
- **`/dsp:eng_review` audits**: all in-code strings against this reference — flags "OK/Submit", blame-the-user phrasing, missing `aria-label` on icon buttons, inconsistent terminology, long sentences, passive voice, placeholders masquerading as labels.
- **i18n early**: if the product will be translated, add expansion allowances at layout time — don't retrofit.

---

*Adapted from [Impeccable](https://github.com/pbakaus/impeccable) by Paul Bakaus — Apache 2.0. See NOTICE.md. Additional UX writing principles synthesized from the content-design canon (Nielsen scannability research, GOV.UK plain-language patterns, Torrey Podmajersky's voice frameworks, Kinneret Yifrah's microcopy work).*
