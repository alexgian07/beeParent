# Bee Parent — Claude Code Instructions

## Project
Static HTML/Tailwind POC for a Greek hive-adoption marketplace. 5 pages: `index.html`, `marketplace.html`, `hive-detail.html`, `dashboard.html`, `about.html`. Deployed to GitHub Pages via `alexgian07/beeParent`.

## Workflow — always follow this order
1. **Edit** the file(s)
2. **Inspect in Chrome** using the browser tools (navigate to `http://localhost:8000/dsp-test/<page>.html`, screenshot, verify it looks correct)
3. **Commit and push** — only after visual confirmation

Never push without first checking in Chrome, unless the user explicitly says to skip it.

## Git
- Remote: `https://alexgian07@github.com/alexgian07/beeParent.git`
- Always push after every commit (user's standing preference)
- Use `git push --force` only when amending an already-pushed commit, and only with user confirmation

## Assets & Brand
- Nav logo: `<img src="Docs/LOGO_transparent.PNG" style="height:88px;width:auto;">` inside `overflow:visible` container
- Footer brand mark (dark bg): `<span style="font-weight:300">bee</span><span style="font-weight:700;letter-spacing:0.12em;font-size:0.76em">PARENT</span>`
- Colour palette: honey (amber), sage (green), coral (orange-red), warm (neutral)
- Font: Plus Jakarta Sans

## Content rules
- No real personal info (name, email, employer) anywhere in the HTML — use fictional placeholders (`Alex G.`, `alex@beeparent.gr`, etc.)
- Bilingual: Greek primary, English via `data-en` attribute + JS toggle
- No fabricated beekeeper names unless they already exist in the file
