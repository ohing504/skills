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

**Option B: Private repo (Recommended)**
```bash
gh repo create {username}/portfolio --private --clone
cd portfolio
```
- Reports stay private, only built site is public
- GitHub Actions will build and deploy to Pages
- User controls which reports appear on the Raw Data page

**Option A: Public repo (Simple)**
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
    ├── faq.md
    ├── blog-topics.md
    └── introduced-by-agents.md
```

If the user provides a resume, save it as `resume.md` in the repo root.
