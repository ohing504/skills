# Concept Generation Guide

This guide helps the agent analyze agent-reference reports and propose a personalized design concept for the portfolio site.

## Step 1: Analyze Reports

Read all reports in the `reports/` directory. Extract personality signals:

### Signal Categories

**Work Style Signals:**
- Problem approach: top-down architect vs. bottom-up builder
- Pace: methodical/thorough vs. fast iteration/MVP-first
- Scope: deep domain specialist vs. broad generalist
- Documentation: heavy documenter vs. code-speaks-for-itself

**Communication Signals:**
- Directive style: concise commands vs. collaborative discussion
- Feedback: specific/detailed vs. high-level direction
- Agent interaction: treats as tool vs. treats as thinking partner

**Technical Identity:**
- Primary role: architect, full-stack builder, frontend specialist, backend/infra, data
- Project diversity: many small projects vs. few large ones
- Tech stack breadth: polyglot vs. focused stack

**Values Signals:**
- Quality vs. speed orientation
- User empathy level
- Open source engagement
- Learning trajectory

## Step 2: Map to Concept

Based on dominant signals, propose one of these concepts (or blend elements):

| Concept | Dominant Signals | Color Direction | Typography | Layout |
|---------|-----------------|-----------------|------------|--------|
| **Blueprint** | Methodical, architecture-first, documentation-heavy | Cool blues (#1e3a5f), slate grays, white | Monospace headers (JetBrains Mono), clean serif body (Source Serif) | Structured grid, generous whitespace, technical diagrams feel |
| **Launchpad** | Fast iteration, many projects, diverse stack | Vibrant warm (#e85d04, #f77f00), accent yellow | Bold sans-serif (Inter 700), compact body (Inter 400) | Project gallery grid, dynamic cards with hover effects, badges |
| **Chronicle** | Deep domain expertise, few large projects | Muted earth tones (#5c4033, #8b7355), cream backgrounds | Elegant serif (Lora), small caps headers | Long-form case studies, timeline navigation, editorial feel |
| **Spectrum** | Full-stack generalist, balanced breadth and depth | Multi-accent gradient, teal to purple (#0d9488 → #7c3aed) | Modern geometric sans (Plus Jakarta Sans) | Balanced sections, skill matrix visualization, even spacing |
| **Terminal** | CLI-oriented, infrastructure/backend, tooling builder | Dark background (#0f172a), green (#22c55e) accents, monospace | Full monospace (Fira Code), terminal-style | Dark theme, code-block aesthetic, minimal decoration |
| **Canvas** | Creative problem-solver, UI/UX focused, design-aware | Soft pastels, one strong accent (#6366f1) | Rounded sans (Nunito), playful but readable | Asymmetric layout, visual emphasis, illustration-friendly |

## Step 3: Propose to User

Present the concept for approval in this format:

```
Based on your agent-reference reports, here's what stands out:
- [2-3 dominant trait observations with evidence]

I propose the **[Concept Name]** concept for your portfolio:

**Rationale:** [1-2 sentences explaining why this concept fits]

**Color palette:**
- Primary: [hex] — [name/purpose]
- Secondary: [hex]
- Accent: [hex]
- Background: [hex]
- Text: [hex]

**Typography:**
- Headings: [font name + weight]
- Body: [font name + weight]

**Section emphasis:** [which sections get the most visual weight and why]

**Section order:** [recommended order based on what's strongest in the reports]

Does this feel right? I can adjust the concept, try a different direction, or blend elements from multiple concepts.
```

## Blending Concepts

Users often don't fit neatly into one category. Common blends:
- Blueprint + Terminal → "Technical Blueprint" (structured but dark-themed)
- Launchpad + Spectrum → "Showcase" (dynamic with broad coverage)
- Chronicle + Canvas → "Story" (editorial with visual flair)

When blending, pick the primary concept for layout and the secondary for color/mood.

## Adjusting After Feedback

If the user says "make it more [X]":
- "more professional" → shift toward Blueprint or Chronicle
- "more fun/creative" → shift toward Canvas or Launchpad
- "more minimal" → reduce decoration, increase whitespace, limit colors
- "darker" → apply dark variant of any concept (dark bg, light text, adjusted accents)

## Notes

- Google Fonts only (free, no licensing issues for GitHub Pages)
- All concepts must work with Tailwind CSS utility classes
- Every concept must be responsive (mobile-first)
- Ensure sufficient color contrast (WCAG AA minimum)
- The concept affects visual styling, not content — all sections are available regardless of concept
