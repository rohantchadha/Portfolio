# Portfolio — Context for Claude Code

## What this is
Live portfolio for Rohant Chadha (Product Designer & PM, New Delhi).
**Live URL:** https://rohantchadha.com  
**GitHub:** https://github.com/rohantchadha/Portfolio  
**Host:** Vercel — auto-deploys on every `git push origin main` (~10 sec to live).

## Repo location
`/Users/rohantchadha/portfolio-deploy/` — this is the actual working folder. Originals at `/Users/rohantchadha/rohant-portfolio/` are an older backup; the deploy folder is the source of truth now.

## Architecture (no framework, static HTML)
- **Landing** is responsive (`index.html`): both desktop and mobile layouts live in ONE file, swapped by CSS at 1025 px breakpoint. Desktop content in `<div class="desktop-layout">`, mobile in `<div class="mobile-layout">`. Mobile IDs/classes are prefixed `m-` to avoid collisions. Two `<script>` IIFEs at the bottom — one for each layout.
- **About / Contact / Work / case studies** are still separate desktop and mobile files (NOT merged). They use a JS responsive redirect: each page has a `<script>` in `<head>` that watches viewport width and `window.location.replace()`s to the counterpart at the 1025 px breakpoint. 300 ms debounce on resize. See any `<head>` for the pattern.
- **Case studies:** Absolute Realty, Rent Flow, KB Foundation, PettyGPT (each has desktop + mobile pair). Pousse Le Rit is `comingSoon: true` in the PROJECTS array — renders an in-window "Coming Soon" stub, no detail page.
- **Fonts:** Google Fonts via CDN per page (varies — JetBrains Mono, Inter, Playfair, etc.) plus one local `MONIQA/Fonts/OpenType-TT (TTF)/Moniqa-Display.ttf` used for the desk title.
- **About page** (`about.html`) is a Figma-Make export — the real content is JSON-escaped inside a `<script type="__bundler/template">` block. Don't try to edit content inside that template by hand; edit the source in Figma Make and re-export. Outer `<head>` is normal HTML — safe to edit meta tags / redirect script.

## File map (active deploy)
```
index.html                          merged responsive landing
splash.html                          optional pre-entry splash
about.html / mobile-about.html       About card (Figma-Make bundle on desktop)
contact.html / mobile-contact.html   Contact (Press Start 2P / VT323 retro)
case-studies-v4.html / mobile-work.html   Work index (React+Babel window mgr)
mobile-absolute-realty.html          ↔ absolute-realty/case-study/index.html
mobile-rentflow.html                 ↔ rent-flow-handoff-v2/rent-flow/project/rentflow-case-study.html
mobile-kb-foundation.html            ↔ kb-foundation/kb-foundation-case-study.html
pettygpt/pettygpt-mobile.html        ↔ pettygpt/pettygpt-case-study.html
favicon.png, og-image.png            social/branding
_archive-removed/                    1 GB of pre-cleanup backup (gitignored, NOT deployed)
```

## Edit workflow
```bash
cd /Users/rohantchadha/portfolio-deploy
# edit files
git add -A
git commit -m "what you changed"
git push
```
Vercel auto-builds in ~10 sec. Check at rohantchadha.com.

## Rules / preferences (carry over from prior sessions)
- **All web work must be equally responsive on mobile AND desktop.** Don't ship desktop-only fixes.
- **PNG positioning gotcha:** when overlaying PNGs onto the desk scene, account for transparent padding when computing `left`/`top`.
- **After every HTML edit, run `open <file>` so it opens in the browser** — user wants to see results immediately, doesn't want to be asked.
- **Don't silently resize decorative text** (e.g., the "ROHANT'S DESK" Moniqa serif title) when fixing layout. Ask first.
- **Use visual-systems-mastery / frontend-design / design-inspiration-engine skills BEFORE any CSS/layout/typography edit.** Don't eyeball it.
- **All design changes go to the deploy folder.** Never touch `/Users/rohantchadha/rohant-portfolio/` (the old original) without explicit go-ahead.

## Known-pending design call (not a bug — design choice)
- Absolute Realty desktop case study has two `<h2 class="section-title">` patterns: 3 are inside `<div class="bento-cell headline">` with a `.section-num` eyebrow; 2 are inside plain `<div class="row">`. The user noticed the visual difference. Decision deferred — discuss before changing.

## Backups
- Full pre-deploy snapshot: `/tmp/portfolio-backup-20260607-095113.tar.gz` (1.37 GB)
- `_archive-removed/` inside deploy folder: 1 GB of files cleaned out (gitignored)
- Original working portfolio: `/Users/rohantchadha/rohant-portfolio/`

## Common tasks
- **Add a new case study:** add an entry to the PROJECTS array in BOTH `case-studies-v4.html` AND `mobile-work.html` (line 139–143 area). Mirror the URL pattern: desktop URL points to the desktop file path, mobile URL points to the mobile file path. Use `comingSoon: true` if no detail page yet.
- **Edit copy on About card:** content is inside `about.html`'s JSON template — open in Figma Make to edit, re-export.
- **Roll back a bad deploy:** Vercel dashboard → Deployments → pick an older one → "Promote to Production".
