# Module 01 · 국내 리콜 → OECD 자동 등록

> **[English →](#english)** · [← README](../README_KR.md)

## 배경 및 문제

국내 제품 리콜 발생 시 OECD 글로벌 리콜 포털(`globalrecalls.oecd.org`) 등록을 **8년간 외주에 의존**, 연 2,000만원 지출. 번역·XML 변환·제출이 반복적·규칙적임에도 자동화 없이 인력 투입 구조가 고착화.

| 항목 | 이전 | 자동화 이후 |
|------|------|------------|
| 연간 비용 | 2,000만원 | 100만원 |
| 건당 처리시간 | 30분 | 5분 (검토만) |
| 등록 적시성 | 수일~수주 | 당일 등록 |
| 의존 구조 | 8년 외주 계약 | 자체 운영 |

## 자동화 프로세스

```
[매일 오전] safetykorea.kr API 수집
    │
    ├─ 이미지 다운로드 & OECD 규격 리사이징
    ▼
GPT-4: 한→영 번역 (기술 용어 특화)
    ▼
AI: GS1 GPC 코드 자동 분류
    ▼
OECD XML 생성 + 스키마 검증
    ▼
담당자 검토 (5분) → globalrecalls.oecd.org 자동 제출
```

## 성과

- 비용 **95% 절감** (2,000만 → 100만원), 처리시간 **83% 단축**
- 외주 8년 계약 종료, 리콜 당일 OECD 등록 체계 구축
- 연 300건+ 자동 처리

**관련 파일:** [`workflows/domestic-recall-oecd-sample.json`](../workflows/domestic-recall-oecd-sample.json)

---

<a name="english"></a>

# Module 01 · Domestic Recall → OECD Auto-Registration

> **[한국어 →](#배경-및-문제)** · [← README](../README.md)

## Background & Problem

For 8 years, registering Korean product recalls to the OECD Global Recall Portal relied on an outsourced contractor ($20K/year). The work — translation, XML conversion, submission — was repetitive and rule-based, yet never automated.

| Metric | Before | After |
|--------|--------|-------|
| Annual cost | $20,000 | $1,000 |
| Time per recall | 30 min | 5 min (review only) |
| Registration lag | Days to weeks | Same day |
| Dependency | 8-yr outsource | In-house automation |

## Process Flow

```
[Daily AM] safetykorea.kr API pull
    │
    ├─ Image download & OECD-spec resize
    ▼
GPT-4: KR → EN translation (technical terminology)
    ▼
AI: GS1 GPC code auto-classification
    ▼
OECD XML generation + schema validation
    ▼
5-min staff review → auto-submit to globalrecalls.oecd.org
```

## Impact

- **95% cost reduction** ($20K → $1K/yr), **83% time savings**
- 8-year outsourcing contract terminated; same-day OECD registration
- 300+ recalls/year processed automatically

**Related:** [`workflows/domestic-recall-oecd-sample.json`](../workflows/domestic-recall-oecd-sample.json)
