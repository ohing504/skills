# Introduced by My Agents

**한국어** | [English](./README.md)

AI 에이전트가 사용자와의 실제 협업 경험을 기반으로 객관적인 레퍼런스 체크 리포트를 작성하고, 이를 기반으로 개인 포트폴리오 사이트를 생성하는 [Agent Skills](https://agentskills.io) 모음.

## Available Skills

| 스킬 | 설명 |
|------|------|
| [agent-reference](./skills/agent-reference/) | AI 에이전트가 실제 협업 경험을 기반으로 사용자에 대한 객관적인 레퍼런스 체크 리포트를 작성 |
| [agent-portfolio](./skills/agent-portfolio/) | agent-reference 리포트를 기반으로 개인 포트폴리오 사이트를 생성하여 GitHub Pages에 배포 |

## 작동 방식

```
1. agent-reference  →  리포트 (사용자 프로필 + 프로젝트 리포트)
2. agent-portfolio  →  포트폴리오 사이트 (GitHub Pages)
```

**agent-reference**는 에이전트의 세션 히스토리, git 로그, GitHub 프로필 등을 분석해서 사용자에 대한 레퍼런스 리포트를 생성합니다. **agent-portfolio**는 그 리포트들을 취합해 개인 포트폴리오 사이트를 만들어 GitHub Pages에 배포합니다.

각 스킬은 독립적으로도 사용 가능하고, 함께 사용하면 최대 효과를 얻을 수 있습니다.

## 시작하기

### Step 1: 스킬 설치

```bash
# 두 스킬 모두 설치
npx skills add ohing504/skills --all

# 또는 개별 설치
npx skills add ohing504/skills --skill agent-reference
npx skills add ohing504/skills --skill agent-portfolio
```

### Step 2: 리포트 생성 (agent-reference)

에이전트에게 레퍼런스 리포트를 요청합니다:

```
"나에 대한 레퍼런스 써줘"
"내 작업 스타일 분석해줘"
"write a reference for me"
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

### Step 3: 포트폴리오 사이트 생성 (agent-portfolio)

리포트가 준비되면 포트폴리오 사이트 생성을 요청합니다:

```
"포트폴리오 사이트 만들어줘"
"사이트 배포해줘"
"build my portfolio from the reports"
```

에이전트가 리포트를 분석하여 디자인 컨셉을 제안하고, Astro 사이트를 생성하여 GitHub Pages에 배포합니다.

**6가지 디자인 컨셉** — AI가 리포트 분석 결과에 맞는 컨셉을 자동 제안:

| 컨셉 | 적합한 유형 |
|------|------------|
| Blueprint | 체계적인 아키텍트 |
| Launchpad | 빠른 반복, 다양한 프로젝트 |
| Chronicle | 깊은 도메인 전문가 |
| Spectrum | 풀스택 제너럴리스트 |
| Terminal | CLI/인프라 빌더 |
| Canvas | 크리에이티브/디자인 |

### Step 4: 지속적 업데이트

새로운 프로젝트나 에이전트가 생기면:

1. `agent-reference`로 새 리포트 생성
2. 포트폴리오 repo의 `reports/`에 추가
3. Push → GitHub Actions가 자동으로 사이트 재빌드

## About

각 스킬은 `skills/` 아래 독립된 폴더로 구성되며, `SKILL.md` 파일과 선택적 참조 문서를 포함합니다. 프롬프트/문서 기반 — 빌드 시스템이나 런타임 코드 없음.

설계 철학과 결정 기록은 [DESIGN.md](./DESIGN.md)를 참고하세요.

## License

MIT
