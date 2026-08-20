# AI News Wiki Scaffolding Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Scaffold the `ai-library` repo into the AI-news personal wiki described in `docs/superpowers/specs/2026-08-20-ai-news-wiki-design.md` — directory tree, index/log skeletons, CLAUDE.md workflow section, and a working local qmd search index. No actual news source is ingested by this plan.

**Architecture:** Two immutable/mutable directory trees (`raw/`, `wiki/`) at repo root, a project-local qmd search index (`.qmd/`), and a new CLAUDE.md section that documents the ingest/query/lint workflows so any future Claude Code session in this repo picks up the conventions automatically.

**Tech Stack:** Plain Markdown + YAML frontmatter (Obsidian-compatible), qmd (`@tobilu/qmd` npm package) for local hybrid search, git for versioning.

## Global Constraints

- Pages use Obsidian-compatible YAML frontmatter + `[[wikilink]]` syntax (spec §2).
- Filenames are kebab-case; source pages are named `YYYY-MM-DD-<slug>.md` (spec §2).
- `log.md` entries use the exact prefix `## [YYYY-MM-DD] ingest | <title>` or `## [YYYY-MM-DD] query | <summary>` (spec §4).
- The wiki schema/workflow lives in CLAUDE.md as a new `## 6. AI Library Wiki` section — not a separate file (spec §6).
- qmd's SQLite index files are excluded from git; qmd's own config (`.qmd/index.yml`) is checked in (spec §6, qmd docs).
- Actual source ingest (first article, etc.) is out of scope for this plan (spec Scope / Out of Scope).

---

### Task 1: Directory scaffolding

**Files:**
- Create: `raw/articles/.gitkeep`
- Create: `raw/papers/.gitkeep`
- Create: `raw/tweets/.gitkeep`
- Create: `raw/videos/.gitkeep`
- Create: `raw/assets/.gitkeep`
- Create: `wiki/entities/.gitkeep`
- Create: `wiki/concepts/.gitkeep`
- Create: `wiki/sources/.gitkeep`
- Create: `wiki/comparisons/.gitkeep`
- Create: `wiki/timeline/.gitkeep`

**Interfaces:**
- Consumes: nothing (first task).
- Produces: the `raw/` and `wiki/` directory trees that Tasks 2-4 write files into.

- [ ] **Step 1: Create all ten directories with empty `.gitkeep` placeholders**

Git doesn't track empty directories, so each leaf directory needs a placeholder file to be committed.

```bash
mkdir -p raw/articles raw/papers raw/tweets raw/videos raw/assets
mkdir -p wiki/entities wiki/concepts wiki/sources wiki/comparisons wiki/timeline
touch raw/articles/.gitkeep raw/papers/.gitkeep raw/tweets/.gitkeep raw/videos/.gitkeep raw/assets/.gitkeep
touch wiki/entities/.gitkeep wiki/concepts/.gitkeep wiki/sources/.gitkeep wiki/comparisons/.gitkeep wiki/timeline/.gitkeep
```

- [ ] **Step 2: Verify all ten directories exist**

Run: `find raw wiki -type d | sort`
Expected output:
```
raw
raw/articles
raw/assets
raw/papers
raw/tweets
raw/videos
wiki
wiki/comparisons
wiki/concepts
wiki/entities
wiki/sources
wiki/timeline
```

- [ ] **Step 3: Commit**

```bash
git add raw wiki
git commit -m "$(cat <<'EOF'
feat: raw/wiki 디렉토리 골격 생성

AI 뉴스 위키 스캐폴딩 - 소스 타입별/위키 카테고리별 디렉토리 추가
EOF
)"
```

---

### Task 2: index.md and log.md skeletons

**Files:**
- Create: `wiki/index.md`
- Create: `wiki/log.md`

**Interfaces:**
- Consumes: `wiki/` directory from Task 1.
- Produces: `wiki/index.md` (catalog, appended to on every future ingest) and `wiki/log.md` (append-only log, appended to on every future ingest/query) that later ingest work will edit.

- [ ] **Step 1: Write `wiki/index.md`**

```markdown
# Index

## Entities

## Concepts

## Comparisons

## Timeline
```

- [ ] **Step 2: Write `wiki/log.md`**

```markdown
# Log

<!-- append-only. Entry format: ## [YYYY-MM-DD] ingest | <title>  또는  ## [YYYY-MM-DD] query | <summary> -->
```

- [ ] **Step 3: Verify structure**

Run: `grep -c "^## " wiki/index.md`
Expected: `4`

Run: `grep "append-only" wiki/log.md`
Expected: the comment line printed (non-empty match)

- [ ] **Step 4: Commit**

```bash
git add wiki/index.md wiki/log.md
git commit -m "$(cat <<'EOF'
feat: wiki index/log 뼈대 추가

카테고리별 카탈로그(index.md)와 append-only 작업 기록(log.md) 초기 뼈대
EOF
)"
```

---

### Task 3: CLAUDE.md wiki workflow section

**Files:**
- Modify: `CLAUDE.md` (append after the existing `## 5. Commit Messages` section)

**Interfaces:**
- Consumes: nothing from other tasks (independent of Task 1/2's file contents, only assumes the directory names/paths from spec §1 which are fixed).
- Produces: the canonical workflow reference that any future Claude Code session in this repo reads automatically — later ingest work follows the steps documented here.

- [ ] **Step 1: Append the new section to `CLAUDE.md`**

Add this exact block at the end of the file (after the `## 5. Commit Messages` section):

```markdown

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
8. `qmd embed` 재실행 (증분 임베딩)

### Query 워크플로

1. `qmd query "질문"`으로 관련 페이지 탐색 (하이브리드 검색, `wiki/index.md`도 보조 참고)
2. 관련 페이지 읽고 출처를 인용하며 답변 종합
3. 재사용 가치 있는 답변은 `wiki/comparisons/` 등에 새 페이지로 저장할지 사용자에게 확인
4. `wiki/log.md`에 append: `## [YYYY-MM-DD] query | 질문 요약`

### Lint (주기적, 사용자 요청 시)

페이지 간 모순, 낡은 주장, 링크 없는 고아 페이지, 누락된 상호참조를 점검해 보고.

### qmd 검색 도구

- 설치: `npm install -g @tobilu/qmd`
- 프로젝트 로컬 인덱스: `qmd init` (`.qmd/index.yml` 생성)
- 컬렉션 등록: `qmd collection add wiki --name wiki`
- 임베딩: `qmd embed`
- 검색: `qmd query "질문"` (하이브리드, 기본 권장) / `qmd search "질문"` (키워드만) / `qmd vsearch "질문"` (의미 검색만)
```

- [ ] **Step 2: Verify the section was appended correctly**

Run: `grep -n "^## 6. AI Library Wiki$" CLAUDE.md`
Expected: one match, at the end of the file (no duplicate section numbers).

Run: `grep -c "^## " CLAUDE.md`
Expected: `6` (five original top-level sections + the new one)

- [ ] **Step 3: Commit**

```bash
git add CLAUDE.md
git commit -m "$(cat <<'EOF'
docs: CLAUDE.md에 AI Library Wiki 섹션 추가

디렉토리 구조, 페이지 규칙, ingest/query/lint 워크플로, qmd 사용법 명시
EOF
)"
```

---

### Task 4: qmd install, project-local index, .gitignore

**Files:**
- Modify: `.gitignore` (create if it doesn't exist)
- Create (by `qmd init`, not hand-written): `.qmd/index.yml`

**Interfaces:**
- Consumes: `wiki/` directory with `index.md`/`log.md` from Task 2 (the collection needs at least one real path to index).
- Produces: a working `qmd query` command that any future Claude Code session can shell out to during the Query workflow documented in Task 3.

- [ ] **Step 1: Check for an existing `.gitignore`, then add qmd's cache entries**

Run: `test -f .gitignore && cat .gitignore || echo "no .gitignore yet"`

If the file exists, append; if not, create it. Final `.gitignore` must contain at least:

```
.qmd/index.sqlite
.qmd/index.sqlite-*
```

- [ ] **Step 2: Install qmd globally**

```bash
npm install -g @tobilu/qmd
```

Run: `qmd --version`
Expected: a version string is printed (no "command not found" error).

- [ ] **Step 3: Initialize a project-local index and register the wiki collection**

```bash
qmd init
qmd collection add wiki --name wiki
```

Run: `test -f .qmd/index.yml && echo "config ok"`
Expected: `config ok`

- [ ] **Step 4: Generate embeddings**

```bash
qmd embed
```

Expected: command completes without error (it will embed `wiki/index.md` and `wiki/log.md` — the only content that exists at this point).

- [ ] **Step 5: Verify search works end-to-end**

Run: `qmd query "index"`
Expected: command completes without error and returns at least the `wiki/index.md` page (its content contains the word "Index").

- [ ] **Step 6: Commit the .gitignore and qmd config (not the sqlite index)**

```bash
git add .gitignore .qmd/index.yml
git status
```

Confirm `git status` shows no `.qmd/index.sqlite*` files staged (they must be ignored) before committing.

```bash
git commit -m "$(cat <<'EOF'
chore: qmd 로컬 검색 인덱스 도입

프로젝트 로컬 qmd 인덱스 초기화, wiki 컬렉션 등록, sqlite 인덱스는 gitignore 처리
EOF
)"
```

---

## Self-Review Notes

- **Spec coverage:** §1 directories → Task 1. §2 frontmatter/naming rules → documented in Task 3's CLAUDE.md block (rules are conventions the LLM follows during future ingest, not files to scaffold now). §3 workflows → Task 3. §4 index/log format → Task 2 (skeletons) + Task 3 (log entry format documented). §5 qmd → Task 4. §6 placement/.gitignore → Task 3 (CLAUDE.md placement) + Task 4 (.gitignore). Out-of-scope items (actual ingest, Marp/matplotlib, multi-user) are correctly excluded — no task touches them.
- **Placeholder scan:** no TBD/TODO; every step has literal file content or exact commands with expected output.
- **Type/naming consistency:** directory names (`raw/{articles,papers,tweets,videos,assets}`, `wiki/{entities,concepts,sources,comparisons,timeline}`), file names (`index.md`, `log.md`), and CLAUDE.md section number (`## 6.`) match exactly across Tasks 1-4 and the spec.
