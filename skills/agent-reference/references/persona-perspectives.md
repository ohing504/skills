# Persona Perspectives & Extended Outputs

This guide covers the creative/presentation layer of aggregation: assigning personas to agents, generating multi-perspective references, FAQ, blog topics, and resume-ready text.

---

## Persona Auto-Mapping

Assign a role to each agent based on what their report primarily covers — not what the agent "is", but what perspective their report reflects.

| Report primarily discusses | Assigned role |
|---------------------------|---------------|
| Architecture, technical strategy, system design | CTO |
| Product planning, market analysis, prioritization | PM |
| Day-to-day implementation, debugging, code review | Team Colleague |
| Learning, exploratory questions, knowledge building | Mentor / Mentee |
| UX decisions, design reasoning, user empathy | Design Lead |
| DevOps, infrastructure, deployment patterns | Platform Engineer |

**Rules:**
- The same agent can have different roles for different projects (if they discussed architecture for project A but did implementation for project B)
- Display as: **"[Role]. [Agent Name]"** — e.g., "CTO. Claude Projects"
- If an agent's report spans multiple roles roughly equally, pick the dominant one or use "Senior Colleague"
- This mapping is derived from content, not predetermined — every user gets a different "team"

---

## Multi-Perspective Reference

Generate `references-multi-perspective.md` — formatted like product reviews, where each agent-persona writes a brief reference in their own voice.

```markdown
# References — Multi-Perspective

## [Role]. [Agent Name]
> **Relationship**: [How they worked together — e.g., "Collaborated on architecture decisions for 6 months"]
> **Confidence**: [H/M/L]

[2-4 paragraph reference in this persona's voice. A CTO would emphasize strategic thinking and system design. A team colleague would emphasize day-to-day collaboration. A PM would emphasize product sense and prioritization.]

**Standout moment**: [One specific memorable observation]

---

## [Role]. [Agent Name]
> ...

[Repeat for each agent]
```

**Voice guidelines:**
- CTO: strategic, focused on technical vision and architectural judgment
- PM: product-oriented, focused on prioritization and user impact
- Team Colleague: practical, focused on collaboration quality and reliability
- Mentor/Mentee: growth-oriented, focused on learning trajectory and curiosity
- Keep each reference concise — the specificity makes it credible, not the length

---

## FAQ Generation

Generate `faq.md` — questions an interviewer might ask, with answers grounded in report data.

### Auto-Extract Questions

Scan all reports for content that naturally maps to common interview concerns:

| Report content | Likely question |
|---------------|----------------|
| Agent collaboration patterns | "How do you use AI tools in your workflow?" |
| Independent decision-making, rejecting agent suggestions | "Can you work independently without AI assistance?" |
| Technology choice reasoning | "How do you choose your tech stack?" |
| Communication style analysis | "What role do you typically play in a team?" |
| Problem-solving under pressure | "How do you handle unexpected production issues?" |
| Scope management observations | "How do you handle changing requirements?" |
| Quality vs. speed tradeoffs | "How do you balance shipping fast with code quality?" |

### Format

```markdown
# FAQ — Based on Agent Observations

## Q: [Question]
**Short answer**: [1-2 sentences]

**Evidence from agents**:
- [Agent/Role]: [Specific observation that supports the answer]
- [Agent/Role]: [Additional supporting evidence]

**Context**: [Any nuance — e.g., "This was more pronounced in project X than project Y"]

---
```

Generate 5-10 questions. Prioritize questions where you have strong evidence. Skip questions where you'd be speculating.

---

## Blog Topics

Generate `blog-topics.md` — topics extracted from sessions where the user engaged in deep discussion, debate, or exploration of a non-trivial question.

These aren't content outlines — they're topic seeds with enough context for the user to decide whether to develop them.

```markdown
# Blog Topic Suggestions

## [Topic Title]
**Source**: [Which sessions/projects this emerged from]
**Core question**: [The underlying question or tension that drove the discussion]
**Why it's interesting**: [What makes this worth writing about — the non-obvious angle]
**Key points discussed**: [Bullet points of the main arguments or insights]

---
```

**Good topics** come from:
- Decisions where the user went against conventional wisdom (and why)
- Deep tradeoff discussions with genuine tension
- Problems that required creative solutions
- Moments where the user changed their mind about something
- Technical or product insights that emerged from iterative exploration

**Skip** topics that are generic or where the user's perspective wouldn't add anything unique.

---

## Resume Text

Generate `introduced-by-agents.md` — a brief, resume-ready section.

```markdown
# Introduced by My Agents

## One-Line Summary
[A single sentence capturing the user's essence as a collaborator, as seen by their AI agents]

## Agent Highlights
[2-3 sentences, each from a different agent-persona, highlighting different strengths]

## At a Glance
- **Work style**: [One-phrase characterization]
- **Strongest signal**: [The single most consistent observation across all agents]
- **Growth trajectory**: [Where they're headed based on observed patterns]

## Full Reports
[Link or reference to the complete report set]
```

This should be tight enough to embed in a portfolio page or LinkedIn section, while being specific enough to be credible. Avoid superlatives and buzzwords — let the specificity do the work.
