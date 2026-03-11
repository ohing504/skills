# Deployment Guide — Private Source + Public Deploy

Complete guide for deploying the agent-portfolio site using two repos: a private source repo and a public deploy repo.

## Architecture

```
{username}/portfolio (private)          {username}/{username}.github.io (public)
├── src/                                ├── index.html
├── public/                             ├── _astro/
├── reports/          ── build ──►      │   ├── *.css
├── materials/           push           │   └── *.js
├── .github/workflows/                  └── ...
├── astro.config.mjs                    (only built static files)
└── package.json
```

- Reports and source code stay private in `portfolio`
- Only the built static site is pushed to `{username}.github.io`
- Site URL: `https://{username}.github.io`
- Uses deploy key for secure cross-repo push

## Step 1: Create Both Repos

```bash
# Create private source repo
gh repo create {username}/portfolio --private --clone
cd portfolio

# Create public deploy target (empty — will receive built files)
gh repo create {username}/{username}.github.io --public
```

## Step 2: Set Up Deploy Key

The GitHub Actions workflow in `portfolio` needs write access to `{username}.github.io`. A deploy key (SSH key pair) is the recommended approach — scoped to a single repo with minimal blast radius.

**Generate SSH key pair locally:**
```bash
ssh-keygen -t rsa -b 4096 -f deploy_key -N ""
```

**Add PUBLIC key to the target (public) repo:**
```bash
# Opens browser to add deploy key
gh repo deploy-key add deploy_key.pub --repo {username}/{username}.github.io --title "Portfolio Deploy" --allow-write
```

Or manually: `{username}.github.io` repo > Settings > Deploy keys > Add deploy key > paste `deploy_key.pub` > check "Allow write access"

**Add PRIVATE key as secret in the source (private) repo:**
```bash
gh secret set DEPLOY_KEY --repo {username}/portfolio < deploy_key
```

**Delete local key files:**
```bash
rm deploy_key deploy_key.pub
```

> **Security notes:**
> - Deploy keys are scoped to a single repo (unlike PATs which can access multiple repos)
> - The private key exists only as a GitHub Actions secret — never committed to git
> - Rotate the key periodically (yearly recommended)

## Step 3: Initialize Astro Project

```bash
npm create astro@latest -- --template minimal --no-install
npm install
npm install tailwindcss @tailwindcss/vite
```

## Step 4: Configure Astro

`astro.config.mjs`:
```javascript
import { defineConfig } from 'astro/config';
import tailwindcss from '@tailwindcss/vite';

export default defineConfig({
  site: 'https://{username}.github.io',
  // No base path needed — deploying to {username}.github.io root
  vite: {
    plugins: [tailwindcss()],
  },
  output: 'static',
});
```

> **Note:** Do NOT set `base`. The built files are pushed directly to the `{username}.github.io` repo root, so the site is served from `/`.

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

## Step 5: Create GitHub Actions Workflow

This workflow builds in `portfolio` and pushes the output to `{username}.github.io` using `peaceiris/actions-gh-pages`.

`.github/workflows/deploy.yml`:
```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout source
        uses: actions/checkout@v5

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Build Astro
        run: npm run build

      - name: Disable Jekyll
        run: touch dist/.nojekyll

      - name: Deploy to {username}.github.io
        uses: peaceiris/actions-gh-pages@v4
        with:
          deploy_key: ${{ secrets.DEPLOY_KEY }}
          external_repository: {username}/{username}.github.io
          publish_branch: main
          publish_dir: ./dist
```

> **Why not `withastro/action`?** The official Astro action uses `actions/deploy-pages` which only supports same-repo deployment. For cross-repo deployment, we build manually and use `peaceiris/actions-gh-pages` to push to the external repo.

## Step 6: Enable Pages on the Public Repo

Pages should auto-enable for `{username}.github.io` repos. If not:

1. Go to `{username}.github.io` repo **Settings** > **Pages**
2. Set Source to **Deploy from a branch**
3. Set Branch to **main** / **/ (root)**

> **Note:** For this setup, the source is "Deploy from a branch" (not "GitHub Actions"), because the built files are pushed directly to the `main` branch of the public repo by the deploy action.

## Step 7: Push and Verify

```bash
git add -A
git commit -m "Initial portfolio site — Introduced by My Agents"
git push -u origin main
```

- Check the **Actions** tab in `portfolio` repo for build status
- Once the action completes, site is live at `https://{username}.github.io`

## Update & Redeploy

```bash
# In the portfolio repo
git add reports/ src/
git commit -m "Update portfolio with new reports"
git push
```

GitHub Actions in `portfolio` automatically rebuilds and pushes to `{username}.github.io`.

## Custom Domain (Optional)

**1. Add CNAME file:**
```bash
echo "yourdomain.com" > public/CNAME
```

The CNAME file will be included in the build output and pushed to the public repo.

**2. Update Astro config:**
```javascript
export default defineConfig({
  site: 'https://yourdomain.com',
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

**4.** Enable HTTPS in `{username}.github.io` repo Settings > Pages > Enforce HTTPS

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Deploy action fails with permission error | Verify deploy key: public key on `{username}.github.io` with write access, private key as `DEPLOY_KEY` secret in `portfolio` |
| Pages not showing | Check `{username}.github.io` Settings > Pages > Source is "Deploy from a branch", branch is `main` |
| 404 after deploy | Verify `site` in `astro.config.mjs`, ensure no `base` path is set |
| CSS/styles missing | Ensure `.nojekyll` step exists in workflow (prevents Jekyll from ignoring `_astro/` folder) |
| Build fails | Check Node version (needs 18+), ensure lockfile is committed |
| Actions not triggering | Ensure `.github/workflows/deploy.yml` is on `main` branch of `portfolio` repo |
| Old content still showing | GitHub Pages CDN can cache for up to 10 minutes — wait and hard-refresh |
