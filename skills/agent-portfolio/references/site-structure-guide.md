# Site Structure Guide

This guide defines the Astro project structure, component specifications, and data processing for the agent-portfolio site.

## Project Scaffold

When scaffolding, use:
```bash
npm create astro@latest -- --template minimal --no-install
npm install
npm install tailwindcss @tailwindcss/vite
```

> **Warning:** Do NOT use `npx astro add tailwind` — it installs `@astrojs/tailwind` (Tailwind 3 integration) which conflicts with Tailwind 4. Install `@tailwindcss/vite` directly instead.

### Directory Structure

```
{portfolio-repo}/
├── src/
│   ├── layouts/
│   │   └── Layout.astro              # Base layout: meta tags, fonts, footer
│   ├── pages/
│   │   ├── index.astro                # Main single-page site
│   │   └── reports/
│   │       └── [...slug].astro        # Dynamic pages for raw report viewing
│   ├── components/
│   │   ├── Hero.astro                 # Hero section
│   │   ├── About.astro                # About/resume section
│   │   ├── Career.astro               # Career timeline section
│   │   ├── AgentReviews.astro         # Agent persona review cards
│   │   ├── Projects.astro             # Project highlight cards
│   │   ├── WorkStyle.astro            # Work style radar/visualization
│   │   ├── GitHubActivity.astro       # GitHub stats and calendar
│   │   ├── FAQ.astro                  # Interviewer FAQ accordion
│   │   ├── BlogTopics.astro           # Blog topic seeds
│   │   ├── RawData.astro              # Links to full reports
│   │   └── Footer.astro               # Credit footer
│   ├── lib/
│   │   └── parse-reports.ts           # Report markdown → structured data
│   └── styles/
│       └── global.css                 # Tailwind base + concept tokens
├── reports/                           # Raw report files (from agent-reference)
├── materials/                          # User-provided materials
│   ├── resume.pdf                     # Resume/CV (any format)
│   └── profile.jpg                    # Profile photo
├── public/
│   └── favicon.svg
├── astro.config.mjs
├── tailwind.config.mjs          # Optional: Tailwind 4 also supports CSS @theme
├── tsconfig.json
└── package.json
```

## Component Specifications

### Layout.astro
- HTML head: charset, viewport, title, description meta, Open Graph tags
- Google Fonts link (based on concept)
- **Global CSS import** — `import '../styles/global.css';` in the frontmatter. Without this, Tailwind classes won't work.
- Slot for page content
- Footer component at bottom

```astro
---
import '../styles/global.css';

interface Props {
  title: string;
  description?: string;
}
---
```

### Hero.astro
**Data needed:** `introduced-by-agents.md` one-line summary OR first report's summary section
**Required:** Yes
**Variants:**
- Full-width with large heading + subtitle
- Agent quote carousel (if multiple agents)
**Content:**
- User's name or handle
- One-line summary from aggregated report
- 1-2 agent quotes as testimonials

### About.astro
**Data needed:** Resume file from `materials/` (any format — PDF, Markdown, text)
**Required:** No (only if resume provided)
**Layout:** Two-column — main content (left) + sidebar (right)
**Content:**
- **Main (left):**
  - Self-introduction / summary from resume header (who they are, what they value)
  - Skills/tech stack badges
  - Can include profile image if provided in `materials/`
- **Sidebar (right) — Career + Education card(s):**
  - Career preview: **2 most recent roles only** (company, role, period — one line each), "+N개 더보기" count, "전체보기 →" link to `/career` page. Do NOT list all career entries.
  - Education: school name, degree, period — compact list in the same sidebar area
  - Career and Education can be one combined card or two stacked cards, but must stay in the sidebar — do NOT give Career its own full-width section on the landing page.

### /career page (separate page)
**Full career timeline page** at `src/pages/career.astro`:
- Complete career timeline: company name, period, role, one-line description
- Key achievements per role (1-2 bullet points, quantified where possible)
- Tech stack badges per role
- Visual timeline layout (vertical, most recent first)
- Education section at the bottom

**Connecting career to agent reports:**
- If a career project overlaps with an agent-reference project report (e.g., same app name, same tech stack), link them visually
- Career section shows "what they did professionally", agent reports show "how they actually work" — the combination is the portfolio's unique value
- Don't duplicate project details — Career shows company context, Projects.astro shows agent-observed depth

### AgentReviews.astro
**Data needed:** Multi-perspective references from persona-perspectives output, or individual user-profile reports
**Required:** Yes
**Variants:**
- Card grid (2-3 columns)
- Each card: agent persona name + role (e.g., "CTO. Claude Code"), relationship summary, 2-3 key observations, standout moment quote
**Content:**
- One card per agent persona
- Role badge (CTO, PM, Teammate, etc.)
- Confidence indicator (High/Medium/Low)

### Projects.astro
**Data needed:** Project report files
**Required:** Yes
**Variants:**
- Card grid with tech stack badges
- Featured project (largest/most confident) gets expanded view
**Content per card:**
- Project name + one-line description
- Tech stack badges
- Agent's observation of user's role
- Key contribution highlight
- Commit count / session count (if available)

### WorkStyle.astro
**Data needed:** Analysis dimensions from user profile reports
**Required:** Yes
**Visualization options:**
- CSS-only radar chart (no JS library needed for static)
- Bar chart with labeled dimensions
- Styled list with progress bars
**Dimensions (from analysis-dimensions.md):**
- Work Style (approach pattern)
- Thinking Process (decision-making)
- Communication (collaboration style)
- Philosophy & Values
- Technical Capability

### GitHubActivity.astro
**Data needed:** GitHub API data collected in Step 1
**Required:** No
**Content:**
- Total contributions (last year)
- Language distribution (horizontal bar chart, CSS-only)
- Active repos count
- Longest streak
- Peak activity day

### FAQ.astro
**Data needed:** `faq.md` from aggregated reports
**Required:** No
**Content:**
- Accordion-style Q&A
- Each answer cites agent evidence
- 5-10 questions

### BlogTopics.astro
**Data needed:** `blog-topics.md` from aggregated reports
**Required:** No
**Content:**
- Topic cards with title, source project, core question
- Compact layout (these are seeds, not full posts)

### RawData.astro
**Data needed:** Report files the user chose to make public
**Required:** No
**Content:**
- List of report files with links to full text
- Each links to `/reports/{slug}` dynamic page
- Organized by date/agent

### Footer.astro
**Required:** Yes
**Content:**
```
Introduced by My Agents · Built with @ohing504/skills
Last updated: {build date}
```
- Link to agent-reference skill repo (https://github.com/ohing504/skills)
- Build date auto-generated at build time via `new Date().toISOString().split('T')[0]`

## Data Processing (parse-reports.ts)

### Purpose
Transform raw markdown report files into structured data for Astro components.

### Key Functions

```typescript
// Read all report directories
function discoverReports(reportsDir: string): ReportSet[]

// Parse a single report markdown into structured data
function parseReport(content: string): ParsedReport

// Extract analysis dimensions for WorkStyle chart
function extractDimensions(profiles: ParsedReport[]): DimensionScores

// Extract project summaries for Projects section
function extractProjects(reports: ParsedReport[]): ProjectSummary[]

// Extract agent personas for AgentReviews section
function extractPersonas(reports: ParsedReport[]): AgentPersona[]
```

### Parsing Strategy
- Reports use consistent markdown headers (from agent-reference templates)
- Parse `## Analysis Context` for metadata (agent name, sessions, confidence)
- Parse `## Agent Introduction` for persona voice
- Parse section headers to extract dimensional data
- Handle multiple report sets (different dates/agents) by grouping and merging

## Tailwind Concept Tokens

Tailwind 4 supports two ways to define concept-specific design tokens:

### Option A: CSS `@theme` (Tailwind 4 native)

Define tokens directly in `src/styles/global.css`:

```css
@import "tailwindcss";

@theme {
  --color-primary: #1e3a5f;
  --color-secondary: #64748b;
  --color-accent: #3b82f6;
  --color-surface: #f8fafc;
  --color-text: #0f172a;
  --font-heading: 'JetBrains Mono', monospace;
  --font-body: 'Source Serif 4', serif;
}
```

Use in components: `class="text-primary font-heading bg-surface"`

### Option B: `tailwind.config.mjs` (compatible with Tailwind 4)

```javascript
// Example for Blueprint concept
export default {
  content: ['./src/**/*.{astro,html,js,jsx,md,mdx,svelte,ts,tsx,vue}'],
  theme: {
    extend: {
      colors: {
        primary: '#1e3a5f',
        secondary: '#64748b',
        accent: '#3b82f6',
        surface: '#f8fafc',
        text: '#0f172a',
      },
      fontFamily: {
        heading: ['JetBrains Mono', 'monospace'],
        body: ['Source Serif 4', 'serif'],
      },
    },
  },
}
```

Either approach works. Option A is preferred for Tailwind 4 as it keeps everything in CSS and doesn't require a separate config file.

## Responsive Design

- Mobile-first: single column, stacked sections
- Tablet (md:): 2-column grids for cards
- Desktop (lg:): full layout with sidebar options
- All components must work at 320px minimum width

## Accessibility

- Semantic HTML (main, section, article, nav)
- Skip-to-content link
- WCAG AA color contrast minimum
- Alt text for any images
- Keyboard-navigable accordion (FAQ section)
- prefers-reduced-motion for any animations
