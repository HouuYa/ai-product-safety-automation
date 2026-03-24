# Module 02 · 해외 리콜 정보 수집 (43개국, 시범 구축)

> **[English →](#english)** · [← README](../README_KR.md)

## 배경 및 문제

담당자가 각국 정부 사이트를 직접 방문·번역·정리하는 수작업 구조. 연간 20%↑ 증가하는 해외 리콜 정보를 처리하는 데 최대 4일 소요, 정보 공유 지연 고착화.

| 항목 | 이전 | 자동화 이후 |
|------|------|------------|
| 처리 시간 | 최대 4일 | 1시간 이내 |
| 수집 국가 | 수동 선별 | 43개국 자동 (시범) |
| 데이터 신선도 | 15일 지연 | 당일 수집 |
| 언어 처리 | 한국어만 | 5개 언어 자동 번역 |

## 수집 대상 (주요국)

| 국가/지역 | 기관 | 수집 방식 |
|----------|------|----------|
| 미국 | CPSC, NHTSA | RSS 피드 |
| 캐나다 | Health Canada | RSS 피드 |
| 일본 | METI, 리콜플러스 | 웹 스크래핑 + JS 렌더링 |
| 중국 | SAMR | 웹 스크래핑 |
| EU | Safety Gate (RAPEX) | 웹 스크래핑 |
| 호주·뉴질랜드 | ACCC, Commerce Comm. | 웹 스크래핑 |
| OECD | 글로벌 리콜 포털 | RSS 피드 |

## 자동화 프로세스

```
[매일 오전] 13개 사이트 동시 수집 트리거
    │
    ├─ RSS 파싱 (미국·캐나다·OECD)
    └─ 웹 스크래핑 (일본·중국·EU·호주 등)
         ScrapingBee: 봇 방지 우회
         Puppeteer: JS 렌더링 필요 사이트
    ▼
Gemini / GPT-4: 5개 언어 → 한국어 번역
    ▼
AI: 위해 유형 분류 (화학·물리·전기)
    ▼
Supabase DB 저장 → 중대 리콜 Telegram 즉시 알림
```

## 성과

- 처리시간 **75% 단축** (4일 → 1시간 이내)
- 연 4,000건+ 자동 처리, 15일 정보 지연 → 실시간 전환

**관련 파일:** [`workflows/overseas-recall-sample.json`](../workflows/overseas-recall-sample.json)

---

<a name="english"></a>

# Module 02 · Overseas Recall Collection (43 Countries, Pilot)

> **[한국어 →](#배경-및-문제)** · [← README](../README.md)

## Background & Problem

Staff manually visited foreign government websites, translated, and entered data — taking up to 4 days per cycle as overseas recall volume grew 20%+ annually.

| Metric | Before | After |
|--------|--------|-------|
| Processing time | Up to 4 days | < 1 hour |
| Country coverage | Manual selection | 43 countries automated (pilot) |
| Data freshness | 15-day lag | Same-day |
| Languages | Korean only | 5 languages auto-translated |

## Sources

| Country/Region | Agency | Method |
|---|---|---|
| US | CPSC, NHTSA | RSS feed |
| Canada | Health Canada | RSS feed |
| Japan | METI, Recall Plus | Scraping + JS rendering |
| China | SAMR | Web scraping |
| EU | Safety Gate (RAPEX) | Web scraping |
| Australia/NZ | ACCC, Commerce Comm. | Web scraping |
| OECD | Global Recall Portal | RSS feed |

## Process Flow

```
[Daily AM] Trigger 13 sites simultaneously
    │
    ├─ RSS parsing (US, Canada, OECD)
    └─ Web scraping (Japan, China, EU, Australia)
         ScrapingBee: bot bypass
         Puppeteer: JS-rendered sites
    ▼
Gemini / GPT-4: 5-language → Korean translation
    ▼
AI: Hazard classification (chemical / physical / electrical)
    ▼
Supabase DB storage → Critical recalls: Telegram instant alert
```

## Impact

- **75% time reduction** (4 days → < 1 hour)
- 4,000+ recalls/year processed; 15-day lag eliminated

**Related:** [`workflows/overseas-recall-sample.json`](../workflows/overseas-recall-sample.json)
