# Module 03 · 온라인 쇼핑몰 유통 감시

> **[English →](#english)** · [← README](../README_KR.md)

## 배경 및 문제

리콜 확정 제품이 쇼핑몰에서 계속 판매되는 사례 반복. 담당자가 직접 쇼핑몰 방문·모델번호 검색·수동 확인하는 구조는 **누적 리콜 증가에 비례하여 인력·시간 소요 증가**라는 구조적 한계 내재.

| 항목 | 이전 | 자동화 이후 |
|------|------|------------|
| 건당 분석시간 | 5분 | 2분 (60% 단축) |
| 이미지 비교 | 불가 | GPT-4 Vision 자동 |
| 사고 징후 탐지 | 불가 | 소비자 후기 AI 분석 |
| 확장성 | 인력 비례 | 리콜 증가와 무관하게 운영 |

## 자동화 프로세스

```
리콜 DB → 모델번호 목록 추출
    ▼
쿠팡·네이버쇼핑 검색 실행
  ScrapingBee + 쿠키 기반 인증 (봇 탐지 우회)
    ▼
상품페이지 수집 (제품명·이미지·후기·Q&A)
    │
    ├─ [분기1] GPT-4 Vision: 이미지 비교 + 모델명·KC번호 교차검증
    │
    └─ [분기2] AI: 후기·문의 사고 키워드 탐지
               (화재·폭발·감전·파손·부상 등)
    ▼
위해성 평가 보고서 자동 생성
    ▼
담당자 최종 판단 → 판매 중지 요청 등 후속 조치
```

## AI 분석 출력 예시

```json
{
  "recall_match_score": 87,
  "match_reason": "모델번호·이미지 리콜공표문과 일치",
  "incident_signals": "high — 후기에 연기 언급 3건",
  "recommended_action": "investigate"
}
```

## 성과

- 분석시간 **60% 단축**, AI+인력 2단계 검증으로 오탐 최소화
- 소비자 후기 기반 **사고 징후 사전 탐지** 신규 기능 (기존 불가)
- 누적 리콜 증가에도 추가 인력 없이 **24/7 지속 감시**

**관련 파일:** [`workflows/marketplace-surveillance-sample.json`](../workflows/marketplace-surveillance-sample.json)

---

<a name="english"></a>

# Module 03 · Online Marketplace Surveillance

> **[한국어 →](#배경-및-문제)** · [← README](../README.md)

## Background & Problem

Recalled products repeatedly resold on e-commerce platforms. Manual monitoring (visit site → search model number → verify) scales linearly with staff — a structural limitation as recalled product inventory grows.

| Metric | Before | After |
|--------|--------|-------|
| Analysis time/product | 5 min | 2 min (60% faster) |
| Image comparison | Not possible | GPT-4 Vision automated |
| Incident signal detection | Not possible | Consumer review AI analysis |
| Scalability | Grows with staff | AI-driven, unlimited |

## Process Flow

```
Recall DB → extract model number list
    ▼
Search Coupang / Naver Shopping
  ScrapingBee + cookie auth (bot bypass)
    ▼
Collect product page (name, images, reviews, Q&A)
    │
    ├─ [Branch 1] GPT-4 Vision: image comparison + model/KC cross-check
    │
    └─ [Branch 2] AI: review/Q&A incident keyword detection
                  (fire, explosion, shock, injury, etc.)
    ▼
Auto-generate risk assessment report
    ▼
Staff final decision → takedown request / follow-up
```

## Sample AI Output

```json
{
  "recall_match_score": 87,
  "match_reason": "Model number and images match recall notice",
  "incident_signals": "high — 3 reviews mention smoke",
  "recommended_action": "investigate"
}
```

## Impact

- **60% faster** analysis; dual AI+human validation minimises false positives
- **New capability:** incident signal detection from consumer reviews
- 24/7 continuous surveillance regardless of recalled product volume growth

**Related:** [`workflows/marketplace-surveillance-sample.json`](../workflows/marketplace-surveillance-sample.json)
