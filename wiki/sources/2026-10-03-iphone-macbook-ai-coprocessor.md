---
type: source
tags: [hardware, apple, on-device]
created: 2026-10-03
updated: 2026-10-03
sources:
  - https://wccftech.com/iphone-17-pro-max-m4-pro-macbook-pro-44-percent-faster-prefill-27b-ai-model/
  - https://www.argmaxinc.com/blog/iphone-17-on-device-inference-benchmarks
auto: true
---

# iPhone 17 Pro Max를 보조 연산장치로 활용 — MacBook Pro 로컬 AI 추론 속도 최대 44% 향상

커스텀 소프트웨어로 iPhone 17 Pro Max를 M4 Pro [[apple|MacBook Pro]]에 연결해 로컬 270억 파라미터 AI 모델을 구동한 결과, 프리필(prefill) 속도가 최대 44% 향상됐다고 2026-10-03 보도됐다.

## 핵심 내용

- 8K 컨텍스트 기준: Mac 단독 초당 132토큰 → iPhone 연동 시 초당 177토큰(35% 향상).
- 16K 컨텍스트 기준: Mac 단독 초당 109토큰 → iPhone 연동 시 초당 157토큰(44% 향상).
- 방식: 256토큰 배치마다 MacBook Pro가 1~40번째 레이어를 처리해 활성값을 iPhone으로 전송하고, iPhone의 A19 Pro GPU가 41~64번째 레이어를 처리하는 동안 Mac은 다음 배치 연산을 시작 — 두 기기 간 연산 분할(split computation)로 유휴 자원을 활용.
- iPhone을 Mac의 AI 보조 연산장치(co-processor)로 활용하는 새로운 활용 사례로 주목받음.

## 출처

- [An iPhone 17 Pro Max Connected To An M4 Pro MacBook Pro Combined With Custom Software Resulted In Up To 44% Prefill Speeds When Running A 27B AI Model - Wccftech](https://wccftech.com/iphone-17-pro-max-m4-pro-macbook-pro-44-percent-faster-prefill-27b-ai-model/)
- [iPhone 17 - Argmax](https://www.argmaxinc.com/blog/iphone-17-on-device-inference-benchmarks)
