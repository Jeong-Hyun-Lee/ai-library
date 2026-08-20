# AI News Auto Routine — Design

## Purpose

`ai-library` 위키(`docs/superpowers/specs/2026-08-20-ai-news-wiki-design.md`에서 스캐폴딩 완료)에 AI 소식을 매일 자동으로 발굴+ingest하는 루틴을 추가한다. 기존 CLAUDE.md `## 6. AI Library Wiki`의 Ingest 워크플로는 "소스 1개씩 깊게 개입"(사용자와 논의 후 반영)을 전제로 설계되어 있는데, 이번 요구사항은 그와 별개로 **완전 자동** — 사람 개입 없이 발굴부터 위키 반영, 커밋/push까지 전부 자동으로 수행한다.

## Scope

매일 실행되는 자동 루틴 하나를 신설한다: 소스 발굴 → 중복 판단 → 상위 3건 ingest → 위키 갱신 → git 커밋/push. 기존 수동 Ingest 워크플로(`## 6.`)는 그대로 유지 — 이번 작업은 그 옆에 별도의 자동 경로(`## 7.`)를 추가하는 것이지, 대체가 아니다.

## 1. 아키텍처 / 트리거

- **메커니즘**: `schedule` 스킬(클라우드 에이전트, cron 기반) 사용 — 로컬 세션이 켜져 있지 않아도 실행된다.
  - 세션 내 `CronCreate` 툴은 세션 종료 시 소멸하고 7일 후 자동 만료되므로 "매일 자동" 요구사항에 맞지 않아 배제한다.
  - `schedule` 스킬의 실제 실행 환경(로컬 자원 접근, git 인증, 설치된 도구 등)은 브레인스토밍 단계에서 확인할 수 없다 — §4 리스크 참고, 구현 단계에서 확정한다.
- **주기**: 매일 1회, 오후 12시 45분 (KST)
- **cron이 실행할 프롬프트**: 짧게 유지 — "이 저장소(`ai-library`)의 `CLAUDE.md` `## 7. AI 뉴스 자동 루틴` 워크플로를 실행해." 형태. 실제 단계는 CLAUDE.md 한 곳에만 정의해 기존 Ingest 워크플로와 동일한 패턴(단일 진입점)을 유지한다.

## 2. 자동 루틴 워크플로 (CLAUDE.md `## 7.`에 신규 기록)

기존 `## 6.`의 Ingest 워크플로(사용자와 논의)는 그대로 둔다. 이건 별개의 자동 경로:

1. `last30days` 스킴으로 "AI 소식"(모델/논문/툴/트렌드) 검색
2. `wiki/log.md` + `wiki/index.md`를 읽고, 이미 다룬 주제/이벤트와 겹치는 후보를 제외 (LLM이 판단, 별도 DB 없음)
3. 남은 후보 중 상위 3건 선정 (품질/중요도 기준)
4. 각 건마다:
   - `wiki/sources/YYYY-MM-DD-슬러그.md` 작성 — 링크+요약만, **`raw/` 저장 생략**, frontmatter에 `auto: true` 추가
   - 관련 `entities/`, `concepts/` 페이지 갱신 (모순 발견 시 명시)
   - `wiki/timeline/YYYY-MM.md`에 항목 추가
5. `wiki/index.md` 갱신 (Sources 섹션 포함)
6. `wiki/log.md`에 append: `## [YYYY-MM-DD] ingest (auto) | 제목` — 수동 ingest(`## [YYYY-MM-DD] ingest | 제목`)와 `(auto)` 표시로 구분
7. `qmd update && qmd embed` 재실행
8. 커밋(한글, Conventional Commits, 본문 최대 2줄, 작성자 표기 없음) + `git push origin main`까지 자동
9. 그날 후보가 0건이면: **아무것도 기록/커밋하지 않는다** (`log.md`, `index.md`, git 전부 무변경). 실행 여부는 `schedule` 스킬의 자체 실행 기록으로만 확인 가능하다.

## 3. Frontmatter 확장

기존 `type`/`tags`/`created`/`updated`/`sources` 필드에 `auto` 필드를 추가한다:

```yaml
auto: true   # 자동 루틴이 사람 개입 없이 작성한 페이지임을 표시
```

수동 ingest로 작성된 페이지는 이 필드를 생략한다 (기본값은 "수동"으로 취급).

## 4. 범위 밖 / 리스크

- **미검증 항목**: `schedule` 스킬이 실제로 어떤 환경에서 실행되는지(로컬 머신 자원 접근 가능 여부, git push 인증, `qmd`/`npm` 설치 여부, `last30days` 스킴 사용 가능 여부)는 브레인스토밍 단계에서 확인할 수 없다 — 구현 단계에서 `schedule` 스킬을 실제로 호출할 때 확정한다. 클라우드 에이전트 환경에 qmd/git 인증이 없으면 워크플로 7~8단계(임베딩/push)는 별도 처리가 필요할 수 있다.
- 자동 ingest 결과에 대한 사람의 사전 검토는 없다(설계상 의도됨) — 오류가 있는 페이지도 다음 Lint(`## 6.`의 Lint 워크플로) 전까지 위키에 그대로 남아있을 수 있다. 별도의 자동 품질 검증 체계는 이번 범위에 포함하지 않는다.
- 검색 도메인은 `last30days`가 다루는 소스(Reddit/X/YouTube/HN/Polymarket/GitHub/웹)로 한정된다 — 학술지, 기업 공식 블로그 등 별도 소스는 이번 루틴이 다루지 않는다 (수동 Ingest로는 여전히 가능).
- `raw/` 원본 스냅샷을 저장하지 않으므로, 링크된 원본이 나중에 사라지면 `wiki/sources/` 페이지의 요약과 URL만 남는다.
