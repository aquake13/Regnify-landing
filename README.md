# Regnify Landing Page

Marketing landing page for [Regnify](https://regnify.sg) — Singapore's AI-powered RegTech platform for end-to-end Licensed Representative lifecycle management.

## Live site

Deployed via GitHub Pages: https://aquake13.github.io/Regnify-landing/

## Stack

Single-file static site — no framework, no build step.

- `index.html` — all HTML, CSS, and JavaScript in one file
- Form submissions via [FormSubmit](https://formsubmit.co) to `ask@regnify.sg`
- Fonts: Inter via Google Fonts

## Local development

```bash
npx serve -p 3000 .
# open http://localhost:3000
```

## Deployment

Pushing to `main` triggers the GitHub Actions workflow (`.github/workflows/deploy.yml`), which deploys automatically to GitHub Pages.

## Contact

[ask@regnify.sg](mailto:ask@regnify.sg)
