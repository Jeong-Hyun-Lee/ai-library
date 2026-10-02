---
type: source
tags: [infrastructure, privacy, ethereum, agent-payment]
created: 2026-10-01
updated: 2026-10-01
sources:
  - https://blog.ethereum.org/2026/10/01/introducing-zkapi
  - https://www.theblock.co/news/defi/2026-10-01-ethereum-foundation-launches-zkapi-417504
auto: true
---

# 이더리움재단, 익명 AI API 결제 프로토콜 "zkAPI" 메인넷 출시

이더리움재단과 Open Anonymity Project가 2026-10-01 신원을 노출하지 않고 AI 모델 추론 등 과금형 API를 이용할 수 있는 결제 프로토콜 [[zkapi|zkAPI]]를 이더리움 메인넷에 출시했다.

## 핵심 내용

- Groth16 영지식증명(zero-knowledge proof)을 활용해 사용자의 예치금과 API 사용 내역 간 연결을 끊음 — AI 제공사는 요청만 보고 결제자는 알 수 없고, 결제 처리자는 결제 금액만 보고 내용은 알 수 없음.
- 사용자는 ETH·USDC를 보관함(vault)에 예치하고 신원 노출 없이 AI API 접근권을 부여받을 수 있음.
- 2026-02-11 이더리움재단 dAI 리드 Davide Crapis와 이더리움 공동창업자 Vitalik Buterin이 Ethereum Research에 게재한 "ZK API Usage Credits: LLMs and Beyond" 제안을 실제 메인넷 vault 컨트랙트로 구현.
- 문제의식: 현재 모든 AI API 호출에는 결제 신원이 결부돼 있어, 제공사가 수년치 프롬프트·응답·사용 패턴을 하나의 행동 프로필로 연결할 수 있다는 우려.

## 출처

- [Introducing zkAPI: private usage credits for any API - Ethereum Foundation Blog](https://blog.ethereum.org/2026/10/01/introducing-zkapi)
- [Ethereum Foundation launches zkAPI to let users pay for AI models without revealing identity - The Block](https://www.theblock.co/news/defi/2026-10-01-ethereum-foundation-launches-zkapi-417504)
