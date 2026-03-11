# Skill Review — Remaining Items

> skill-creator 리뷰에서 나온 개선 항목 중 아직 미처리된 것들. HIGH는 모두 해결됨.

**리뷰 날짜**: 2026-03-11
**리뷰어**: skill-creator (critic agent)
**상태**: HIGH 5건 완료, MEDIUM 1건(멀티에이전트) 완료

---

## MEDIUM 우선순위

### M2. Phase 2 블로그 토픽 데이터 흐름 불명확
- **스킬**: agent-reference
- **파일**: `references/persona-perspectives.md`
- **문제**: 블로그 토픽 추출이 "세션에서 deep discussion 찾기"로 안내되어 있는데, Phase 2는 리포트 파일 기반으로 동작함. 세션 데이터에 직접 접근하지 않음.
- **수정**: Phase 2에서 블로그 토픽은 Phase 1 리포트에 포함된 기술 토론 내용에서 추출한다고 명확히 기술

### M3. 빈 데이터/최소 데이터 처리 가이드 없음
- **스킬**: agent-reference
- **파일**: `SKILL.md` Step 2 (Assess Scope)
- **문제**: `~/.claude/projects/`가 비어있고, git 히스토리도 최소이며, `gh`도 없을 때의 처리 방법 없음
- **수정**: "데이터가 매우 부족한 경우 (세션 <3, 커밋 <10), 리포트 생성을 사양하고 더 많은 작업 후 재시도를 권장하는 것이 적절" 같은 가이드 추가

### M4. 포트폴리오 업데이트 워크플로 불완전
- **스킬**: agent-portfolio
- **파일**: `SKILL.md` "Updating the Site" 섹션
- **문제**: "run this skill again"이 Step 3(Scaffold)를 재실행하면 기존 커스텀(Step 4)을 덮어쓸 위험
- **수정**: 업데이트 시에는 Step 1(보고서 복사)과 데이터 파싱만 재실행하고, 컴포넌트/테마는 유지하도록 명시. 또는 "incremental update" 플로우 분리

### M5. `parse-reports.ts` 타입 정의 부재
- **스킬**: agent-portfolio
- **파일**: `references/site-structure-guide.md`
- **문제**: 5개 함수 시그니처만 있고 `ReportSet`, `ParsedReport`, `DimensionScores`, `ProjectSummary`, `AgentPersona` 타입 정의 없음
- **수정**: 각 타입의 인터페이스 정의 추가 (최소한 주요 필드와 optional 표시)

### M6. `[...slug].astro` 동적 라우트 미명세
- **스킬**: agent-portfolio
- **파일**: `references/site-structure-guide.md`
- **문제**: 디렉토리 트리에 `src/pages/raw-data/[...slug].astro`가 있지만, slug 생성 방식, 페이지 렌더링, 마크다운→HTML 변환 방법 미정의
- **수정**: `getStaticPaths()` 예시, slug 파생 규칙 (리포트 파일명 기반), 렌더링 스펙 추가

### M7. `aggregated-project-[name].md` 포트폴리오 입력에 미포함
- **스킬**: agent-portfolio
- **파일**: `SKILL.md` Step 1 (Organize Reports)
- **문제**: agent-reference Phase 2가 생성하는 `aggregated-project-[name].md`가 포트폴리오의 aggregated 디렉토리 목록에 없음. Projects.astro의 데이터 소스로 활용 가능
- **수정**: `reports/aggregated/` 목록에 `aggregated-project-*.md` 추가, Projects.astro 스펙에서 데이터 소스로 명시

---

## LOW 우선순위

### L1. 리포트 크기/길이 가이드라인 없음
- **스킬**: agent-reference
- **문제**: 200+ 세션, 12+ 프로젝트 분석 시 리포트가 과도하게 길어질 수 있음
- **수정**: 사용자 프로필 리포트 권장 길이 (1500-3000단어), 프로젝트 리포트 (500-1500단어) 등 대략적 가이드 추가

### L2. 리포트 버전 관리 가이드 없음
- **스킬**: agent-reference
- **문제**: Phase 1을 시간차를 두고 여러 번 실행할 때, 이전 리포트를 덮어쓸지 새 파일로 만들지 안내 없음
- **수정**: 날짜 기반 디렉토리이므로 자연스럽게 버전 관리됨을 명시. 같은 날 재실행 시 덮어쓰기 허용

### L3. 한국어 트리거 구문 효과 미검증
- **스킬**: agent-reference
- **문제**: description에 포함된 "나에 대한 레퍼런스 써줘" 등이 실제 트리거링 정확도에 미치는 영향 미검증
- **수정**: skill-creator의 `run_loop.py` description 최적화 루프로 실험적 검증 필요

### L4. Astro scaffold 명령어 미세 불일치
- **스킬**: agent-portfolio
- **파일**: `references/site-structure-guide.md` vs `SKILL.md`
- **문제**: SKILL.md는 `--no-install`, `--yes` 플래그 사용, site-structure-guide.md는 미사용
- **수정**: site-structure-guide.md의 scaffold 명령어를 SKILL.md와 동일하게 맞춤

### L5. 에러 복구 가이드 없음
- **스킬**: agent-portfolio
- **문제**: `npm create astro@latest` 실패 (Node 버전, 네트워크), `npx astro add tailwind` 실패 시 대처 방법 없음
- **수정**: 일반적인 트러블슈팅 (Node 18+ 필수, `--verbose` 플래그, 수동 설치 fallback) 추가

### L6. 이미지 최적화/폴백 가이드 없음
- **스킬**: agent-portfolio
- **문제**: `assets/profile.jpg` 등 이미지 관련 사이즈, 최적화, 이미지 없을 때 폴백 렌더링 안내 없음
- **수정**: Astro의 `<Image>` 컴포넌트 사용 권장, 이미지 없을 시 이니셜/아바타 폴백 명시

### L7. SEO/Open Graph 메타 태그 구체 명세 없음
- **스킬**: agent-portfolio
- **문제**: Layout.astro에 "Open Graph tags" 언급은 있지만 구체 속성(og:title, og:description, og:image) 미정의
- **수정**: 최소 OG 태그 목록과 데이터 소스 (aggregated-profile에서 파생) 명시

### L8. CSS-only radar chart 해석 모호
- **스킬**: agent-portfolio
- **파일**: `references/site-structure-guide.md`
- **문제**: "CSS-only radar chart (no JS library needed for static)"라 했지만, 데이터 기반 동적 레이더 차트는 CSS만으로 구현하기 매우 어려움
- **수정**: "SVG-based radar chart (inline, no JS runtime needed)" 으로 변경하는 것이 현실적

---

## 참고: 이미 해결된 항목

| # | 항목 | 커밋 |
|---|------|------|
| H1 | Description ~190→~100단어 최적화 | `2d27714` |
| H2 | github-analysis-guide.md 스테일 Phase 3 참조 제거 | `2d27714` |
| H3 | 리포트 파일명 규칙 통일 | `2d27714` |
| H4 | `references-multi-perspective.md` 포트폴리오 입력 추가 | `2d27714` |
| H5 | Option A/B 라벨 정리 | `2d27714` |
| M1 | 비-Claude-Code 에이전트 데이터 소스 가이드 | `b0a0f5d` |
