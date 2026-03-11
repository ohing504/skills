# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A collection of Agent Skills for Claude and other AI agents. Each skill is a self-contained folder under `skills/` with a `SKILL.md` and optional reference documents. Prompt/document-based — no build system, no runtime code.

All skill content is written in **English** (for global ecosystem compatibility). Report output language follows the user's session language.

## Commands

```bash
# Install skills from this repo
npx skills add ohing504/skills --skill agent-reference agent-portfolio

# Test with skill-creator (installed at .agents/skills/skill-creator/)
# See skill-creator agents/ and references/ for eval workflows
```

## Repository Structure

```
skills/
├── agent-reference/                     # "Introduced by My Agents"
│   ├── SKILL.md                         # Main entry point (Phase 1: analyze, Phase 2: aggregate)
│   └── references/
│       ├── analysis-dimensions.md       # 5 analysis dimensions guide
│       ├── user-profile-template.md     # Per-person report template
│       ├── project-report-template.md   # Per-project report template
│       ├── aggregation-guide.md         # Phase 2 cross-report synthesis
│       ├── persona-perspectives.md      # Multi-perspective refs, FAQ, blog, resume
│       ├── github-analysis-guide.md     # gh CLI-based GitHub profile & repo analysis
│       └── multi-agent-data-sources.md  # Data paths for Cursor, Windsurf, Cline, Aider, etc.
└── agent-portfolio/                     # Portfolio site generator
    ├── SKILL.md                         # Main entry point (5-step workflow)
    └── references/
        ├── concept-generation-guide.md  # Design concept analysis & mapping
        ├── site-structure-guide.md      # Astro project & component specs
        ├── deployment-private.md        # Deploy: private source + public deploy
        └── deployment-public.md         # Deploy: single public repo
```

## agent-reference

AI agents write objective reference check reports about a user based on real collaboration experience. Two phases:
- **Phase 1 (analyze)**: Agent analyzes session history, git, memory files → user profile + project reports
- **Phase 2 (aggregate)**: Combine reports from multiple agents → unified profile, FAQ, blog topics

See `DESIGN.md` for the original concept, Phase 3 portfolio site plan, and design decisions log.

## agent-portfolio

Generate a personalized portfolio site from agent-reference reports. AI analyzes reports to propose a design concept, scaffolds an Astro site with concept-based theming, and deploys to GitHub Pages.
- **Input**: agent-reference reports + resume (optional) + GitHub data (optional)
- **Output**: Astro site deployed to `{username}.github.io`

See `docs/plans/2026-03-11-agent-portfolio-design.md` for the full design document.

## Installed Tools

The `skill-creator` skill (from `anthropics/skills`) is installed at `.agents/skills/skill-creator/`. Use it for drafting, testing, and optimizing skills.

## Key Design Decisions

- **Prompt-based, not script-based**: Agents analyze naturally within their own session context
- **Agent personas are not fixed**: Roles are auto-mapped based on report content
- **Reports split into user profile + project**: Mirrors resume structure
- **Templates are intentionally loose**: Each agent should include unique observations
- **Confidence metadata required**: Reports must declare session count and confidence level
