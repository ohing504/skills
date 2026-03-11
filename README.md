# Introduced by My Agents

AI 에이전트가 사용자와의 실제 협업 경험을 기반으로 객관적인 레퍼런스 체크 리포트를 작성하고, 이를 기반으로 개인 포트폴리오 사이트를 생성하는 [Agent Skills](https://agentskills.io) 모음.

## Available Skills

| Skill | Description |
|-------|-------------|
| [agent-reference](./skills/agent-reference/) | AI agents write objective reference check reports about a user based on real collaboration experience |
| [agent-portfolio](./skills/agent-portfolio/) | Generate a personalized portfolio site from agent-reference reports, deployed to GitHub Pages |

## How It Works

```
1. agent-reference  →  Reports (user profile + project reports)
2. agent-portfolio  →  Portfolio site on GitHub Pages
```

**agent-reference**는 에이전트의 세션 히스토리, git 로그, GitHub 프로필 등을 분석해서 사용자에 대한 레퍼런스 리포트를 생성합니다. **agent-portfolio**는 그 리포트들을 취합해 개인 포트폴리오 사이트를 만들어 GitHub Pages에 배포합니다.

각 스킬은 독립적으로도 사용 가능하고, 함께 사용하면 최대 효과를 얻을 수 있습니다.

## Getting Started

### Step 1: Install Skills

```bash
# 두 스킬 모두 설치
npx skills add ohing504/skills --all

# 또는 개별 설치
npx skills add ohing504/skills --skill agent-reference
npx skills add ohing504/skills --skill agent-portfolio
```

### Step 2: Generate Reports (agent-reference)

에이전트에게 레퍼런스 리포트를 요청합니다:

```
"나에 대한 레퍼런스 써줘"
"write a reference for me"
"analyze my work patterns"
```

에이전트가 세션 히스토리, git, GitHub 데이터를 분석한 뒤, 리포트를 아래 구조로 생성합니다:

```
reports/
├── 2026-03-claude-code/
│   ├── user-profile.md
│   ├── project-financial.md
│   └── project-agentfiles.md
└── 2026-09-cursor/
    ├── user-profile.md
    └── project-newproject.md
```

여러 에이전트(Claude Code, Cursor 등)에서 각각 Phase 1을 실행하면, 다양한 관점의 리포트를 모을 수 있습니다. Phase 2(aggregate)로 통합 리포트를 생성할 수도 있습니다.

### Step 3: Build Portfolio Site (agent-portfolio)

리포트가 준비되면 포트폴리오 사이트 생성을 요청합니다:

```
"포트폴리오 사이트 만들어줘"
"build my portfolio from the reports"
"deploy to github pages"
```

에이전트가 리포트를 분석하여 디자인 컨셉을 제안하고, Astro 사이트를 생성하여 GitHub Pages에 배포합니다.

**6가지 디자인 컨셉** — AI가 리포트 분석 결과에 맞는 컨셉을 자동 제안:

| Concept | Best for |
|---------|----------|
| Blueprint | 체계적인 아키텍트 |
| Launchpad | 빠른 반복, 다양한 프로젝트 |
| Chronicle | 깊은 도메인 전문가 |
| Spectrum | 풀스택 제너럴리스트 |
| Terminal | CLI/인프라 빌더 |
| Canvas | 크리에이티브/디자인 |

### Step 4: Keep It Updated

새로운 프로젝트나 에이전트가 생기면:

1. `agent-reference`로 새 리포트 생성
2. 포트폴리오 repo의 `reports/`에 추가
3. Push → GitHub Actions가 자동으로 사이트 재빌드

## About

Each skill is a self-contained folder under `skills/` with a `SKILL.md` file and optional reference documents. Prompt/document-based — no build system, no runtime code.

See [DESIGN.md](./DESIGN.md) for the design philosophy and decisions log.

## License

MIT
