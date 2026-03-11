---
name: agent-portfolio
description: Generate a personalized portfolio site from agent-reference reports and deploy it to GitHub Pages. The site reflects the user's working style as observed by their AI collaborators — AI analyzes the reports, proposes a design concept, scaffolds an Astro site with concept-based theming, and deploys to {username}.github.io. Use this skill whenever the user asks to "build my portfolio", "create portfolio site", "make a site from my reports", "deploy to github pages", "github.io site", or says things like "포트폴리오 사이트 만들어줘", "사이트 배포해줘", or wants to turn agent-reference reports into a live website. Also triggers when the user has agent-reference reports ready and wants to publish them as a site, wants a personal site generated from AI collaboration data, or asks to update/redeploy an existing agent portfolio. Do NOT use for general Astro development, generic website building, agent-reference analysis without site generation, or resume writing that does not involve deploying a site.
---

# Agent Portfolio — Introduced by My Agents

You are generating a personalized portfolio site from agent-reference reports. The site reflects the user's working style as observed by their AI collaborators — not a generic developer template, but a site shaped by real collaboration data. Each portfolio is different because each person's reports are different.

The result is an Astro site deployed to GitHub Pages at `{username}.github.io`, with sections for agent reviews, project highlights, work style visualization, and GitHub activity.

## How This Skill Works

| Step | What happens |
|------|-------------|
| 1. Collect Inputs | Gather agent-reference reports, resume, GitHub data |
| 2. Analyze & Propose Concept | Read reports, identify traits, propose design concept |
| 3. Scaffold Site | Create Astro project with concept-based theme |
| 4. Customize & Review | User reviews, agent adjusts |
| 5. Deploy | Push to GitHub, enable Pages, go live |

Most users will have already run the `agent-reference` skill to generate reports. If not, suggest running that skill first.

---

## Step 1: Collect Inputs

This step gathers all data needed to generate the portfolio. Guide the user through each input:

**1. Agent-reference reports (required)**
- Ask the user where their report files are
- Expected structure: markdown files from agent-reference Phase 1 and optionally Phase 2
- If no reports exist, suggest running the `agent-reference` skill first and stop here

**2. Resume / self-introduction (optional)**
- Ask if they have a resume, CV, or self-intro to include
- This populates the "About" section
- Any format (markdown, PDF text, plain text)

**3. GitHub data (optional)**
- Check `gh auth status` to see if gh CLI is available
- If available, offer to pull: contribution calendar, repo list, language stats
- Reference: `gh api graphql` for contribution data, `gh repo list` for repos

**4. Existing portfolio materials (optional)**
- Any existing site content, project descriptions, or images to incorporate

### Set Up the Repository

After collecting inputs, help the user create the portfolio repo:

**Option A: Private repo (Recommended)**
```bash
gh repo create {username}/portfolio --private --clone
cd portfolio
```
- Reports stay private, only built site is public
- GitHub Actions will build and deploy to Pages
- User controls which reports appear on the Raw Data page

**Option B: Public repo (Simple)**
```bash
gh repo create {username}/{username}.github.io --public --clone
cd {username}.github.io
```
- Simplest setup — repo IS the site
- All reports visible in git history
- Fine if user has no private repo content

### Organize Reports

Copy reports into the repo:
```
reports/
├── {date}-{agent-name}/
│   ├── user-profile.md
│   ├── project-{name}.md
│   └── ...
└── aggregated/          (if Phase 2 was run)
    ├── aggregated-profile.md
    ├── references-multi-perspective.md
    ├── introduced-by-agents.md
    ├── faq.md
    └── blog-topics.md
```

If the user provides a resume, save it as `resume.md` in the repo root.

### Portfolio Repo Structure

After setup, your repo should look like this:

```
portfolio/
├── reports/                    # Agent-reference reports (required)
│   ├── 2026-03-claude-code/
│   │   ├── user-profile.md
│   │   └── project-*.md
│   └── aggregated/             # Phase 2 outputs (if available)
│       ├── aggregated-profile.md
│       ├── references-multi-perspective.md
│       ├── faq.md
│       └── blog-topics.md
├── resume.md                   # Your resume/CV (optional)
├── assets/                     # Profile photo, project screenshots (optional)
│   └── profile.jpg
└── README.md                   # Will be auto-generated
```

The skill will generate the `src/`, `public/`, and config files in the next steps. You only need to provide the data above.

---

## Step 2: Analyze & Propose Concept

Read `references/concept-generation-guide.md` for the full concept generation workflow.

### What You Do

1. **Read all reports** in the `reports/` directory — user profiles, project reports, aggregated outputs
2. **Identify dominant traits** — work style signals, communication patterns, technical identity
3. **Map to a design concept** — each concept has a color palette, typography, and layout direction
4. **Present the concept** to the user for approval

### Available Concepts

| Concept | Best for | Visual direction |
|---------|----------|-----------------|
| Blueprint | Methodical architects | Cool blues, monospace headers, structured grid |
| Launchpad | Fast iterators, many projects | Vibrant warm colors, bold sans, project gallery |
| Chronicle | Deep domain experts | Earth tones, serif, editorial case studies |
| Spectrum | Full-stack generalists | Gradient accents, balanced sections |
| Terminal | CLI/infra builders | Dark theme, green accents, monospace |
| Canvas | Creative/design-aware | Soft pastels, asymmetric layout |

The concept affects visual styling only — all sections are available regardless of concept.

**Always get user approval before proceeding.** Show the proposed color palette, fonts, and section order. Offer to adjust or try a different concept.

---

## Step 3: Scaffold Site

Read `references/site-structure-guide.md` for complete component specifications.

### Initialize Astro

```bash
npm create astro@latest -- --template minimal --no-install
npx astro add tailwind --yes
npm install
```

### Generate Components

Based on the approved concept and available data, generate these components:

| Component | Data source | Required |
|-----------|------------|----------|
| Hero.astro | aggregated profile or first user-profile report | Yes |
| About.astro | resume.md | Only if resume provided |
| AgentReviews.astro | user-profile reports (persona sections) | Yes |
| Projects.astro | project-*.md reports | Yes |
| WorkStyle.astro | analysis dimensions from profiles | Yes |
| GitHubActivity.astro | gh CLI data from Step 1 | Only if GitHub data collected |
| FAQ.astro | faq.md from aggregated outputs | Only if available |
| BlogTopics.astro | blog-topics.md from aggregated outputs | Only if available |
| RawData.astro | report files user chose to make public | Optional |
| Footer.astro | auto-generated | Yes |

### Apply Concept Theme

Configure `tailwind.config.mjs` with concept-specific design tokens (colors, fonts) from the approved concept. Import Google Fonts in `Layout.astro`.

### Process Report Data

Create `src/lib/parse-reports.ts` to transform raw markdown reports into structured data for components. See site-structure-guide.md for the parsing functions specification.

---

## Step 4: Customize & Review

### Preview Locally

```bash
npm run dev
```

Open `http://localhost:4321` and walk through the site with the user.

### Review Checklist

Go through each section with the user:

- [ ] **Hero** — Does the summary line capture who they are?
- [ ] **Agent Reviews** — Are the persona voices distinct and accurate?
- [ ] **Projects** — Are the right projects highlighted? Any to add/remove?
- [ ] **Work Style** — Does the visualization feel accurate?
- [ ] **Overall feel** — Does the concept match their identity?

### Adjust

Based on feedback:
- **Content changes** — update report excerpts, rewrite summaries
- **Design changes** — adjust colors, fonts, section order in Tailwind config
- **Section changes** — add/remove optional sections
- **Concept change** — if the whole direction feels wrong, go back to Step 2

Iterate until the user approves the site.

---

## Step 5: Deploy

Read `references/deployment-guide.md` for the complete deployment workflow.

### Quick Deploy

```bash
git add -A
git commit -m "Initial portfolio site — Introduced by My Agents"
git push -u origin main
```

GitHub Actions will automatically build and deploy. The site will be live at:
- **Private repo**: `https://{username}.github.io/portfolio`
- **Public repo**: `https://{username}.github.io`

### Verify

- Check the Actions tab in GitHub for build status
- Visit the site URL once deployment completes
- Test on mobile (responsive check)

### GitHub Profile README (Optional)

If the user wants, generate a minimal GitHub Profile README that links to the portfolio. See deployment-guide.md for the template.

---

## Updating the Site

When you have new reports (new projects, new machine, new agent):

1. Run `agent-reference` skill again → new report files
2. Copy new reports to `reports/` in the portfolio repo
3. Run this skill again or just `npm run build` to regenerate
4. Push → GitHub Actions rebuilds automatically

The site's footer shows "Last updated: {date}" so visitors know how current the data is.

---

## Important Principles

**The site tells the user's story through agent eyes.** Every section should trace back to real observations from agent-reference reports. Don't pad with generic content.

**Concept serves content.** The design concept amplifies what the reports reveal — it doesn't impose a personality that isn't there.

**User controls everything.** Which reports to include, which sections to show, what to make public. The agent proposes; the user decides.
