---
type: source
tags: [tool, open-source, research-agent]
created: 2026-09-17
updated: 2026-09-17
sources:
  - https://github.com/alphaXiv/OpenResearch
auto: true
---

# alphaXiv, 코딩 에이전트를 연구 에이전트로 바꾸는 "OpenResearch" 공개

AI 논문 열람 서비스 alphaXiv가 오픈소스 로컬 우선(local-first) 워크스페이스 [[openresearch|OpenResearch]]를 공개, GitHub Trending에 오르며 주목받고 있다.

## 핵심 내용

- Claude Code, Codex, OpenCode, Cursor 등 기존 코딩 에이전트를 연구 에이전트로 전환 — 문헌 검토·가설 수립·실험 실행·결과물 산출까지 자율 수행.
- 아이디어 제안 → 코드 변경 → 실험 실행 → 근거 검토 → 다음 시도 결정까지 이어지는 루프를 자율 반복하며, 여러 에이전트가 서로 다른 방향을 병렬 탐색하고 실험 계보(lineage)를 트리 구조로 보존.
- 동일한 커밋 스냅샷을 로컬, SSH, 또는 Slurm·Kubernetes·Ray·Hugging Face Jobs·Modal·Tinker 등 다양한 컴퓨팅 환경에서 실행 가능.
- 로컬 우선 설계로 프로젝트·대화·실험·로그·코드·산출물이 모두 사용자 기기 내(127.0.0.1, SQLite)에 저장되어 외부로 전송되지 않음.
- Rust 기반 메인 저장소(`alphaXiv/OpenResearch`)와 별도 CLI 저장소(`alphaXiv/openresearch-cli`)로 배포.

## 출처

- [GitHub - alphaXiv/OpenResearch: Turn your coding agents into research agents](https://github.com/alphaXiv/OpenResearch)
