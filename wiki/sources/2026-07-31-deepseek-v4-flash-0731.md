---
type: source
tags: [model-release, deepseek, agentic, moe]
created: 2026-08-26
updated: 2026-08-26
sources:
  - https://ai-tldr.dev/releases/deepseek-v4-flash-0731/
  - https://www.digitalapplied.com/blog/deepseek-v4-flash-0731-official-release-agent-benchmarks
  - https://xenospectrum.com/en/deepseek-v4-flash-0731-pricing/
auto: true
---

# DeepSeek V4-Flash-0731 정식 출시

DeepSeek이 2026-07-31 [[deepseek|DeepSeek]] V4-Flash를 프리뷰에서 정식 API로 전환했다("DeepSeek-V4-Flash-0731"). 아키텍처와 크기는 프리뷰와 동일하게 유지한 채 post-training만 재실행했다.

## 핵심 내용

- 2,840억 파라미터 MoE, 프리뷰와 동일 아키텍처(post-training만 재실행)
- Terminal-Bench 2.1 점수 61.8 → 82.7로 20.9점 상승
- 하이브리드 어텐션 구조, 컨텍스트 윈도우 100만 토큰
- 가격은 프리뷰와 동일하게 유지: 입력(캐시 미스) $0.14, 출력 $0.28, 캐시 히트 입력 $0.0028(백만 토큰당)
- 동시 요청 처리량 2,500건으로 V4-Pro(500건) 대비 5배 — 에이전트 플릿 운용에 유리
- 최상위권 오픈 가중치 모델 중 에이전트 품질 대비 가장 저렴한 축에 속함

## 출처

- [DeepSeek V4-Flash goes official — 0731 hits 82.7 (AI/TLDR)](https://ai-tldr.dev/releases/deepseek-v4-flash-0731/)
- [DeepSeek V4 Flash 0731: Official Release, Agent Benchmarks (Digital Applied)](https://www.digitalapplied.com/blog/deepseek-v4-flash-0731-official-release-agent-benchmarks)
- [DeepSeek V4 Flash 0731 Launches Public Beta with Price Held at $0.14 (XenoSpectrum)](https://xenospectrum.com/en/deepseek-v4-flash-0731-pricing/)
