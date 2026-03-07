# Analysis Dimensions

This guide defines what to observe when analyzing a user. These are prompts for reflection, not a rigid checklist — include what you genuinely observed, skip what you didn't, and add anything notable that falls outside these categories.

Ground every observation in specific evidence: a decision they made, a phrase they used, a pattern across sessions. Avoid generic statements that could apply to anyone.

---

## Work Style

How they approach problems and build things.

- **Problem-solving approach**: Top-down (architecture first) vs. bottom-up (prototype first)? Do they plan extensively or iterate rapidly?
- **MVP mindset**: Do they scope aggressively to ship fast, or do they build toward completeness before releasing?
- **Build vs. refactor balance**: When do they choose quick implementation vs. taking time to restructure? What triggers a refactor?
- **Dependency decisions**: How do they choose libraries and tools? Do they prefer minimal dependencies, or leverage ecosystem solutions?
- **Documentation habits**: Do they document as they go, after the fact, or rarely? What do they consider worth documenting?
- **Error handling philosophy**: Do they handle edge cases preemptively or address them as they arise?

## Thinking Process

How they reason about decisions and tradeoffs.

- **Priority framework**: What do they optimize for — cost, user experience, technical elegance, speed to market, maintainability?
- **Tradeoff resolution**: When facing competing concerns, how do they decide? Do they seek more information, go with intuition, or apply a consistent principle?
- **Market/competitive awareness**: How deeply do they consider the broader landscape when making product decisions?
- **Abstraction preferences**: Do they favor concrete, explicit code or do they reach for abstractions early? How do they decide when an abstraction is warranted?
- **Scope management**: How do they handle scope creep — in their own ideas and in external requests?

## Communication

How they interact with agents and would likely interact with teammates.

- **Feedback style**: Do they give specific, detailed feedback or broad directional guidance? Do they explain the "why" behind requests?
- **Response to suggestions**: When the agent proposes an approach, do they accept readily, push back with reasoning, modify and improve, or sometimes ignore?
- **Behavior when stuck**: Do they methodically debug, try different approaches rapidly, step back to rethink the problem, or ask for help early?
- **Instruction clarity**: Are their requests precise and unambiguous, or do they rely on shared context and expect the agent to fill in gaps?
- **Iteration pattern**: Do they review output carefully and give detailed feedback, or accept and move on quickly?

## Philosophy & Values

Principles that emerge implicitly from their choices.

- **Recurring themes**: What principles come up repeatedly in their decisions — across different projects and contexts?
- **Technical values**: What do their technology choices reveal about what they care about (simplicity, performance, developer experience, user privacy, etc.)?
- **User empathy**: How do they think about end users? Do they naturally consider accessibility, edge cases, or diverse use cases?
- **Quality bar**: Where do they draw the line between "good enough" and "needs more work"?
- **Learning orientation**: Do they seek to understand deeply, or focus on practical application? How do they react to unfamiliar territory?

## Technical Capability

Observable skills and knowledge depth — report what you've actually seen, not what you assume.

- **Architecture**: How do they structure systems? Do they think in terms of boundaries, interfaces, and data flow?
- **Debugging approach**: How do they isolate issues — systematic narrowing, hypothesis-driven, or intuitive leaps?
- **Code quality signals**: Naming conventions, code organization, testing habits, commit granularity
- **Stack breadth**: What areas have you seen them work in — frontend, backend, infrastructure, data, design?
- **Tool fluency**: How effectively do they use their development tools, including AI agents?

---

## Guidance for Analysis

**Be concrete, not generic.** "Good problem solver" is useless. "Consistently breaks large problems into independent subtasks before writing any code, as seen when building the authentication system and the data pipeline" is valuable.

**Report asymmetry honestly.** If you observed 20 instances of strong architecture decisions and 2 instances of rushing through testing — report both with those proportions. Don't artificially balance strengths and weaknesses.

**Distinguish frequency from severity.** A habit they show every session matters differently than something that happened once. Note the pattern, not just the instance.

**Note what surprised you.** The most interesting observations are often the unexpected ones — moments where the user did something you wouldn't have predicted based on their general pattern.

**Separate user behavior from agent behavior.** When analyzing session data, remember that tool call statistics reflect what the agent executed, not what the user did directly. The user's work style is revealed by the *kinds of tasks they delegate*, the *order of their requests*, and *how they respond to results* — not by raw tool counts. Frame observations in terms of what the user directed: "asks the agent to verify before committing" rather than "Bash was called 12,007 times."

**Respect data boundaries.** Your analysis is limited to what exists on the current machine. A project that appears small or early-stage may have extensive history on another machine, in another agent, or predating the available logs. When you lack full context on a project, say so rather than guessing its maturity.
