---
type: source
tags: [model-release, meta, open-weight, agentic, multimodal]
created: 2026-08-26
updated: 2026-08-26
sources:
  - https://research.meta.ai/blog/introducing-muse-glimmer-open-agentic-model
  - https://venturebeat.com/technology/meta-returns-to-open-source-with-muse-glimmer-an-apache-2-0-licensed-30b-parameter-ai-model-optimized-for-agents-available-now
  - https://www.marktechpost.com/2026/08/10/meta-ai-releases-muse-glimmer/
auto: true
---

# Meta, Muse Glimmer 오픈소스 공개

Meta가 2026-08-10 [[muse|Muse]] Glimmer를 Apache 2.0 라이선스로 공개했다. [[muse|Muse Spark]]에서 증류한 300억 파라미터 모델로, 소비자용 GPU 한 대에서 구동 가능한 온디바이스 에이전트를 겨냥했다. Meta가 다시 오픈소스 노선으로 돌아선 사례로 꼽힌다.

## 핵심 내용

- 밀집(dense) causal transformer, 총 파라미터 약 296억, 52개 레이어
- 전용 비전 인코더(약 18억 파라미터, ViT-G/14 스타일, 50레이어)를 내장해 텍스트·이미지를 인터리브로 네이티브 처리, 이미지당 최대 4,096 비주얼 토큰
- 스크린샷·다이어그램·문서를 코드 실행 중 인라인으로 해석하는 로컬 에이전트 활용에 최적화
- 어텐션은 [Local, Local, Local, Global] 패턴 반복, 슬라이딩 윈도우 2,048, RoPE(theta 500,000)는 로컬 레이어에만 적용
- Apache 2.0 라이선스로 소비자 GPU 1대에서 구동 가능

## 출처

- [Introducing Muse Glimmer: An Open Agentic Model That Runs on Your Device (Meta AI Research 공식)](https://research.meta.ai/blog/introducing-muse-glimmer-open-agentic-model)
- [Meta returns to open source with Muse Glimmer (VentureBeat)](https://venturebeat.com/technology/meta-returns-to-open-source-with-muse-glimmer-an-apache-2-0-licensed-30b-parameter-ai-model-optimized-for-agents-available-now)
- [Meta AI Releases Muse Glimmer (MarkTechPost)](https://www.marktechpost.com/2026/08/10/meta-ai-releases-muse-glimmer/)
