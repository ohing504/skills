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
