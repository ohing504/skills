# Introduced by My Agents

[한국어](./README.ko.md) | **English**

AI agents write objective reference reports about you from real collaboration data, then build a personalized portfolio site.

## Installation

**For Humans** — paste this prompt to your LLM agent (Claude Code, Cursor, etc.):

```
Follow the getting started guide and build my portfolio:
https://raw.githubusercontent.com/ohing504/skills/main/docs/guide/getting-started.md
```

**For LLM Agents**:

```bash
curl -s https://raw.githubusercontent.com/ohing504/skills/main/docs/guide/getting-started.md
```

Or install skills directly:

```bash
npx skills add ohing504/skills --skill agent-reference agent-portfolio
```

## What It Does

```
agent-reference  →  Reference reports (user profile + project analysis)
agent-portfolio  →  Portfolio site on GitHub Pages
```

| Skill | Description |
|-------|-------------|
| [agent-reference](./skills/agent-reference/) | Analyze session history, git, GitHub → objective reference reports |
| [agent-portfolio](./skills/agent-portfolio/) | Reports → personalized Astro portfolio site, deployed to GitHub Pages |

## Quick Start

Just tell your agent:

```
"write a reference for me"          → generates reports
"build my portfolio from reports"   → builds & deploys site
```

## About

Prompt/document-based agent skills — no build system, no runtime code. See [DESIGN.md](./DESIGN.md) for design philosophy.

## License

MIT
