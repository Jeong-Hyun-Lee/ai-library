---
type: source
tags: [tool, perplexity, privacy, on-device, local-model]
created: 2026-09-08
updated: 2026-09-08
sources:
  - https://www.perplexity.ai/hub/blog/introducing-hybrid-compute-on-mac
  - https://www.marktechpost.com/2026/09/01/perplexity-releases-hybrid-compute-on-mac-cloud-agents-orchestrate-down-to-a-local-model-gated-on-device/
  - https://9to5mac.com/2026/09/01/perplexity-launches-privacy-minded-hybrid-compute-ai-feature-for-mac/
auto: true
---

# Perplexity, Mac용 프라이버시 중심 Hybrid Compute 출시

Perplexity가 2026-09-01 Mac용 Perplexity Computer에 [[hybrid-compute|Hybrid Compute]] 모드를 도입했다. 클라우드 프론티어 모델과 온디바이스 로컬 모델을 작업 성격에 따라 자동 전환해 처리한다.

## 핵심 내용

- 어려운 추론·검색·계획은 클라우드에서, 민감 정보가 개입되는 단계는 Apple 실리콘에서 구동되는 로컬 모델로 자동 전환
- 출시 시점 로컬 모델 3종 지원: Gemma 4 E4B, Qwen3.6 35B-A3B, 자체 모델(PPLX Qwen 3.8 27B, 원클릭 다운로드)
- macOS 15 이상, 통합 메모리 24GB 이상의 Apple 실리콘 Mac에서 구동, 터미널 없이 앱 내 원클릭 설정
- 온디바이스 Privacy Gate가 프롬프트·툴 출력·메모리·로그를 클라우드 전송 전에 검사 — 판별 분류기 코드를 "PII-Tracer"라는 이름으로 Hugging Face에 오픈소스 공개
- Perplexity Pro·Max·Enterprise 구독자 대상 제공

## 출처

- [Introducing Hybrid Compute on Mac (Perplexity 공식)](https://www.perplexity.ai/hub/blog/introducing-hybrid-compute-on-mac)
- [Perplexity Releases Hybrid Compute on Mac (MarkTechPost)](https://www.marktechpost.com/2026/09/01/perplexity-releases-hybrid-compute-on-mac-cloud-agents-orchestrate-down-to-a-local-model-gated-on-device/)
- [Perplexity launches privacy-minded 'hybrid compute' AI feature for Mac (9to5Mac)](https://9to5mac.com/2026/09/01/perplexity-launches-privacy-minded-hybrid-compute-ai-feature-for-mac/)
