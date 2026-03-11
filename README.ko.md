# Introduced by My Agents

**한국어** | [English](./README.md)

AI 에이전트가 실제 협업 데이터를 기반으로 사용자에 대한 객관적인 레퍼런스 리포트를 작성하고, 개인 포트폴리오 사이트를 생성합니다.

## 설치

**사람용** — 이 프롬프트를 AI 에이전트(Claude Code, Cursor 등)에 붙여넣으세요:

```
이 가이드를 따라서 내 포트폴리오를 만들어줘:
https://raw.githubusercontent.com/ohing504/skills/main/docs/guide/getting-started.md
```

**LLM 에이전트용**:

```bash
curl -s https://raw.githubusercontent.com/ohing504/skills/main/docs/guide/getting-started.md
```

또는 스킬 직접 설치:

```bash
npx skills add ohing504/skills --skill agent-reference agent-portfolio
```

## 작동 방식

```
agent-reference  →  레퍼런스 리포트 (사용자 프로필 + 프로젝트 분석)
agent-portfolio  →  포트폴리오 사이트 (GitHub Pages)
```

| 스킬 | 설명 |
|------|------|
| [agent-reference](./skills/agent-reference/) | 세션 히스토리, git, GitHub 분석 → 객관적인 레퍼런스 리포트 |
| [agent-portfolio](./skills/agent-portfolio/) | 리포트 → 개인 Astro 포트폴리오 사이트, GitHub Pages 배포 |

## 빠른 시작

에이전트에게 이렇게 말하세요:

```
"나에 대한 레퍼런스 써줘"          → 리포트 생성
"포트폴리오 사이트 만들어줘"       → 사이트 빌드 & 배포
```

## About

프롬프트/문서 기반 에이전트 스킬 — 빌드 시스템이나 런타임 코드 없음. 설계 철학은 [DESIGN.md](./DESIGN.md) 참고.

## License

MIT
