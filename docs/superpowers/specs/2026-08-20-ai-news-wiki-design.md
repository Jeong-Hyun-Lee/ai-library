# AI News Wiki — Design

## Purpose

`ai-library`는 AI 관련 소식(모델 출시, 논문, 툴, 트렌드)을 다루는 개인 지식 베이스다. RAG식으로 매번 원본을 다시 뒤지는 대신, LLM(Claude Code)이 소스를 하나씩 읽고 그 내용을 지속적으로 유지·갱신되는 Obsidian 위키로 축적한다. 사람은 소스 수집·질문·방향 제시를 맡고, LLM은 요약·상호참조·정합성 유지를 맡는다.

## Scope

이번 설계는 **위키 골격(scaffolding)**까지만 다룬다: 디렉토리 구조, 페이지 규칙, 워크플로 정의(CLAUDE.md), index/log 포맷, qmd 검색 도구 도입. 실제 소스 ingest(첫 기사 반영 등)는 이 스캐폴딩이 끝난 뒤 별도 작업으로 진행한다.

## 1. 디렉토리 구조

```
raw/                    # 원본 소스, 불변 (LLM이 읽기만 함)
  articles/
  papers/
  tweets/
  videos/
  assets/                # Obsidian Web Clipper로 받은 이미지 등

wiki/                    # LLM이 전적으로 관리
  entities/               # 회사, 모델, 인물 (예: openai.md, gpt-5.md)
  concepts/               # 기술 개념 (예: rag.md, agentic-coding.md)
  sources/                # 소스 1개당 요약 페이지 1개
  comparisons/            # 온디맨드 비교 페이지
  timeline/               # 월별 타임라인 (예: 2026-08.md)
  index.md                # 카탈로그, 카테고리별 링크 + 한줄요약
  log.md                  # append-only 작업 기록
```

## 2. 페이지 규칙 (frontmatter)

Obsidian 호환 — YAML frontmatter + wikilink 사용.

```yaml
---
type: entity | concept | source | comparison | timeline
tags: [company, llm]
created: 2026-08-20
updated: 2026-08-20
sources: ["[[2026-08-20-some-article]]"]
---
```

- `entities/`, `concepts/` 페이지: 본문에 `[[다른 페이지]]` 링크로 상호참조. 새 소스가 반영될 때마다 `updated` 갱신.
- `sources/` 페이지: 원본 raw 파일 경로, 핵심 요약, 이 소스가 건드린 entity/concept 링크 목록.
- 파일명: kebab-case. 소스 페이지는 `YYYY-MM-DD-슬러그.md`.

## 3. 워크플로 (CLAUDE.md에 기록)

**Ingest** (소스 1개씩 깊게 개입하는 방식):
1. `raw/`에 새 파일 확인
2. 읽고 핵심 내용을 사용자와 논의
3. `wiki/sources/YYYY-MM-DD-슬러그.md` 요약 페이지 작성
4. 관련 `entities/`, `concepts/` 페이지 갱신 (신규 생성 또는 기존 갱신, 모순 발견 시 명시)
5. `wiki/timeline/YYYY-MM.md`에 항목 추가
6. `index.md` 갱신
7. `log.md`에 append: `## [YYYY-MM-DD] ingest | 제목`
8. `qmd index` 재실행 (증분 인덱싱)

**Query**:
1. `qmd search`로 관련 페이지 탐색 (index.md는 보조 카탈로그로 병행 참고)
2. 관련 페이지 읽고 출처를 인용하며 답변 종합
3. 답변이 재사용 가치 있으면 → `wiki/comparisons/` 등에 새 페이지로 저장할지 사용자에게 확인
4. `log.md`에 append: `## [YYYY-MM-DD] query | 질문 요약`

**Lint** (주기적, 사용자 요청 시):
- 페이지 간 모순, 최신 소스에 밀린 낡은 주장, 링크 없는 고아 페이지, 페이지 없이 언급만 된 중요 개념, 누락된 상호참조를 탐지해 보고

## 4. index.md / log.md 포맷

**index.md** — 카테고리별 표:
```markdown
# Index

## Entities
- [[openai]] — OpenAI, GPT 시리즈 개발사 (소스 3개)
- [[gpt-5]] — OpenAI 최신 모델 (2026-08 출시)

## Concepts
- [[rag]] — Retrieval-Augmented Generation

## Comparisons
- [[gpt-5-vs-claude-opus-5]]

## Timeline
- [[2026-08]]
```

**log.md** — append-only, 파싱 가능한 접두사:
```markdown
## [2026-08-20] ingest | GPT-5 출시 발표
## [2026-08-20] query | GPT-5 vs Claude 비교
```
`grep "^## \[" wiki/log.md | tail -5`로 최근 항목 조회 가능.

## 5. qmd 통합

- 로컬 검색엔진 (github.com/tobi/qmd), BM25 + 벡터 하이브리드, 온디바이스.
- `wiki/` 전체를 인덱싱 대상으로 등록, ingest가 끝날 때마다 재인덱싱.
- MCP 서버로도 연결 가능 — CLAUDE.md에 CLI 사용 예시 기록 (`qmd search "질문"`).
- 설치(Go 필요, 로컬 바이너리)와 최초 인덱싱은 구현 단계에서 진행.

## 6. 배치 위치 & 범위

- **CLAUDE.md**: 기존 코딩 가이드라인 아래에 새 섹션 `## 6. AI Library Wiki` 추가 — 위 워크플로/frontmatter/qmd 규칙 전부 기록. 별도 파일로 쪼개지 않고 Claude Code가 자동으로 읽는 단일 진입점(CLAUDE.md)에 유지.
- **.gitignore**: qmd 인덱스/캐시 파일(예: `.qmd/`) 제외 추가.
- **초기 스캐폴딩 범위**: `raw/`, `wiki/` 하위 폴더 전부 생성(빈 폴더 유지를 위한 `.gitkeep` 또는 placeholder), `index.md`/`log.md` 초기 뼈대 작성, CLAUDE.md 섹션 추가, qmd 설치 + 최초 인덱싱. 실제 소스 ingest는 이번 스캐폴딩에 포함하지 않는다.

## Out of Scope

- 실제 AI 뉴스 소스 수집·ingest
- Marp 슬라이드, matplotlib 차트 등 출력 포맷 자동화
- 팀/다중 사용자 협업 워크플로
