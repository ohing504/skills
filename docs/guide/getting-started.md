# Getting Started — Introduced by My Agents

This guide is for LLM agents helping a user set up their portfolio.

**IMPORTANT: Do NOT execute commands without user confirmation.** Present options, explain trade-offs, and wait for the user to choose before taking any action. Every step that involves a choice must be presented to the user first.

## Overview

Two skills work together:
- **agent-reference**: Analyze session history, git logs, GitHub profile → generate reference reports
- **agent-portfolio**: Take those reports → build a personalized Astro portfolio site on GitHub Pages

## Step 1: Create the Portfolio Repo

**Present these numbered options to the user and wait for their choice before proceeding:**

**1. Private source + public deploy (recommended)**
- Two repos: `portfolio` (private) for source/reports, `{username}.github.io` (public) for the deployed site
- Reports stay private, clean URL: `https://{username}.github.io`

```bash
gh repo create {username}/portfolio --private --clone
cd portfolio
gh repo create {username}/{username}.github.io --public
```

**2. Single public repo**
- One repo: everything in one place, but reports are visible in git history
- Site URL: `https://{username}.github.io`

```bash
gh repo create {username}/{username}.github.io --public --clone
cd {username}.github.io
```

After the user chooses, confirm the repo names with them before creating.

## Step 2: Install Skills

```bash
npx skills add ohing504/skills --skill agent-reference agent-portfolio
```

This installs both skills in the portfolio repo.

## Step 3: Collect User Materials

**Ask the user** if they have any of these. Wait for their response before moving on. All are optional:

- **Resume / CV** — any format and filename (`resume.pdf`, `이력서.pdf`, `cv.md`, etc.)
- **Profile photo** — for the site hero/about section
- **Project screenshots** — for project cards
- **Existing portfolio content** — any materials to incorporate

Save user-provided materials to `materials/`:

```
materials/
├── resume.pdf         # or 이력서.pdf, cv.md, etc.
├── profile.jpg        # Profile photo
└── screenshots/       # Project screenshots
```

## Step 4: Generate Reference Reports

This is the core step. Run the `agent-reference` skill to analyze the user's collaboration history.

```
"write a reference for me"
"나에 대한 레퍼런스 써줘"
```

The skill will:
1. Analyze session history, git logs, memory files, and optionally GitHub profile
2. Present discovered projects for the user to select
3. Generate reports into `reports/{date}-{agent-name}/`:
   - `user-profile.md` — who they are as a collaborator
   - `project-{name}.md` — per-project analysis

### Multiple Agents

For richer perspectives, the user can run Phase 1 in different agents:
- Claude Code, Cursor, Windsurf, Cline, Aider, etc.
- Each agent produces its own report set in a separate directory
- Phase 2 (aggregate) combines them into unified outputs

### Deeper Analysis from Working Projects

For better session data access, the user can also install `agent-reference` directly in their working projects:

```bash
# In each project to analyze
npx skills add ohing504/skills --skill agent-reference
```

Then copy the generated reports back to the portfolio repo's `reports/` directory.

## Step 5: Build the Portfolio Site

Run the `agent-portfolio` skill to generate the site.

```
"build my portfolio from the reports"
"포트폴리오 사이트 만들어줘"
```

The skill will:
1. Read all reports and identify dominant traits
2. Propose a design concept (Blueprint, Launchpad, Chronicle, Spectrum, Terminal, or Canvas)
3. Get user approval on the concept
4. Scaffold an Astro + Tailwind site
5. Generate components with real report data
6. Preview locally for user review

See the `agent-portfolio` SKILL.md for the full 5-step workflow.

## Step 6: Deploy

Follow the deployment guide matching the option chosen in Step 1.

### If Option 1 (private source + public deploy)

See `agent-portfolio/references/deployment-private.md` for full details.

1. Set up deploy key (SSH key pair) for cross-repo push
2. Create GitHub Actions workflow using `peaceiris/actions-gh-pages`
3. Push to `portfolio` — Actions builds and pushes to `{username}.github.io`

```bash
git add -A
git commit -m "Initial portfolio site — Introduced by My Agents"
git push -u origin main
```

### If Option 2 (single public repo)

See `agent-portfolio/references/deployment-public.md` for full details.

1. Create GitHub Actions workflow using `withastro/action` (official)
2. Enable Pages in repo Settings > Source: GitHub Actions
3. Push — Actions builds and deploys automatically

```bash
git add -A
git commit -m "Initial portfolio site — Introduced by My Agents"
git push -u origin main
```

Site will be live at `https://{username}.github.io`.

## Updating

When the user has new reports (new projects, new agents, new machine):

1. Run `agent-reference` to generate new reports
2. Copy to `reports/` in the portfolio repo (or the public repo for Option 2)
3. Push — GitHub Actions rebuilds automatically

## Expected Repo Structure

### Option 1: Private source + public deploy

```
portfolio/ (private)
├── reports/
│   ├── 2026-03-claude-code/
│   │   ├── user-profile.md
│   │   └── project-*.md
│   └── aggregated/
│       ├── aggregated-profile.md
│       ├── references-multi-perspective.md
│       ├── faq.md
│       └── blog-topics.md
├── src/
├── public/
├── materials/
│   ├── resume.pdf
│   └── profile.jpg
├── astro.config.mjs
├── package.json
├── .github/workflows/deploy.yml
└── .gitignore

{username}.github.io/ (public — receives built files only)
├── index.html
├── _astro/
│   ├── *.css
│   └── *.js
└── .nojekyll
```

### Option 2: Single public repo

```
{username}.github.io/ (public)
├── reports/
│   ├── 2026-03-claude-code/
│   │   ├── user-profile.md
│   │   └── project-*.md
│   └── aggregated/
│       ├── aggregated-profile.md
│       ├── references-multi-perspective.md
│       ├── faq.md
│       └── blog-topics.md
├── src/
├── public/
├── materials/
│   ├── resume.pdf
│   └── profile.jpg
├── astro.config.mjs
├── package.json
├── .github/workflows/deploy.yml
└── .gitignore
```
