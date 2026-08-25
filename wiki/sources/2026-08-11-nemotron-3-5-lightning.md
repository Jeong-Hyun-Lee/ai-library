---
type: source
tags: [model-release, nvidia, nemotron, open-weight, moe, agentic]
created: 2026-08-25
updated: 2026-08-25
sources:
  - https://developer.nvidia.com/blog/nvidia-nemotron-3-5-lightning-delivers-fast-accurate-specialized-task-execution-for-long-running-agents/
  - https://www.cnbc.com/2026/08/11/nvidia-releases-nemotron-3point5-lightning-open-source-ai-model-.html
  - https://www.marktechpost.com/2026/08/11/nvidia-ai-releases-nemotron-3-5-lightning-and-nemo-switchyard/
auto: true
---

# Nemotron 3.5 Lightning 출시

NVIDIA가 2026-08-11 오픈 가중치 모델 [[nemotron|Nemotron]] 3.5 Lightning을 공개했다. 젠슨 황이 지난달 오픈소스 노선을 재천명한 이후 NVIDIA의 첫 오픈소스 모델 출시다.

## 핵심 내용

- 30B MoE, 활성 파라미터 3B — PC 단일 GPU에서 구동 가능한 경량 설계
- Mamba-2 + MoE + Attention 하이브리드 아키텍처, 컨텍스트 윈도우 100만 토큰
- PinchBench 86% 달성, 유사 크기 모델 대비 최대 4배 빠른 출력 속도
- 동일 정확도 기준 Qwen3.6 35B보다 PinchBench 1만 태스크 처리 30% 빠름
- 승인·비용 없이 자유롭게 다운로드·수정 가능한 오픈소스 라이선스
- 에이전트 워크플로 각 단계를 최적 모델로 라우팅하는 오픈소스 라이브러리 NeMo Switchyard도 함께 공개

## 출처

- [NVIDIA Nemotron 3.5 Lightning Delivers Fast, Accurate Specialized Task Execution for Long-Running Agents (NVIDIA 공식 블로그)](https://developer.nvidia.com/blog/nvidia-nemotron-3-5-lightning-delivers-fast-accurate-specialized-task-execution-for-long-running-agents/)
- [Nvidia releases Nemotron 3.5 Lightning, open-source AI model (CNBC)](https://www.cnbc.com/2026/08/11/nvidia-releases-nemotron-3point5-lightning-open-source-ai-model-.html)
- [NVIDIA AI Releases Nemotron 3.5 Lightning (MarkTechPost)](https://www.marktechpost.com/2026/08/11/nvidia-ai-releases-nemotron-3-5-lightning-and-nemo-switchyard/)
