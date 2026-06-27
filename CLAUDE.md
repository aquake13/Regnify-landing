# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this project is

A single-file landing page for **Regnify** — a Singapore-based AI-powered RegTech platform for Licensed Representative lifecycle management under MAS regulation. The entire site lives in `index.html` with embedded CSS and JavaScript. No build step, no framework, no dependencies beyond a Google Fonts import.

## Running locally

```
npx serve -p 3000 .
```

Then open `http://localhost:3000`. The `.claude/launch.json` is pre-configured for this — use `preview_start` with name `"Regnify Landing Page"`.

## Deployment

- **GitHub repo**: `https://github.com/aquake13/Regnify-landing`
- **Live site**: `https://aquake13.github.io/Regnify-landing/`
- Pushing to `main` triggers `.github/workflows/deploy.yml`, which deploys automatically to GitHub Pages via `actions/deploy-pages`.
- Use `/updategit` (project slash command) to run the full publish workflow: secrets scan → security scan → README update → commit/push → repo About update → deployment confirmation.

## Git hygiene

- `assets/` is in `.gitignore` — kept local only, never commit it.
- `.claude/settings.local.json` and `.claude/launch.json` are also excluded.
- Binary/source files (`*.pdf`, `*.docx`, `*.pptx`) are excluded to avoid bloating git history.
- Always run a secrets scan before pushing (the `/updategit` command does this automatically).

## Architecture

Everything is in `index.html`:

- **CSS** (`<style>` block) — CSS custom properties for the full brand palette at `:root`, then layout via CSS Grid and Flexbox. No utility classes; each section has its own named selectors.
- **HTML** — Linear one-page structure: Nav → Hero → Problem → Lifecycle → Why → AI → Ecosystem → FAQ → Contact Form → Footer.
- **JavaScript** (`<script>` block at bottom) — Sticky nav on scroll, mobile hamburger menu, IntersectionObserver fade-in animations, FAQ accordion, and async form submission via FormSubmit.

## Brand constraints

Colours, typography, and logo usage are defined in the brand guide at `../Branding/regnify-brand-guide.pdf`. The SVG logos are inlined directly in the HTML (dark version for dark backgrounds, light version for light backgrounds). CSS variables must stay aligned with the brand palette:

```css
--charcoal: #1A1917   --gold: #C8A84B
--warm-dark: #2E2B25  --light-gold: #E8D99A
--ivory: #F0E8D8      --cream: #F5F1EA
--mid-warm: #9A8E7E   --deep-warm: #4A4740
```

Font: Inter (Google Fonts), weights 400/500/600/700.

## Form

The enquiry form submits to `https://formsubmit.co/ask@regnify.sg`. Validation is client-side JS only (name ≥2 chars, email regex, phone regex). No backend. Hidden fields set `_subject`, `_captcha=false`, `_template=table`.

## Key editorial decisions

- Terminology: **Licensed Representatives** (Singapore/MAS), not "Appointed Representatives" (UK/FCA).
- Regulatory context: MAS, CMFAS, RNF, CPD, Form 3A/B/C/D, Financial Advisers Act, Securities and Futures Act.
- AI section intentionally describes capabilities at **4 high-level pillars only** — do not restore granular feature listings to avoid competitor IP exposure.
- Lifecycle stages show **2–3 bullets per column** — do not expand back to granular per-feature lists.
- Phone number intentionally omitted from footer — email only (`ask@regnify.sg`).
- Contact section: no phone number anywhere on the page.
