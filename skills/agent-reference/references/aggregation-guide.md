# Aggregation Guide

How to combine individual agent reports into unified outputs. This phase synthesizes multiple perspectives into a coherent whole while preserving the distinctiveness of each agent's observations.

---

## Input

Collect all Phase 1 report files:
- `user-profile-[agent]-[date].md` — one per agent
- `project-[name]-[agent]-[date].md` — one per project per agent

## Process

### 1. Inventory and Assess

List all reports with their metadata:

| Agent | Type | Confidence | Sessions | Period | Primary Focus |
|-------|------|-----------|----------|--------|---------------|
| [name] | User Profile | [H/M/L] | [n] | [range] | [type of work] |
| [name] | Project: X | [H/M/L] | [n] | [range] | [type of work] |

Note coverage gaps: are there projects with only one agent's perspective? Agents with very low confidence? Time periods with sparse data?

### 2. Cross-Reference Observations

For each analysis dimension (work style, thinking, communication, values, technical):

1. **Identify convergence**: What do multiple agents agree on? Convergent observations from independent agents are the strongest signals.
2. **Identify unique observations**: What did only one agent see? These aren't less valid — they may reflect that agent's unique vantage point.
3. **Identify contradictions**: Where do agents disagree? Investigate whether the contradiction reflects:
   - Different contexts (brainstorming vs. implementation bring out different behaviors)
   - Growth over time (earlier reports may differ from later ones)
   - Genuine inconsistency (worth noting in the report)

### 3. Weight by Confidence

Not all reports carry equal weight:

| Factor | Higher weight | Lower weight |
|--------|--------------|-------------|
| Sessions analyzed | More sessions | Fewer sessions |
| Stated confidence | High | Low |
| Specificity of evidence | Concrete examples | General impressions |
| Recency | More recent | Older |
| Relevance | Matches the dimension being assessed | Tangential context |

Don't apply this mechanically — use judgment. A low-confidence report with one brilliant specific observation can be more valuable than a high-confidence report full of generalities.

### 4. Synthesize

Write unified reports that:
- Lead with the strongest, most-corroborated observations
- Attribute unique perspectives to their source agent
- Note where agents saw different facets of the same trait
- Flag contradictions honestly rather than papering over them
- Maintain the evidence-based standard — don't let synthesis drift into unsupported generalizations

---

## Output: Aggregated Profile

Combine user profile reports into `aggregated-profile.md`:

```markdown
# Aggregated User Profile

## Sources
[Table of contributing agents, their confidence levels, and session counts]

## Overview
[2-3 paragraph synthesis — the "executive summary" of who this person is as a collaborator]

## Work Style
[Synthesized from all agents, with attribution for unique observations]

## Thinking & Decision-Making
[Synthesized]

## Communication & Collaboration
[Synthesized]

## Philosophy & Values
[Synthesized]

## Technical Profile
[Synthesized, noting stack breadth observed across agents]

## Strengths
[Consolidated list with strongest evidence, noting which agents corroborate]

## Areas for Growth
[Consolidated with proportional representation]

## Across Agents
[What's consistent across all agents? What varies by context?]
```

## Output: Aggregated Project Reports

For each project with reports from multiple agents, combine into `aggregated-project-[name].md`:

```markdown
# Aggregated Project Report: [Name]

## Sources
[Contributing agents and their contexts]

## Project Overview
[Synthesized understanding — different agents may have seen different aspects]

## User's Contributions
[Combined view of what the user did]

## Technical Decisions
[Merged decision table with richer context from multiple perspectives]

## Key Observations
[Strongest observations from any agent, with attribution]

## Summary
[Unified project summary]
```

For projects with only one agent's report, use that report as-is (noting it's single-source).

---

## Handling Edge Cases

**Single agent, multiple projects**: You can still aggregate project reports into a useful unified profile. The cross-project patterns are valuable even from one perspective.

**Many agents, shallow context**: Multiple low-confidence reports can aggregate into medium-confidence synthesis if they converge. Note this in the output.

**Brainstorming-only sessions**: Sessions without code or project work still contribute to user profile analysis (thinking style, communication, values). They just won't produce project reports.
