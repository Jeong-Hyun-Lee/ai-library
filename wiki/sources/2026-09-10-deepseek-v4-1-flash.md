---
type: source
tags: [model-release, deepseek, open-weight, multimodal, agentic]
created: 2026-09-14
updated: 2026-09-14
sources:
  - https://siliconangle.com/2026/09/10/deepseek-releases-v4-1-flash-says-it-outperforms-flagship-v4-pro/
  - https://venturebeat.com/technology/deepseek-v4-1-flash-debuts-with-0-003-1m-off-peak-cached-input-rate-and-benchmarks-eclipsing-gpt-5-6-sol-claude-opus-5
  - https://www.theregister.com/ai-and-ml/2026/09/11/deepseeks-new-model-sets-a-template-for-powerful-llms-that-run-lean/5295715
auto: true
---

# DeepSeek, V4.1-Flash 출시 — 플래그십 V4-Pro보다 우수하다고 주장

DeepSeek이 2026-09-10 네이티브 멀티모달 모델 [[deepseek|DeepSeek]]-V4.1-Flash를 출시했다. 자사 플래그십 V4-Pro보다 성능·비용·속도·총 처리시간에서 앞선다는 복수 기관의 테스트 결과를 근거로 제시했다.

## 핵심 내용

- MoE 아키텍처, 총 파라미터 5,520억, 입력 처리 시 활성 파라미터 80억·출력 생성 시 160억, 컨텍스트 윈도우 100만 토큰, 네이티브 비전 지원
- DeepSWE 74.2점(GPT-6 Astra·Gemini 3.8 Flash·Opus 5와 비슷한 수준), Terminal-Bench 2.1 90.6점, CyberGym 88.1점
- 가격(오프피크, 월~금 01:00-04:00·06:00-10:00 UTC): 캐시 히트 입력 $0.003, 캐시 미스 입력 $0.15, 출력 $0.60(백만 토큰당) — 피크 시간대는 2배
- 2026-09-14부터 API의 V4-Pro 요청을 V4.1-Flash가 대신 처리하며 V4.1-Flash 요금으로 과금(V4.1-Pro 출시 전까지)

## 출처

- [DeepSeek releases V4.1-Flash, says it outperforms flagship V4-Pro (SiliconANGLE)](https://siliconangle.com/2026/09/10/deepseek-releases-v4-1-flash-says-it-outperforms-flagship-v4-pro/)
- [DeepSeek-V4.1-Flash debuts with $0.003/1M off-peak cached-input rate and benchmarks eclipsing GPT-5.6 Sol, Claude Opus 5 (VentureBeat)](https://venturebeat.com/technology/deepseek-v4-1-flash-debuts-with-0-003-1m-off-peak-cached-input-rate-and-benchmarks-eclipsing-gpt-5-6-sol-claude-opus-5)
- [DeepSeek's new model sets a template for powerful LLMs that run lean (The Register)](https://www.theregister.com/ai-and-ml/2026/09/11/deepseeks-new-model-sets-a-template-for-powerful-llms-that-run-lean/5295715)
