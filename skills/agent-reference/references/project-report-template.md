# Project Report Template

This template guides per-project analysis — what the user did in a specific project and how they did it.

Generate one report per project you have meaningful context on. If you worked with the user across multiple projects, generate separate reports for each.

Write in the language of the current session.

---

## Report Format

```markdown
# Project Report: [Project Name]

## Analysis Context

- **Agent**: [Your name/identifier]
- **Project**: [Project name]
- **Commits reviewed**: [Number, if you have git access — otherwise "N/A"]
- **Sessions analyzed**: [Number of sessions related to this project]
- **Period**: [Date range of work on this project]
- **Confidence**: [Low / Medium / High]

---

## Project Overview

Describe the project as you understand it — what it does, who it's for, what problem it solves. This should reflect your actual understanding from working on it, not a polished marketing description.

Include the tech stack if relevant.

---

## User's Role & Contributions

What did the user do in this project? Were they the sole developer, architect, one of many contributors? What areas did they own?

---

## Technical Decisions

Key technical choices the user made and the reasoning behind them. Focus on decisions where you observed the deliberation process — not just what was chosen, but why, and what alternatives were considered.

**Format suggestion:**
| Decision | Chosen Approach | Reasoning | Alternatives Considered |
|----------|----------------|-----------|------------------------|
| [What was decided] | [What they chose] | [Why] | [What else was on the table] |

---

## Notable Problem-Solving

Specific instances where the user tackled a challenging problem. Describe the problem, how they approached it, and the outcome. These are the stories that make a project report concrete and credible.

---

## Characteristics Revealed

What did this project specifically reveal about the user that might not be visible elsewhere? Every project brings out different aspects of a person — what emerged here?

---

## Git Pattern Analysis

If you have access to git history, analyze:

- **Commit style**: Message conventions, granularity, how well commits tell the project story
- **Commit frequency**: Patterns in when and how often they commit
- **Code quality signals**: Naming, organization, consistency
- **Dependency choices**: What they reached for vs. built themselves
- **Review patterns**: How they handle their own code review (self-review before commit, squash vs. granular commits)

If you don't have git access, skip this section.

---

## Summary

One paragraph capturing the user's contribution to this project and what it reveals about them as a developer/collaborator.

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

- **One project, one report.** Don't combine multiple projects. The aggregation phase handles cross-project synthesis.
- **Project context matters.** A side project built in a weekend reveals different things than a production system maintained over months. Frame your observations accordingly.
- **Technical decisions are gold.** The most valuable content in a project report is the decision-making process — what was chosen, why, and what was rejected. This is what differentiates a reference check from a code review.
- **Don't evaluate the project itself.** You're analyzing the user's work, not whether the project is good. A well-executed simple project and a messy ambitious project both yield valuable observations.
