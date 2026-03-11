# Deployment Guide — Single Public Repo

Complete guide for deploying the agent-portfolio site using a single `{username}.github.io` public repo.

## Architecture

```
{username}/{username}.github.io (public)
├── src/                          # Astro source
├── public/                       # Static assets
├── reports/                      # agent-reference reports (visible in git history)
├── materials/                    # Resume, photos (visible in git history)
├── .github/workflows/deploy.yml  # Build & deploy
├── astro.config.mjs
└── package.json
```

- Everything in one repo — simplest setup
- All reports and materials are visible in git history
- Site URL: `https://{username}.github.io`

## Step 1: Create the Repo

```bash
gh repo create {username}/{username}.github.io --public --clone
cd {username}.github.io
```

## Step 2: Initialize Astro Project

```bash
npm create astro@latest -- --template minimal --no-install
npm install
npm install tailwindcss @tailwindcss/vite
```

## Step 3: Configure Astro

`astro.config.mjs`:
```javascript
import { defineConfig } from 'astro/config';
import tailwindcss from '@tailwindcss/vite';

export default defineConfig({
  site: 'https://{username}.github.io',
  // No base path needed — {username}.github.io repo serves from root
  vite: {
    plugins: [tailwindcss()],
  },
  output: 'static',
});
```

> **Note:** Do NOT set `base`. The `{username}.github.io` repo name matches the root domain pattern, so no subpath is needed.

Create the global CSS file:

`src/styles/global.css`:
```css
@import "tailwindcss";
```

Import in your layout:
```astro
---
import '../styles/global.css';
---
```

## Step 4: Create GitHub Actions Workflow

Uses the official `withastro/action` — handles build, `.nojekyll` injection, and artifact upload automatically.

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
        uses: actions/checkout@v5
      - name: Build site
        uses: withastro/action@v5

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

## Step 5: Enable Pages

Pages is typically auto-enabled for `{username}.github.io` repos. If not:

1. Go to repo **Settings** > **Pages**
2. Set Source to **GitHub Actions**

## Step 6: Push and Verify

```bash
git add -A
git commit -m "Initial portfolio site — Introduced by My Agents"
git push -u origin main
```

- Check the **Actions** tab for build status
- Site live at `https://{username}.github.io`

## Update & Redeploy

```bash
# Add new reports, rebuild, push
git add reports/ src/
git commit -m "Update portfolio with new reports"
git push
```

GitHub Actions automatically rebuilds and deploys on push.

## Custom Domain (Optional)

**1. Add CNAME file:**
```bash
echo "yourdomain.com" > public/CNAME
```

**2. Update Astro config:**
```javascript
export default defineConfig({
  site: 'https://yourdomain.com',
  // No base needed with custom domain
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

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Pages not deploying | Check Settings > Pages > Source is "GitHub Actions" |
| 404 after deploy | Verify `site` in `astro.config.mjs` is correct |
| CSS/styles missing | Ensure `.nojekyll` exists (auto-handled by `withastro/action`) |
| Build fails | Check Node version (needs 18+), ensure lockfile is committed |
| Actions not triggering | Ensure `.github/workflows/deploy.yml` is on `main` branch |
