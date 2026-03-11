# User Profile Report Template

This template guides per-agent analysis of the user as a person — characteristics that are consistent across projects.

The template is intentionally loose. Include sections where you have genuine observations, skip sections where you don't, and add sections for anything notable that doesn't fit the categories below. Your unique perspective as a specific agent in a specific context is what makes this report valuable.

Write in the language of the current session.

---

## Report Format

```markdown
# User Profile Report

## Analysis Context

- **Agent**: [Your name/identifier — e.g., "Claude Code", "Claude Projects", "Cursor"]
- **Sessions analyzed**: [Number of sessions you're drawing from]
- **Period**: [Approximate date range]
- **Primary interaction type**: [Architecture discussion, implementation, debugging, brainstorming, etc.]
- **Confidence**: [Low / Medium / High]
  - Low: <5 sessions or narrow context
  - Medium: 5-20 sessions with moderate variety
  - High: 20+ sessions across diverse tasks

---

## Agent Introduction

Briefly introduce yourself and your working relationship with this user. What kind of work did you do together? What was the typical dynamic?

This sets the stage for the reader to understand the lens through which you're observing.

---

## Work Style

How they approach problems, make decisions, and build things. Refer to `analysis-dimensions.md` for specific dimensions to consider.

Include concrete examples. Instead of "prefers clean code", describe the specific patterns you observed — what they refactored, what they left alone, what standards they applied.

---

## Thinking Process

How they reason about decisions and manage tradeoffs. What do they optimize for? How do they handle uncertainty?

---

## Communication Style

How they interact with you (and by extension, how they likely collaborate with others). What's their feedback style? How do they respond to suggestions? What happens when things go wrong?

---

## Philosophy & Values

Principles that emerge implicitly from their choices — not what they say they value, but what their actions consistently reveal.

---

## GitHub Activity Patterns

> Include this section if GitHub data was collected via `gh` CLI. See `github-analysis-guide.md`. If GitHub data is unavailable, skip this section.

- **Contribution rhythm**: Daily patterns, streaks, consistency (from contribution calendar)
- **Project breadth**: Active repos, language diversity, domain range
- **Collaboration signals**: PR review patterns, issue management, open source engagement
- **Work cadence**: Peak days/times, burst vs. steady patterns

---

## Strengths

List with specific evidence. Each strength should be grounded in something you actually observed, not a general impression.

**Format suggestion:**
- **[Strength]**: [Evidence — specific decision, pattern, or moment that demonstrates it]

---

## Areas for Growth

List with specific evidence. Be honest but proportionate — don't exaggerate minor issues into major weaknesses, and don't euphemize genuine gaps.

**Format suggestion:**
- **[Area]**: [What you observed, and why it matters]

---

## Memorable Moments

Specific instances that stood out — a particularly elegant solution, a surprising decision, a moment of insight, or a notable struggle. These make the report vivid and credible.

---

## Summary

A brief paragraph that captures the essence of this person as a collaborator, as you experienced them. If a hiring manager read only this paragraph, what would they need to know?

---

## Review History

> This section is added after the user reviews the report and provides feedback.
> If the user does not request changes, this section can be omitted.

| Date | Feedback | Changes Made |
|------|----------|-------------|
| [date] | [Brief summary of what the user pointed out] | [What was changed in response] |
```

---

## Notes for the Analyzing Agent

- **Write from your perspective.** You are one agent among potentially many. Your report will be combined with reports from other agents who worked with this user in different contexts. Lean into what makes your perspective unique.
- **Evidence over judgment.** "Rewrote the entire state management layer after discovering a race condition, choosing a simpler architecture that eliminated the class of bug entirely" is better than "great architect."
- **Don't fabricate.** If you lack data on a dimension, omit it. A shorter, honest report is far more valuable than a padded one.
- **Proportional reporting.** If 90% of your interactions were positive, the report should reflect that ratio — not force a 50/50 balance.
