# Introduced by My Agents

[한국어](./README.ko.md) | **English**

A collection of [Agent Skills](https://agentskills.io) that let AI agents write objective reference check reports about you based on real collaboration experience, then generate a personalized portfolio site from those reports.

## Available Skills

| Skill | Description |
|-------|-------------|
| [agent-reference](./skills/agent-reference/) | AI agents write objective reference check reports about a user based on real collaboration experience |
| [agent-portfolio](./skills/agent-portfolio/) | Generate a personalized portfolio site from agent-reference reports, deployed to GitHub Pages |

## How It Works

```
1. agent-reference  →  Reports (user profile + project reports)
2. agent-portfolio  →  Portfolio site on GitHub Pages
```

**agent-reference** analyzes session history, git logs, and GitHub profile to generate reference reports about the user. **agent-portfolio** takes those reports and builds a personalized portfolio site deployed to GitHub Pages.

Each skill works independently, but they're designed to work together for the full experience.

## Getting Started

### Step 1: Install Skills

```bash
# Install both skills
npx skills add ohing504/skills --all

# Or install individually
npx skills add ohing504/skills --skill agent-reference
npx skills add ohing504/skills --skill agent-portfolio
```

### Step 2: Generate Reports (agent-reference)

Ask your agent to write a reference report:

```
"write a reference for me"
"analyze my work patterns"
"what do you think of me as a developer"
```

The agent analyzes session history, git, and GitHub data, then generates reports:

```
reports/
├── 2026-03-claude-code/
│   ├── user-profile.md
│   ├── project-financial.md
│   └── project-agentfiles.md
└── 2026-09-cursor/
    ├── user-profile.md
    └── project-newproject.md
```

Run Phase 1 in multiple agents (Claude Code, Cursor, etc.) for diverse perspectives. Use Phase 2 (aggregate) to combine them into unified reports.

### Step 3: Build Portfolio Site (agent-portfolio)

Once reports are ready, ask your agent to build the site:

```
"build my portfolio from the reports"
"create a portfolio site"
"deploy to github pages"
```

The agent analyzes your reports, proposes a design concept, scaffolds an Astro site, and deploys to GitHub Pages.

**6 Design Concepts** — AI auto-suggests a concept based on your report analysis:

| Concept | Best for |
|---------|----------|
| Blueprint | Methodical architects |
| Launchpad | Fast iterators, many projects |
| Chronicle | Deep domain experts |
| Spectrum | Full-stack generalists |
| Terminal | CLI/infra builders |
| Canvas | Creative/design-aware |

### Step 4: Keep It Updated

When you have new projects or switch agents:

1. Run `agent-reference` to generate new reports
2. Add them to `reports/` in your portfolio repo
3. Push — GitHub Actions rebuilds automatically

## About

Each skill is a self-contained folder under `skills/` with a `SKILL.md` file and optional reference documents. Prompt/document-based — no build system, no runtime code.

See [DESIGN.md](./DESIGN.md) for the design philosophy and decisions log.

## License

MIT
