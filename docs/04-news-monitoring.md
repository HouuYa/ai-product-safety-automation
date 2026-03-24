# Module 04 · 제품사고 뉴스 모니터링

> **[English →](#english)** · [← README](../README_KR.md)

## 배경 및 문제

담당자가 매일 포털 수동 검색 → 결과 정리 → 이메일 전달. 근무시간 외·주말은 사실상 탐지 불가. 인명피해 등 중대 사고는 보도 직후 **수 시간 내 초기 대응**이 중요하나 체계 미비.

| 항목 | 이전 | 자동화 이후 |
|------|------|------------|
| 모니터링 주기 | 하루 1~2회 (근무시간) | 2시간마다, 24/7 |
| 야간·주말 탐지 | 사실상 불가 | 자동 운영 |
| 전파 방식 | 이메일 (지연 발생) | Telegram 즉시 알림 |
| 사고 분류 | 수동 | 6개 유형 AI 자동 분류 |

## 모니터링 소스

- **네이버 뉴스** — 국내 주요 언론 통합 (키워드 RSS)
- **소방방재신문** — 화재·폭발 특화 매체
- **안전신문** — 제품안전 전문 매체

## 자동화 프로세스

```
[2시간마다] 스케줄 트리거
    ▼
뉴스 포털 키워드 검색
  (화재·폭발·감전·리콜·제품사고·어린이 안전 등)
    ▼
신규 기사 필터링 (URL 중복 체크)
    ▼
AI: 제품사고 관련성 판별 → 무관하면 폐기
    ▼ (관련 있음)
AI: 구조화 추출 {제품명, 사고유형, 피해내용, 출처}
    ▼
6개 유형 분류 (화재·폭발·감전·질식·낙상·화학)
    ▼
Telegram 채널 즉시 전송
```

## Telegram 알림 예시

```
🔴 [제품사고 알림] 화재

📌 제품명: A사 전기매트 B-200
📋 사고유형: 화재·화상
📝 내용: 취침 중 전기매트 화재, 1명 화상
📰 소방방재신문 (2025-10-15 07:32)
🔗 https://...
```

## 성과

- 모니터링 주기 **12배 향상** (하루 1~2회 → 2시간마다)
- 야간·주말 **24시간 자동 탐지** 체계 구축
- 보도 후 최대 **2시간 내** 담당자 수신

---

<a name="english"></a>

# Module 04 · Product Incident News Monitoring

> **[한국어 →](#배경-및-문제)** · [← README](../README.md)

## Background & Problem

Staff manually searched news portals, compiled results, and sent emails — only during working hours. Critical incidents (injuries, fires) require rapid response within hours of media coverage, but the gap was unaddressed nights and weekends.

| Metric | Before | After |
|--------|--------|-------|
| Monitoring cycle | 1–2×/day (work hours) | Every 2 hours, 24/7 |
| Night/weekend coverage | Essentially none | Fully automated |
| Dissemination | Email (delayed) | Telegram instant alert |
| Classification | Manual | 6 types, AI-automated |

## Sources

- **Naver News** — Major Korean media aggregator (keyword RSS)
- **Fire Safety News** — Fire & explosion specialist
- **Safety Newspaper** — Product safety specialist

## Process Flow

```
[Every 2 hours] Schedule trigger
    ▼
News portal keyword search
  (fire, explosion, shock, recall, product accident, child safety…)
    ▼
New article filter (URL dedup check)
    ▼
AI: Relevance check → discard if unrelated
    ▼ (relevant)
AI: Structured extraction {product, type, damage, source}
    ▼
6-type classification (fire / explosion / shock / choking / fall / chemical)
    ▼
Telegram instant notification
```

## Sample Alert

```
🔴 [Product Incident Alert] Fire

📌 Product: Brand A Electric Mat B-200
📋 Type: Fire / Burn injury
📝 Summary: Fire during sleep, 1 person burned
📰 Fire Safety News (2025-10-15 07:32)
🔗 https://...
```

## Impact

- Monitoring frequency **12× higher** (1–2×/day → every 2 hours)
- **24/7 automated detection** including nights and weekends
- Staff notified within **2 hours** of publication
