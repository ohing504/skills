# Skills

A collection of [Agent Skills](https://agentskills.io) for Claude and other AI agents.

## Available Skills

| Skill | Description |
|-------|-------------|
| [agent-reference](./skills/agent-reference/) | "Introduced by My Agents" — AI agents write objective reference check reports about a user based on real collaboration experience |
| [agent-portfolio](./skills/agent-portfolio/) | Generate a personalized portfolio site from agent-reference reports, deployed to GitHub Pages |

## Installation

```bash
npx skills add ohing504/skills --skill agent-reference
```

Or install all skills from this repo:

```bash
npx skills add ohing504/skills --all
```

## About

Each skill is a self-contained folder under `skills/` with a `SKILL.md` file and optional reference documents. See [DESIGN.md](./DESIGN.md) for the design philosophy behind agent-reference.

## License

MIT
