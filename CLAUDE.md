# CLAUDE.md

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

## 5. Commit Messages

- Conventional Commits 형식을 따른다 (`type: description`).
- 한글로 작성한다.
- 자세한 설명이 필요하면 본문은 최대 2줄까지만 작성한다.
- 작성자(author/co-author) 정보는 쓰지 않는다.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.

## 6. AI Library Wiki

**주제**: AI 관련 소식(모델, 논문, 툴, 트렌드) 전용 개인 지식 베이스.

### 디렉토리 구조

```
raw/                    # 원본 소스, 불변 (읽기만 함)
  articles/ papers/ tweets/ videos/ assets/
wiki/                    # LLM이 전적으로 관리
  entities/ concepts/ sources/ comparisons/ timeline/
  index.md               # 카탈로그
  log.md                  # append-only 작업 기록
```

### 페이지 규칙

- Obsidian 호환: YAML frontmatter + `[[wikilink]]`.
- frontmatter 필드: `type`, `tags`, `created`, `updated`, `sources`.
- 파일명: kebab-case. 소스 페이지는 `YYYY-MM-DD-슬러그.md`.

### Ingest 워크플로 (소스 1개씩 깊게 개입)

1. `raw/`에서 새 파일 확인
2. 읽고 핵심 내용을 사용자와 논의
3. `wiki/sources/YYYY-MM-DD-슬러그.md` 요약 페이지 작성
4. 관련 `entities/`, `concepts/` 페이지 갱신 (모순 발견 시 명시)
5. `wiki/timeline/YYYY-MM.md`에 항목 추가
6. `wiki/index.md` 갱신
7. `wiki/log.md`에 append: `## [YYYY-MM-DD] ingest | 제목`
8. `qmd update && qmd embed` 재실행 (증분 인덱싱 + 임베딩)

### Query 워크플로

1. `qmd query "질문"`으로 관련 페이지 탐색 (하이브리드 검색, `wiki/index.md`도 보조 참고)
2. 관련 페이지 읽고 출처를 인용하며 답변 종합
3. 재사용 가치 있는 답변은 `wiki/comparisons/` 등에 새 페이지로 저장할지 사용자에게 확인
4. `wiki/log.md`에 append: `## [YYYY-MM-DD] query | 질문 요약`

### Lint (주기적, 사용자 요청 시)

페이지 간 모순, 낡은 주장, 링크 없는 고아 페이지, 누락된 상호참조를 점검해 보고.

### qmd 검색 도구

**주의**: qmd 명령(`qmd update`, `qmd embed`, `qmd query` 등)은 항상 저장소 루트에서 실행한다. `.qmd/index.yml`이 상대 경로(`path: wiki`)를 쓰기 때문에, 다른 디렉터리에서 실행하면 엉뚱한 경로를 스캔해 컬렉션 파일이 삭제된 것으로 처리될 수 있다.

**최초 설정 (이 저장소에서 이미 완료됨 — `qmd init` / `qmd collection add`는 재실행 금지)**

- 설치: `npm install -g @tobilu/qmd`
- 프로젝트 로컬 인덱스: `qmd init` (`.qmd/index.yml` 생성)
- 컬렉션 등록: `qmd collection add wiki --name wiki`
- 임베딩: `qmd embed`
- `.qmd/index.yml`이 이미 커밋되어 있다. `qmd collection add`를 다시 실행하면 경로 인자가 절대경로로 resolve되어 `path:` 필드를 덮어쓴다 (커밋 609304f에서 고친 상대경로 버그가 재발함) — `qmd init`/`qmd collection add`만 재실행 금지, `qmd embed`는 평소 워크플로에서 계속 사용.

**새 클론 / 새 머신에서 할 일**

1. `npm install -g @tobilu/qmd` (설치)
2. `qmd pull` (모델 다운로드, 최초 1회)
3. `qmd update && qmd embed` (파일시스템 스캔 + 임베딩. `qmd init`, `qmd collection add`는 재실행하지 않는다)

**검색**

- `qmd query "질문"` (하이브리드, 기본 권장) / `qmd search "질문"` (키워드만) / `qmd vsearch "질문"` (의미 검색만)
