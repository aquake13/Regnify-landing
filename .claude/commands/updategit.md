Run the full Regnify GitHub publish workflow. Execute each step in order and stop immediately if any step fails.

## Step 1 — Secrets scan

Scan all tracked files for secrets and sensitive data before anything is pushed. Run the following checks:

```bash
# Check for API keys, tokens, passwords, private keys
grep -rEin "(api[_-]?key|secret[_-]?key|access[_-]?token|auth[_-]?token|private[_-]?key|password\s*=|passwd\s*=|pwd\s*=|bearer\s+[a-z0-9]{20,}|sk-[a-zA-Z0-9]{20,}|ghp_[a-zA-Z0-9]+|-----BEGIN (RSA|EC|OPENSSH|PGP) PRIVATE KEY)" --include="*.html" --include="*.js" --include="*.json" --include="*.md" --include="*.yml" .
```

Also check for unintended credentials in git history:
```bash
git log --all --oneline
git diff HEAD~1 HEAD 2>/dev/null || true
```

If any secrets are found, STOP and report them to the user. Do NOT proceed to any subsequent step.

## Step 2 — Security scan

Scan `index.html` for common web security issues and report findings before pushing:

- **XSS**: Look for `innerHTML`, `document.write`, `eval(`, `setTimeout(string`, `setInterval(string` with unsanitised user input
- **Sensitive data exposure**: Check that no internal email addresses beyond `ask@regnify.sg`, internal URLs, or employee names are embedded
- **Mixed content**: Confirm all external resource URLs (fonts, images) use `https://`
- **Form action**: Confirm the form action URL is `https://formsubmit.co/ask@regnify.sg` and no other endpoints
- **Third-party scripts**: Confirm no unexpected external scripts are loaded

Report any findings. If any HIGH severity issues are found, stop and ask the user how to proceed. LOW/INFO findings may be reported but should not block the publish.

## Step 3 — Create or update README.md

Check if `README.md` exists. If it does not exist, create it. If it exists, review whether it accurately reflects the current state of the project (live URL, stack, deployment method, contact). Update it if needed.

The README should include:
- Project name and one-line description
- Live GitHub Pages URL (`https://aquake13.github.io/Regnify-landing/`)
- Stack (single-file static site, FormSubmit, Inter/Google Fonts)
- Local dev command (`npx serve -p 3000 .`)
- Deployment note (push to `main` triggers GitHub Actions)
- Contact email

## Step 4 — Ensure GitHub Actions workflow exists

Check that `.github/workflows/deploy.yml` exists and is correctly configured to deploy a static site to GitHub Pages on push to `main`. If it is missing or incorrect, create or fix it using the standard `actions/upload-pages-artifact` + `actions/deploy-pages` pattern with `permissions: pages: write, id-token: write`.

## Step 5 — Commit and push

Stage all modified and untracked files (excluding `.claude/`, `*.local*`, and any file flagged in Step 1):

```bash
git add index.html README.md CLAUDE.md .github/workflows/deploy.yml
git status
```

Show the user a summary of what will be committed. Then commit with a concise message describing what changed, and push to `origin main`:

```bash
git push origin main
```

Confirm the push succeeded and show the commit SHA.

## Step 6 — Update repository About

Use the `gh` CLI to update the repository description, homepage URL, and topics so the GitHub repo page is informative:

```bash
gh repo edit aquake13/Regnify-landing \
  --description "Regnify landing page — AI-powered RegTech platform for Licensed Representative lifecycle management (Singapore/MAS)" \
  --homepage "https://aquake13.github.io/Regnify-landing/" \
  --add-topic "regtech" \
  --add-topic "compliance" \
  --add-topic "singapore" \
  --add-topic "mas" \
  --add-topic "landing-page"
```

## Step 7 — Confirm deployment

Check the GitHub Actions run status:

```bash
gh run list --repo aquake13/Regnify-landing --limit 1
```

Report the workflow status to the user. If it is queued or in progress, tell them to monitor it at `https://github.com/aquake13/Regnify-landing/actions`. If it has already succeeded, confirm the live URL: `https://aquake13.github.io/Regnify-landing/`
