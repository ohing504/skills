# GitHub Profile Analysis Guide

This guide covers how to collect and analyze GitHub data using the `gh` CLI to enrich reference reports with public contribution patterns.

---

## Prerequisites

The `gh` CLI must be authenticated. Check with:
```bash
gh auth status
```

If not available, skip GitHub analysis entirely and note it in the report's Analysis Context.

---

## Step 1: Discover Repositories

### List user's repos

```bash
# All repos (public + private), sorted by recent push
gh repo list --limit 50 --json name,isPrivate,pushedAt,primaryLanguage,description,stargazerCount \
  --jq '.[] | {name, private: .isPrivate, pushed: .pushedAt, lang: .primaryLanguage.name, desc: .description, stars: .stargazerCount}'
```

### Identify active repos

```bash
# Repos pushed to in the last 6 months
gh repo list --limit 50 --json name,isPrivate,pushedAt \
  --jq '[.[] | select(.pushedAt > "2025-09-01")] | sort_by(.pushedAt) | reverse | .[] | "\(.name) (\(if .isPrivate then "private" else "public" end)) - last push: \(.pushedAt)"'
```

### Present choices to the user

Show the active repo list and ask:
- Which repos to include in the analysis
- Whether to include private repos (and at what detail level)
- Any repos to exclude entirely

**Always ask before analyzing.** Never assume a user wants all repos included — especially private ones.

---

## Step 2: Analyze Contribution Patterns

### Overall activity

```bash
# Contribution stats for the authenticated user
gh api graphql -f query='
{
  viewer {
    contributionsCollection {
      totalCommitContributions
      totalPullRequestContributions
      totalIssueContributions
      totalRepositoryContributions
      contributionCalendar {
        totalContributions
        weeks {
          contributionDays {
            contributionCount
            date
            weekday
          }
        }
      }
    }
  }
}'
```

From the contribution calendar, extract:
- **Work cadence**: Daily/weekly patterns, streaks, breaks
- **Peak days**: Which days of the week are most active
- **Consistency**: Steady contributor vs. burst worker
- **Time patterns**: If commit timestamps are available, morning/evening/weekend tendencies

### Per-repo commit activity

```bash
# Recent commits for a specific repo
gh api repos/{owner}/{repo}/stats/commit_activity \
  --jq '.[] | {week: .week, total: .total, days: .days}'

# Commit frequency over time
gh api repos/{owner}/{repo}/stats/participation \
  --jq '{owner: .owner, all: .all}'
```

---

## Step 3: Repository-Level Analysis

For each selected repo:

```bash
# Repo metadata
gh repo view {owner}/{repo} --json name,description,primaryLanguage,languages,isPrivate,createdAt,pushedAt,stargazerCount,forkCount,issues,pullRequests

# Recent PRs by the user
gh pr list --repo {owner}/{repo} --author @me --state all --limit 20 \
  --json title,state,createdAt,mergedAt,additions,deletions,changedFiles

# Recent issues by the user
gh issue list --repo {owner}/{repo} --author @me --state all --limit 20 \
  --json title,state,createdAt,closedAt,labels
```

**What to look for:**
- PR patterns: Size, frequency, self-merge vs. review flow
- Issue management: Bug reports vs. feature requests, how they're described
- Language diversity across repos
- Open source contributions (if any)

---

## Step 4: Privacy Handling for Private Repos

Private repos require careful handling. The user may include them for analysis depth but not want details exposed in reports.

### Privacy levels (let the user choose per repo)

| Level | What appears in reports | Example |
|-------|------------------------|---------|
| **Full** | Repo name, description, tech stack, specific observations | "In the MoneyBoard project (Next.js financial app)..." |
| **Anonymized** | Generic description, tech stack, observations without identifying details | "In a financial web application (Next.js)..." |
| **Stats only** | Contribution counts and patterns, no project details | "Contributed to 3 additional private projects (142 commits)" |
| **Excluded** | Not mentioned at all | — |

### Default behavior

- **Public repos**: Full detail unless user says otherwise
- **Private repos**: Always ask first. Default to "Anonymized" if user says "include but be careful"
- **Forked repos**: Note that it's a fork; focus on the user's contributions, not the original project

### In report text

When referencing anonymized repos:
- Replace repo name with a generic descriptor based on primary language and domain
- Omit URLs, specific file paths, business logic details
- Keep technical patterns and work style observations (these reveal the person, not the project)

---

## Step 5: Synthesize for Reports

### For User Profile Reports

Add to the Work Style section:
- **Contribution rhythm**: How consistently they work (daily committer vs. sprint-based)
- **Project breadth**: How many domains/languages they actively work across
- **Open source engagement**: Contributions to others' projects, public project maintenance
- **Collaboration signals**: PR review patterns, issue discussions

### For Project Reports

Enrich the Git Pattern Analysis section with:
- PR size patterns (small focused PRs vs. large feature branches)
- Time from PR creation to merge
- How they describe their work in PR titles/descriptions
- Issue-to-PR linkage (structured workflow vs. ad-hoc)

### For Landing Page (Phase 3)

Extract visual-ready data:
- Contribution heatmap data (the "잔디"/grass)
- Language distribution chart data
- Active project timeline
- Stats: total commits, PRs, repos, languages

```json
{
  "github_summary": {
    "total_contributions_last_year": 1247,
    "active_repos": 8,
    "languages": ["TypeScript", "Python", "Dart"],
    "longest_streak_days": 45,
    "peak_day": "Wednesday",
    "pr_merge_rate": 0.95,
    "contribution_heatmap": []
  }
}
```

---

## Common Pitfalls

**Don't equate commit count with productivity.** A single well-architected commit can be more valuable than 50 trivial ones. Use commit data as evidence of patterns, not as a scorecard.

**Don't expose private repo contents.** Even in anonymized mode, be careful about unique technical details that could identify a project (unusual tech stack combinations, very specific domain logic).

**Don't over-index on GitHub activity.** Many developers do significant work outside GitHub — local projects, other platforms, pre-git-history work. GitHub data supplements the analysis; it doesn't define it.

**Distinguish personal vs. organizational repos.** Work done in an org repo may have different commit patterns (PR reviews, CI checks, branch protection) than personal projects. Both are valid data points but reveal different things.
