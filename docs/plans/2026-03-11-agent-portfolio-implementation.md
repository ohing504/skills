# Agent Portfolio Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Create the `agent-portfolio` skill that generates a personalized Astro portfolio site from agent-reference reports, deployed to GitHub Pages.

**Architecture:** Prompt/document-based skill (like agent-reference). SKILL.md guides the agent through: collect inputs → analyze reports → propose concept → scaffold Astro site → customize per concept → deploy. Reference docs provide detailed guidance for each phase.

**Tech Stack:** Astro, Tailwind CSS, GitHub Actions, GitHub Pages. Skill itself is Markdown only.

---

### Task 1: Create Skill Skeleton

**Files:**
- Create: `skills/agent-portfolio/SKILL.md`
- Create: `skills/agent-portfolio/references/` (directory)

**Step 1: Create SKILL.md frontmatter + overview**

Write the SKILL.md with frontmatter (name, description) and the high-level workflow overview. This is the main entry point — it should explain what the skill does and list the steps.

```markdown
---
name: agent-portfolio
description: [see Step 2 for full description]
---

# Agent Portfolio — Introduced by My Agents

Generate a personalized portfolio site from agent-reference reports...

## How This Skill Works
[workflow table: collect → analyze → concept → scaffold → customize → deploy]
```

**Step 2: Write the skill description**

The description must trigger on:
- "build my portfolio", "create portfolio site", "make a site from my reports"
- "deploy to github pages", "github.io site"
- "포트폴리오 사이트 만들어줘", "사이트 배포해줘"
- References to agent-reference reports + site generation

Must NOT trigger on:
- General Astro development, generic website building
- agent-reference analysis (that's the other skill)
- Resume writing without site generation

**Step 3: Verify**

Review that SKILL.md frontmatter is valid YAML and description covers trigger/non-trigger cases.

**Step 4: Commit**

```bash
git add skills/agent-portfolio/SKILL.md
git commit -m "feat(agent-portfolio): create skill skeleton with workflow overview"
```

---

### Task 2: Write Step 1 — Collect Inputs

**Files:**
- Modify: `skills/agent-portfolio/SKILL.md`

**Step 1: Write the "Collect Inputs" section**

This step guides the agent to gather all required and optional inputs:

1. **agent-reference reports** (required) — ask user for report files location
2. **Resume / self-introduction** (optional) — ask if they have one to include
3. **GitHub data** (optional) — check `gh auth status`, offer to analyze
4. **Existing portfolio materials** (optional) — any existing content to incorporate

Include the repo setup decision:
- Option B (recommended): Private repo + GitHub Actions → Pages
- Option A (simple): Public `{username}.github.io` repo

Guide the agent to create the repo with `gh repo create` if needed.

**Step 2: Write the reports directory structure guide**

```
reports/
├── {date}-{agent}/
│   ├── user-profile.md
│   ├── project-{name}.md
│   └── ...
└── aggregated/ (if Phase 2 was run)
    ├── aggregated-profile.md
    ├── faq.md
    └── ...
```

**Step 3: Verify**

Check that the section covers all input types from the design doc and the repo setup flow is clear.

**Step 4: Commit**

```bash
git add skills/agent-portfolio/SKILL.md
git commit -m "feat(agent-portfolio): add input collection and repo setup step"
```

---

### Task 3: Write Concept Generation Guide

**Files:**
- Create: `skills/agent-portfolio/references/concept-generation-guide.md`

**Step 1: Write the report analysis framework**

How to extract personality signals from agent-reference reports:
- Dominant work style (methodical vs. rapid, breadth vs. depth)
- Communication patterns (concise vs. detailed, directive vs. collaborative)
- Technical identity (architect, builder, generalist, specialist)
- Project diversity (many small vs. few large)

**Step 2: Write the concept mapping table**

Map report signals → design concepts:

| Signal cluster | Concept name | Color direction | Typography | Layout emphasis |
|---|---|---|---|---|
| Methodical, architecture-first | Blueprint | Cool blues/grays | Mono headers, clean serif body | Structured grid, whitespace |
| Fast iteration, diverse stack | Launchpad | Vibrant/warm | Bold sans-serif | Project gallery, dynamic cards |
| Deep domain, few projects | Case Study | Muted earth tones | Elegant serif | Long-form content, timeline |
| Full-stack generalist | Spectrum | Multi-accent gradient | Modern sans | Balanced sections, skill matrix |

Include 5-6 concepts with clear visual direction.

**Step 3: Write the concept proposal format**

How the agent should present the concept to the user for approval:
- Concept name + 1-sentence rationale
- Color palette (3-5 colors with hex codes)
- Typography suggestion (heading + body fonts)
- Section ordering recommendation
- "Does this feel right? I can adjust..."

**Step 4: Verify**

Review that concepts are distinct enough to produce visually different sites but all achievable with Tailwind CSS.

**Step 5: Commit**

```bash
git add skills/agent-portfolio/references/concept-generation-guide.md
git commit -m "feat(agent-portfolio): add concept generation guide with 5-6 design concepts"
```

---

### Task 4: Write Site Structure Guide

**Files:**
- Create: `skills/agent-portfolio/references/site-structure-guide.md`

**Step 1: Write the Astro project scaffold specification**

Exact directory structure the agent should generate:

```
src/
├── layouts/
│   └── Layout.astro          # Base layout with meta, fonts, footer
├── pages/
│   ├── index.astro            # Main page (all sections)
│   └── raw-data/
│       └── [...slug].astro    # Dynamic report pages (optional)
├── components/
│   ├── Hero.astro
│   ├── About.astro
│   ├── AgentReviews.astro
│   ├── Projects.astro
│   ├── WorkStyle.astro
│   ├── GitHubActivity.astro
│   ├── FAQ.astro
│   ├── BlogTopics.astro
│   ├── RawData.astro
│   └── Footer.astro
├── content/
│   └── reports/               # Processed report data (from reports/)
├── styles/
│   └── global.css             # Tailwind + concept-specific tokens
└── lib/
    └── parse-reports.ts       # Report markdown → structured data
```

**Step 2: Write per-section component specifications**

For each section, document:
- What data it needs (which report fields)
- Layout variants per concept (e.g., Agent Reviews as cards vs. timeline)
- Required vs. optional rendering
- Responsive behavior

**Step 3: Write the data processing guide**

How to transform agent-reference markdown reports into structured data for Astro components:
- Parse report frontmatter/headers → JSON
- Extract agent personas, project summaries, work style dimensions
- Handle multiple report sets (different dates/agents)

**Step 4: Verify**

Confirm all sections from design doc are covered and the Astro structure follows `astrolicious@astro` skill conventions.

**Step 5: Commit**

```bash
git add skills/agent-portfolio/references/site-structure-guide.md
git commit -m "feat(agent-portfolio): add site structure guide with component specs"
```

---

### Task 5: Write Deployment Guide

**Files:**
- Create: `skills/agent-portfolio/references/deployment-guide.md`

**Step 1: Write Option B (Private repo, recommended)**

Complete step-by-step:
1. `gh repo create {username}/portfolio --private`
2. Initialize Astro project
3. Create `.github/workflows/deploy.yml` (full YAML)
4. Push to GitHub
5. Enable Pages in settings (source: GitHub Actions)
6. Verify site is live at `{username}.github.io`

Include the complete GitHub Actions workflow YAML for Astro → Pages.

**Step 2: Write Option A (Public repo)**

1. `gh repo create {username}/{username}.github.io --public`
2. Same Astro setup
3. Same Actions workflow
4. Pages auto-enabled for `*.github.io` repos

**Step 3: Write the update/redeploy flow**

How to add new reports and rebuild:
1. Copy new report files to `reports/`
2. Run agent-portfolio skill again (or just `npm run build`)
3. Push → Actions rebuilds automatically

**Step 4: Write GitHub Profile README section (optional)**

Template for `{username}/{username}/README.md`:
- Agent quote
- Tech badges
- Portfolio link
- Generation command

**Step 5: Verify**

Test that the GitHub Actions YAML is valid and the Pages deployment flow is correct.

**Step 6: Commit**

```bash
git add skills/agent-portfolio/references/deployment-guide.md
git commit -m "feat(agent-portfolio): add deployment guide with GitHub Actions workflow"
```

---

### Task 6: Complete SKILL.md Workflow

**Files:**
- Modify: `skills/agent-portfolio/SKILL.md`

**Step 1: Write Step 2 — Analyze & Propose Concept**

Reference `concept-generation-guide.md`. Guide the agent to:
1. Read all reports
2. Identify dominant signals
3. Map to a concept
4. Present concept to user for approval

**Step 2: Write Step 3 — Scaffold Site**

Reference `site-structure-guide.md`. Guide the agent to:
1. Create Astro project (`npm create astro@latest`)
2. Add Tailwind (`npx astro add tailwind`)
3. Apply concept theme (colors, fonts, layout)
4. Generate all section components with real report data

**Step 3: Write Step 4 — Customize & Review**

Interactive section:
- Preview the site locally (`npm run dev`)
- User reviews each section
- Agent adjusts based on feedback

**Step 4: Write Step 5 — Deploy**

Reference `deployment-guide.md`. Guide through repo creation, Actions setup, and deployment.

**Step 5: Write the footer and credit section**

Specify the exact footer content:
```
Introduced by My Agents · Built with agent-reference
Last updated: {date}
```

**Step 6: Verify**

Read the full SKILL.md end-to-end and confirm the workflow is complete and coherent.

**Step 7: Commit**

```bash
git add skills/agent-portfolio/SKILL.md
git commit -m "feat(agent-portfolio): complete full workflow in SKILL.md"
```

---

### Task 7: Update Repo Documentation

**Files:**
- Modify: `CLAUDE.md`
- Modify: `README.md`
- Modify: `DESIGN.md`

**Step 1: Update CLAUDE.md**

Add `agent-portfolio` to:
- Repository Structure tree
- Skills section (new subsection)
- Commands section (installation command)

**Step 2: Update README.md**

Add to Available Skills table:

```markdown
| [agent-portfolio](./skills/agent-portfolio/) | Generate a personalized portfolio site from agent-reference reports, deployed to GitHub Pages |
```

**Step 3: Update DESIGN.md**

Mark Phase 3 as implemented. Add to Design Decisions Log:
- "Portfolio site as a separate skill" — reusable by any agent-reference user
- "Astro + GitHub Pages" — static, free hosting, Markdown native

**Step 4: Commit**

```bash
git add CLAUDE.md README.md DESIGN.md
git commit -m "docs: add agent-portfolio to repo documentation"
```

---

### Task 8: Install Astro Skill Dependency

**Files:**
- Modify: `.gitignore` (if needed)

**Step 1: Install astro skill**

```bash
npx skills add astrolicious/agent-skills --skill astro -g -y
```

**Step 2: Document in CLAUDE.md**

Note that `astro` skill is recommended for users building with agent-portfolio.

**Step 3: Commit** (if any repo changes)

```bash
git add -A && git commit -m "chore: install astro skill as recommended dependency"
```

---

### Task 9: End-to-End Test

**Step 1: Dry run the skill**

Pretend to be a user and walk through the SKILL.md workflow mentally:
1. Do I know what inputs are needed?
2. Is the repo setup flow clear?
3. Does the concept generation guide produce a coherent concept?
4. Is the site structure guide specific enough to scaffold an Astro project?
5. Is the deployment guide complete enough to go live?

**Step 2: Verify all cross-references**

Check that every `references/*.md` file mentioned in SKILL.md actually exists and the filenames match.

**Step 3: Verify file counts**

Expected files:
```
skills/agent-portfolio/
├── SKILL.md
└── references/
    ├── concept-generation-guide.md
    ├── site-structure-guide.md
    └── deployment-guide.md
```

**Step 4: Final commit and push**

```bash
git push origin main
```

---

## Summary

| Task | Description | Files |
|------|-------------|-------|
| 1 | Skill skeleton | SKILL.md (frontmatter + overview) |
| 2 | Input collection step | SKILL.md (Step 1) |
| 3 | Concept generation guide | references/concept-generation-guide.md |
| 4 | Site structure guide | references/site-structure-guide.md |
| 5 | Deployment guide | references/deployment-guide.md |
| 6 | Complete SKILL.md workflow | SKILL.md (Steps 2-5) |
| 7 | Update repo docs | CLAUDE.md, README.md, DESIGN.md |
| 8 | Install astro skill | Global skill install |
| 9 | End-to-end test | Verification pass |
