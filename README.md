> **[English →](./README_EN.md)**

<div align="center">

# AI 기반 제품안전 정보화 자동화

### 제품안전 행정 업무 자동화 — n8n · LLM · Supabase

[![2025 성과](https://img.shields.io/badge/2025-%EC%84%B1%EA%B3%BC-00CFA8?style=for-the-badge)](https://houuya.github.io/ai-product-safety-automation/)
[![2026 계획](https://img.shields.io/badge/2026-%EA%B3%84%ED%9A%8D-FFB400?style=for-the-badge)](https://houuya.github.io/ai-product-safety-automation/2026.html)

</div>

---

리콜·제품사고 정보의 수집·분류·분석·확산 업무를 외주 개발이 아닌 노코드 도구로 직접 자동화한 기록입니다.

## 📖 상세 페이지

| | |
|---|---|
| **[2025년 — 무엇을 만들었나 →](https://houuya.github.io/ai-product-safety-automation/)** | 현재 상시 운영 중인 자동화 4종 |
| **[2026년 — 무엇을 할 것인가 →](https://houuya.github.io/ai-product-safety-automation/2026.html)** | 6개 세부과제(AI Sub-Agent)와 2027년 통합 플랫폼 |

## 2025년 — 업무 자동화 4종

- [국내 리콜 → OECD 자동 등록](./docs/01-domestic-recall-oecd.md)
- [해외 리콜 정보 수집(43개국)](./docs/02-overseas-recall-collection.md)
- [온라인 쇼핑몰 유통 감시](./docs/03-marketplace-surveillance.md)
- [제품사고 뉴스 모니터링](./docs/04-news-monitoring.md)

## 2026년 — 6개 Sub-Agent

각 세부과제는 정보화 4단계 — **수집 → 분류·표준화 → 분석·검색 → 확산** — 중 한 단계를 담당하며, 2027년 포털(Master Agent)의 AI Sub-Agent 기능 모듈로 설계

| 단계 | 세부과제 |
|---|---|
| 수집 | 해외리콜 유통차단 · SNS 사고징후 선제포착 · 제품사고 접수 AI 콜센터 |
| 분류·표준화 | 위해요인 2차원 분류(ISO 5665) |
| 분석·검색 | KC안전기준 디지털화(RAG 검색) |
| 확산 | 민간 자율 AX 확산 |

`+` 기반과제([제품안전정보 포털](https://www.safetykorea.kr) 고도화, 2027년 구축을 목표로 예산 확보 중)

→ [2026년 추진계획 전체 보기](https://houuya.github.io/ai-product-safety-automation/2026.html)

## 🛠 기술 스택

n8n · ChatGPT / Gemini(Vision 포함) · Supabase(PostgreSQL) · ScrapingBee · Telegram API

[시스템 아키텍처 →](./ARCHITECTURE.md)

## 📂 샘플 워크플로우(n8n)

[국내 리콜 OECD 자동등록](./workflows/domestic-recall-oecd-sample.json) — 비식별화 샘플

> ⚠️ API 키·인증정보·내부 URL은 제거되어 있습니다.

## 📜 라이선스

MIT License. 문서와 비식별화 샘플만 포함합니다.
