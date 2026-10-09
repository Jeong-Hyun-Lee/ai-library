---
type: source
tags: [anthropic, security, open-source]
created: 2026-10-08
updated: 2026-10-08
sources:
  - https://www.anthropic.com/research/launching-opt-in-vuln-finding-service-for-open-source
  - https://siliconangle.com/2026/10/08/anthropic-launches-critical-infrastructure-program-and-free-oss-scanner-for-open-source/
auto: true
---

# Anthropic, "Cyber Mission" 출범 — 무료 오픈소스 취약점 스캐너 "OSS Scanner" 공개

[[claude|Anthropic]]이 2026-10-08 보안 이니셔티브 "Anthropic Cyber Mission"을 출범하며, 오픈소스 프로젝트를 위한 무료 옵트인 취약점 스캐너 "OSS Scanner"를 공개했다. Google의 OSS-Fuzz를 참고해 설계.

## 핵심 내용

- 등록된 프로젝트는 Anthropic 최상위 모델로부터 주기적 스캔을 받으며, 각 보고서에는 익스플로잇 가능성을 보여주는 PoC·설명·가능하면 수정안까지 포함. 버그 도입 시점을 찾는 바이섹션 기능도 포함.
- 보고서는 사람 검토 없이 모델이 생성해 바로 전달되며, Anthropic은 진양성률(true-positive rate)이 90% 이상일 것으로 예상 — 다만 유지관리자의 자체 검토가 필요.
- 핵심 인프라·보안 민감 프로젝트를 중심으로 OSS-Fuzz와 유사한 기준으로 GitHub 기반 신청을 통해 프로젝트별 심사. Defender Advantage Fund(0xDAF)가 자금 지원.
- 동시에 "Critical Infrastructure Defense Program" 출범 — Accenture·CrowdStrike·Deloitte·PwC 등 11개 보안 서비스 기업에 Claude 모델과 상주 엔지니어를 제공해 전력망·수도·교통망 방어 지원.
- 이전 "Project Glasswing"을 통해 수백 개 주요 오픈소스 프로젝트를 스캔해 비공개로 결과를 전달한 바 있으며, 미등록 프로젝트에는 기존 조정된 취약점 공개(coordinated disclosure) 정책을 계속 적용.

## 출처

- [An opt-in vulnerability-finding service for open-source software - Anthropic](https://www.anthropic.com/research/launching-opt-in-vuln-finding-service-for-open-source)
- [Anthropic launches critical infrastructure program and free OSS Scanner for open source - SiliconANGLE](https://siliconangle.com/2026/10/08/anthropic-launches-critical-infrastructure-program-and-free-oss-scanner-for-open-source/)
