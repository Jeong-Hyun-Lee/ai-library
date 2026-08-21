# AI News Auto Routine Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a fully-automated daily AI-news discovery+ingest routine to the `ai-library` wiki, per `docs/superpowers/specs/2026-08-20-ai-news-auto-routine-design.md` — a new CLAUDE.md workflow section, and a live daily cron job (via the `schedule` skill) that triggers it.

**Architecture:** Two tasks. Task 1 documents the automated workflow in CLAUDE.md (single source of truth, same pattern as the existing manual Ingest workflow) and extends the page frontmatter schema with an `auto` field. Task 2 actually creates the recurring cloud-scheduled job via the `schedule` skill, pointing its prompt at the CLAUDE.md section Task 1 wrote, and discovers/documents whatever real runtime constraints the `schedule` skill has (this was explicitly flagged as unverified in the spec).

**Tech Stack:** Markdown + YAML frontmatter (existing wiki conventions), the `schedule` skill (cron-based cloud agent), git.

## Global Constraints

- The automated workflow is documented as CLAUDE.md `## 7. AI 뉴스 자동 루틴` — a new section, not a replacement of the existing `## 6.`'s manual Ingest workflow (spec Scope).
- Auto-ingested pages get `auto: true` in frontmatter; manual pages omit the field (spec §3).
- Cron schedule: daily at 12:45 PM KST, recurring (spec §1).
- If a day produces zero candidates, nothing is written or committed — no log entry, no index change, no commit (spec §2 step 9).
- Commit messages: Conventional Commits format, Korean, body max 2 lines, no author/co-author line (CLAUDE.md §5, already established this session).
- `raw/` is NOT used by the auto routine — auto-ingested source pages contain only a link + summary (spec §2 step 4).
- The `schedule` skill's actual runtime environment (repo/git access, tool availability) is unverified — Task 2 must discover and document this rather than assume it matches this local session (spec §4).

---

### Task 1: CLAUDE.md — auto routine section + frontmatter extension

**Files:**
- Modify: `CLAUDE.md:89-93` (frontmatter field bullet list, under `## 6.` → `### 페이지 규칙`)
- Modify: `CLAUDE.md` (append new `## 7. AI 뉴스 자동 루틴` section at end of file, after the current last line)

**Interfaces:**
- Consumes: the current CLAUDE.md structure — 6 top-level `## ` sections, the last content under `## 6.` being the `### qmd 검색 도구` subsection ending with the `검색` bullet list.
- Produces: a `## 7. AI 뉴스 자동 루틴` section that Task 2's cron job prompt references by exact name (`저장소의 CLAUDE.md ## 7. AI 뉴스 자동 루틴 워크플로`).

- [ ] **Step 1: Add the `auto` frontmatter field to the page-rules bullet list**

Current text at `CLAUDE.md:89-93`:

```markdown
### 페이지 규칙

- Obsidian 호환: YAML frontmatter + `[[wikilink]]`.
- frontmatter 필드: `type`, `tags`, `created`, `updated`, `sources`.
- 파일명: kebab-case. 소스 페이지는 `YYYY-MM-DD-슬러그.md`.
```

Replace the middle bullet so the block reads:

```markdown
### 페이지 규칙

- Obsidian 호환: YAML frontmatter + `[[wikilink]]`.
- frontmatter 필드: `type`, `tags`, `created`, `updated`, `sources`, `auto`(선택 — 자동 루틴이 사람 개입 없이 작성한 페이지면 `true`. 수동 ingest 페이지는 생략).
- 파일명: kebab-case. 소스 페이지는 `YYYY-MM-DD-슬러그.md`.
```

- [ ] **Step 2: Append the new `## 7. AI 뉴스 자동 루틴` section**

Add this exact block at the very end of `CLAUDE.md` (after its current last line, `- `qmd query "질문"` (하이브리드, 기본 권장) / `qmd search "질문"` (키워드만) / `qmd vsearch "질문"` (의미 검색만)`):

```markdown

## 7. AI 뉴스 자동 루틴

**트리거**: `schedule` 스킬(클라우드 에이전트, cron)로 매일 오후 12시 45분(KST) 실행. 세션 내 `CronCreate`는 세션 종료 시 소멸 + 7일 만료라 사용하지 않는다.

**워크플로** (`## 6.`의 수동 Ingest와 별개, 사람 개입 없이 전부 자동):

1. `last30days` 스킴으로 "AI 소식"(모델/논문/툴/트렌드) 검색
2. `wiki/log.md` + `wiki/index.md`를 읽고, 이미 다룬 주제/이벤트와 겹치는 후보 제외 (LLM 판단, 별도 DB 없음)
3. 남은 후보 중 상위 3건 선정 (품질/중요도 기준)
4. 각 건마다:
   - `wiki/sources/YYYY-MM-DD-슬러그.md` 작성 — 링크+요약만, `raw/` 저장 생략, frontmatter에 `auto: true` 추가
   - 관련 `entities/`, `concepts/` 페이지 갱신 (모순 발견 시 명시)
   - `wiki/timeline/YYYY-MM.md`에 항목 추가
5. `wiki/index.md` 갱신 (Sources 섹션 포함)
6. `wiki/log.md`에 append: `## [YYYY-MM-DD] ingest (auto) | 제목` — 수동 ingest와 `(auto)` 표시로 구분
7. `qmd update && qmd embed` 재실행
8. 커밋(한글, Conventional Commits, 본문 최대 2줄, 작성자 표기 없음) + `git push origin main`까지 자동
9. 그날 후보가 0건이면 아무것도 기록/커밋하지 않는다 (`log.md`, `index.md`, git 전부 무변경)
```

- [ ] **Step 3: Verify**

Run: `grep -n "^## 7. AI 뉴스 자동 루틴$" CLAUDE.md`
Expected: exactly one match, at the end of the file.

Run: `grep -c "^## " CLAUDE.md`
Expected: `7` (six original sections + the new one — CLAUDE.md currently ends at `## 6.`, this adds `## 7.`).

Run: `grep -n "auto" CLAUDE.md`
Expected: matches on the frontmatter bullet (Step 1), the `auto: true` mention in the new section's step 4, and no unrelated stray matches.

- [ ] **Step 4: Commit**

```bash
git add CLAUDE.md
git commit -m "$(cat <<'EOF'
docs: AI 뉴스 자동 루틴 섹션 추가

CLAUDE.md에 자동 발굴+ingest+커밋/push 워크플로와 auto frontmatter 필드 문서화
EOF
)"
```

---

### Task 2: Create the daily cron job via the `schedule` skill

**Files:**
- None. This task creates external cloud-scheduler state via the `schedule` skill — it does not modify any file in this repo. (If the `schedule` skill itself writes a config file into the repo as part of its normal operation, that's expected and should be documented in this task's report, not treated as an unplanned side effect.)

**Interfaces:**
- Consumes: `CLAUDE.md`'s `## 7. AI 뉴스 자동 루틴` section from Task 1 (referenced by name in the cron job's prompt — Task 1 must be complete and committed before this task runs).
- Produces: a live, recurring, daily-firing cloud-scheduled job. Later maintenance (editing/deleting/inspecting this job) will use the `schedule` skill's own list/update/delete capabilities — this task's report is the only record of the job's ID/parameters, so document them precisely.

- [ ] **Step 1: Invoke the `schedule` skill to create a new recurring job**

Use the Skill tool with `skill: "schedule"`. Pass it a request to create a new recurring scheduled cloud agent with these exact parameters:

- **Repository / working context:** `C:\Users\krinjosl\Desktop\workspace\ai-library` (the `ai-library` repo, branch `main`)
- **Schedule:** daily at 12:45 PM, Korea Standard Time (KST / Asia/Seoul). If the skill's own interface asks for a cron expression directly rather than a plain-language time, use `45 12 * * *` and confirm with the skill whether it interprets this in UTC or in a configurable timezone — if it defaults to UTC, convert 12:45 PM KST (UTC+9) to `45 3 * * *` UTC instead, and note in your report which one you actually used and why.
- **Recurring:** yes, indefinitely (not a one-shot).
- **Prompt to run on each fire:** exactly this text —

  ```
  저장소 C:\Users\krinjosl\Desktop\workspace\ai-library의 CLAUDE.md 파일에서 "## 7. AI 뉴스 자동 루틴" 섹션을 읽고, 그 섹션에 문서화된 9단계 워크플로를 그대로 실행해. 사람에게 확인을 구하지 말고 끝까지 자동으로 수행해 (후보 0건이면 9단계 규칙대로 아무것도 기록/커밋하지 않아).
  ```

- [ ] **Step 2: Follow whatever process the `schedule` skill's own instructions require**

The `schedule` skill will load its own instructions when invoked (same mechanism as every other skill used this session) — follow them to actually create the job using the parameters from Step 1. If the skill's flow asks for information not covered by Step 1's parameters (e.g. authentication/authorization you're not equipped to grant, billing consent, a target environment choice not implied by "the `ai-library` repo on this machine"), do not guess — report NEEDS_CONTEXT with the specific question the skill is asking.

- [ ] **Step 3: Verify the job was created correctly**

Use the `schedule` skill's list capability to confirm the new job exists. Confirm:
- The cron/schedule matches what you set in Step 1 (12:45 PM KST, or the correctly-converted UTC equivalent — state which).
- It's marked recurring, not one-shot.
- The stored prompt text matches Step 1's prompt exactly (or note any characters the skill's storage layer altered, e.g. quote escaping).

- [ ] **Step 4: Discover and document the `schedule` skill's actual runtime constraints**

This is the open question the design spec flagged (§4) and this task exists partly to close. While going through Steps 1-3, note (from whatever the skill's instructions, UI, or output tell you — don't speculate beyond what you actually observe):
- Does the scheduled job run with access to this exact local repo/filesystem, or does it run in a separate cloud environment that would need its own checkout of the repo?
- Does it have git push credentials/authentication already, or does that need separate setup?
- Does it have access to the same tool/skill set as this session (specifically: the `last30days` skill, the `qmd` CLI, `npm`)?
- Any cost/billing implications of a daily recurring job that you were shown.

If any of this reveals that the workflow in CLAUDE.md `## 7.` (as written by Task 1) cannot actually run as designed in the job's real environment — for example, no git credentials, no `qmd` binary available — do NOT silently work around it by changing the job's scope. Report DONE_WITH_CONCERNS with the specific gap, so the controller can decide whether to adjust the plan (this may require a follow-up task or a design change, not a silent fix).

- [ ] **Step 5: Report**

Write your full report to: `C:\Users\krinjosl\Desktop\workspace\ai-library\.superpowers\sdd\task-2-auto-routine-report.md`
- The exact parameters used to create the job (cron/schedule value, timezone handling, recurring flag, full prompt text)
- The job's ID/name as returned by the `schedule` skill (so it can be found again later)
- The Step 3 verification output
- The Step 4 findings (runtime environment, git/tool access, cost/billing notes) — be explicit about what you actually observed vs. what remains unknown
- Any concerns

Then report back with ONLY (under 15 lines):
- **Status:** DONE | DONE_WITH_CONCERNS | BLOCKED | NEEDS_CONTEXT
- The job ID/name and its schedule as created
- One-line summary of the Step 4 runtime-constraint findings
- Your concerns, if any
- The report file path

---

## Self-Review Notes

- **Spec coverage:** §1 (trigger mechanism, cron time, prompt pattern) → Task 2. §2 (9-step workflow, incl. the "0 candidates → no record" rule) → Task 1 documents it verbatim, Task 2's cron prompt points at it. §3 (frontmatter `auto` field) → Task 1 Step 1. §4 (unverified `schedule` runtime constraints) → Task 2 Step 4 exists specifically to close this gap, with an explicit instruction not to paper over a real gap if found. Purpose/Scope (new section alongside, not replacing, `## 6.`) → Task 1 only appends, never edits the existing Ingest/Query/Lint subsections.
- **Placeholder scan:** no TBD/TODO; Task 1 has the complete literal text to write; Task 2 cannot have "complete literal commands" the way Task 1 does because its job is partly to discover an unknown interface — this is handled by giving it exact parameters/values plus explicit escalation rules (ask before guessing) rather than a fabricated command transcript, which would violate the actual constraint (the `schedule` skill's interface is genuinely unknown at plan-writing time).
- **Type/naming consistency:** the cron prompt in Task 2 Step 1 references the section title exactly as Task 1 Step 2 creates it (`## 7. AI 뉴스 자동 루틴`), and the frontmatter field name (`auto`) matches between Task 1's two edits and the spec.
