# Deployment Guide

Complete guide for deploying the agent-portfolio site to GitHub Pages, including repo setup, GitHub Actions workflow, and update procedures.

## Option B: Private Repo (Recommended)

### Why Recommended
- Reports may contain private repo analysis — keep source private
- Only the built static site is publicly visible
- User controls exactly which reports appear on the Raw Data page
- GitHub Free plan supports Pages from private repos

### Step-by-Step

**1. Create the repo** (if not done in Step 1):
```bash
gh repo create {username}/portfolio --private --clone
cd portfolio
```

**2. Initialize Astro project:**
```bash
npm create astro@latest -- --template minimal --no-install
npx astro add tailwind --yes
npm install
```

**3. Configure Astro for GitHub Pages:**

`astro.config.mjs`:
```javascript
import { defineConfig } from 'astro/config';
import tailwind from '@astrojs/tailwind';

export default defineConfig({
  site: 'https://{username}.github.io',
  integrations: [tailwind()],
  output: 'static',
});
```

For private repo (non-`.github.io` name), add base path:
```javascript
export default defineConfig({
  site: 'https://{username}.github.io',
  base: '/portfolio',
  // ...
});
```

**4. Create GitHub Actions workflow:**

`.github/workflows/deploy.yml`:
```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm
      - name: Install dependencies
        run: npm ci
      - name: Build Astro
        run: npm run build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./dist

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

**5. Enable GitHub Pages:**
```bash
# Push the code first
git add -A
git commit -m "Initial portfolio site"
git push -u origin main

# Enable Pages via GitHub API
gh api repos/{username}/portfolio/pages \
  --method POST \
  --field source='{"branch":"main","path":"/"}' \
  --field build_type="workflow" 2>/dev/null || echo "Pages may need manual setup in Settings > Pages > Source: GitHub Actions"
```

**6. Verify deployment:**
- Go to repo Settings > Pages
- Source should be "GitHub Actions"
- Wait for the first workflow run to complete
- Site live at `https://{username}.github.io/portfolio`

## Option A: Public Repo (Simple)

### Step-by-Step

**1. Create the repo:**
```bash
gh repo create {username}/{username}.github.io --public --clone
cd {username}.github.io
```

**2-4.** Same as Option B (Astro setup, config, Actions workflow)

But `astro.config.mjs` does NOT need a `base` path:
```javascript
export default defineConfig({
  site: 'https://{username}.github.io',
  integrations: [tailwind()],
  output: 'static',
});
```

**5. Enable Pages:**
Pages is auto-enabled for `{username}.github.io` repos. Just push and wait for the Action to complete.

**6. Verify:** Site live at `https://{username}.github.io`

## Custom Domain (Optional)

If the user has a custom domain:

**1. Add CNAME file:**
```bash
echo "yourdomain.com" > public/CNAME
```

**2. Update Astro config:**
```javascript
export default defineConfig({
  site: 'https://yourdomain.com',
  // remove base if previously set
});
```

**3. Configure DNS:**
- CNAME record: `www` → `{username}.github.io`
- A records for apex domain:
  ```
  185.199.108.153
  185.199.109.153
  185.199.110.153
  185.199.111.153
  ```

**4.** Enable HTTPS in repo Settings > Pages > Enforce HTTPS

## Update & Redeploy

When the user has new reports or wants to update the site:

**1. Add new reports:**
```bash
# Copy new report files to reports/ directory
cp ~/path/to/new-reports/* reports/{date}-{agent}/
```

**2. Regenerate the site:**
Run the `agent-portfolio` skill again — it reads all reports in `reports/` and regenerates components.

Or, if only adding reports without changing the site structure:
```bash
npm run build   # Rebuild with new data
```

**3. Push and deploy:**
```bash
git add reports/ src/
git commit -m "Update portfolio with new reports ({date})"
git push
```
GitHub Actions automatically rebuilds and deploys on push.

## GitHub Profile README (Optional)

If the user opts in, generate `{username}/{username}/README.md`:

**1. Check if profile repo exists:**
```bash
gh repo view {username}/{username} 2>/dev/null || gh repo create {username}/{username} --public --clone
```

**2. Generate README.md:**

```markdown
# Hi, I'm {name} 👋

> "{one-line agent summary quote}"
> — *{Agent persona}, via [Introduced by My Agents](https://github.com/ohing504/skills)*

## Tech Stack
{badges generated from GitHub language data and report analysis}

## Portfolio
🔗 **[{username}.github.io](https://{username}.github.io)** — built from AI agent collaboration reports

---

<sub>Profile generated with [agent-reference](https://github.com/ohing504/skills)</sub>
```

**3. Push:**
```bash
git add README.md
git commit -m "Update profile with agent-reference portfolio link"
git push
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Pages not deploying | Check Settings > Pages > Source is "GitHub Actions" |
| 404 after deploy | Verify `base` path in `astro.config.mjs` matches repo name |
| Build fails | Check Node version (needs 18+), run `npm ci` locally first |
| Custom domain not working | Wait for DNS propagation (up to 48h), verify CNAME file in `public/` |
| Actions workflow not triggering | Ensure `.github/workflows/deploy.yml` is on `main` branch |
