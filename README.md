> **[한국어 버전 →](./README_KR.md)**

<div align="center">

# AI-Powered Product Recall & Incident Management System

### Automation Pipeline · n8n · GPT-4 · Supabase · OECD Integration

[![Portfolio](https://img.shields.io/badge/Portfolio-Live%20Site-00CFA8?style=for-the-badge&logo=github)](https://houuya.github.io/ai-product-safety-automation/)
[![Award](https://img.shields.io/badge/2025%20Award-Proactive%20Administration%20Excellence-FFB400?style=for-the-badge)](https://houuya.github.io/ai-product-safety-automation/#award)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

[![n8n](https://img.shields.io/badge/Built%20with-n8n-FF6D5A)](https://n8n.io)
[![OpenAI](https://img.shields.io/badge/Powered%20by-OpenAI-412991)](https://openai.com)
[![Gemini](https://img.shields.io/badge/Powered%20by-Gemini-4285F4)](https://gemini.google.com)
[![Supabase](https://img.shields.io/badge/Database-Supabase-3ECF8E)](https://supabase.com)

</div>

---

> A full-stack AI automation system for government product recall management, integrating OECD GlobalRecalls, n8n workflows, Supabase data pipelines, multimodal AI analysis, and marketplace surveillance with real-time monitoring.

> **Transforming government operations through AI automation: 95% cost reduction, 83% time savings**

---

## 🎯 Overview

An end-to-end AI-powered automation system that:

- **Collects & manages** product recall information from 43 countries
- **Integrates** with OECD's Global Recall Portal for international data sharing
- **Monitors** online marketplaces for recalled products and incident signals
- **Detects** product safety incidents from news sources in real-time
- **Analyzes** consumer reviews and Q&A to identify early warning signs

**Built by a government official, not a professional developer** — demonstrating that modern AI and low-code tools can enable non-technical experts to build enterprise-grade automation systems.

### Key Achievements

| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| ⚡ Processing time / recall | 30 min | 5 min | **83% faster** |
| 💰 Annual outsourcing budget | $20,000 | $1,000 | **95% cost savings** |
| 🌍 Overseas recall coverage | 4 days lag | Real-time | **Instant** |
| 🛒 Marketplace surveillance | Manual | 24/7 Auto | **Fully automated** |
| 📰 Incident news detection | Daily manual | Every 2 hrs | **12x more frequent** |

---

## 🏆 2025 Recognition

**Ministry of Trade, Industry and Energy — Proactive Administration Excellence Award**

Selected as an outstanding case of proactive administration (Q3 2025) by a private expert review panel. Recognized for: *"Establishing an AI-based system for rapid collection, analysis, and dissemination of hazardous product information to ensure public safety."*

→ [View award details](https://houuya.github.io/ai-product-safety-automation/#award)

---

## 📊 System Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                   External Data Sources                      │
│  13 Gov't Recall Sites │ safetykorea.kr │ Marketplaces │ News│
└────────────┬─────────────────────────────────────────────────┘
             │
             ▼
┌──────────────────────────────────────────────────────────────┐
│              Data Collection Layer (n8n Triggers)            │
│     RSS Feeds │ REST APIs │ Web Scrapers │ Cron Schedules    │
└────────────┬─────────────────────────────────────────────────┘
             │
             ▼
┌──────────────────────────────────────────────────────────────┐
│           AI Processing Layer (n8n + GPT-4 / Gemini)         │
│  Translation │ GPC Classification │ Hazard Analysis │ Vision  │
└────────────┬─────────────────────────────────────────────────┘
             │
             ▼
┌───────────────────────┐    ┌───────────────────────────────┐
│  Supabase (PostgreSQL)│    │    Integration Outputs        │
│  Structured recall DB │    │  OECD Portal │ Telegram Alerts│
└───────────────────────┘    └───────────────────────────────┘
```

[View full architecture →](./ARCHITECTURE.md)

---

## 🚀 Key Features

### 1. Domestic Recall → OECD Registration

Automated pipeline for registering Korean product recalls to OECD Global Recall Portal.

**Process:**
1. Monitor national recall portal (`safetykorea.kr`) daily
2. Extract recall information via API
3. Translate to English using GPT-4
4. Assign GPC (Global Product Classification) codes automatically
5. Generate OECD-compliant XML
6. 5-minute human review → auto-submit to `globalrecalls.oecd.org`

**Impact:** Replaced 8-year outsourcing contract, saving $19K annually

[Learn more →](./docs/01-domestic-recall-oecd.md)

---

### 2. Overseas Recall Collection (43 Countries, Pilot)

Multi-language scraping and AI analysis of international recall data.

**Countries covered:** US (CPSC), Canada, Japan (METI + Recall Plus), China (SAMR), Australia (ACCC), New Zealand, EU Safety Gate (27 members)

**Process:**
1. Daily scraping of 13 government websites
2. Extract recall notices, images, PDFs
3. Translate (English, Chinese, Japanese, German, French)
4. Classify hazard types and chemical substances
5. Store in structured Supabase database
6. Real-time Telegram alerts for critical recalls

**Impact:** Processing time reduced from 4 days to <1 hour (75% reduction)

[Learn more →](./docs/02-overseas-recall-collection.md)

---

### 3. Marketplace Surveillance

AI-powered detection of recalled products on e-commerce platforms.

**Process:**
1. Cookie-based scraping of product listings (model numbers, images, reviews, Q&A)
2. Cross-reference with recall database
3. GPT-4 Vision multimodal analysis
   - Compare product images to recall notice images
   - Analyze consumer reviews for incident keywords
4. Generate risk assessment report → human final decision

**Impact:** 60% reduction in manual review time

[Learn more →](./docs/03-marketplace-surveillance.md)

---

### 4. News Monitoring

Automated product incident news detection and dissemination.

**Process:**
1. Monitor Naver News + Fire Safety journals every 2 hours
2. AI-powered incident classification (fire, explosion, electrocution, etc.)
3. Extract product details and hazard information
4. Distribute via Telegram to stakeholders instantly

[Learn more →](./docs/04-news-monitoring.md)

---

## 🛠 Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Automation** | n8n (cloud) | Workflow orchestration |
| **AI/ML** | ChatGPT, GPT-4 Vision, Gemini | Translation, classification, image analysis |
| **Database** | Supabase (PostgreSQL) | Structured data storage + REST API |
| **Scraping** | ScrapingBee, Puppeteer | Web data extraction with bot-bypass |
| **API** | REST, RSS, Webhooks | System integrations |
| **Alerts** | Telegram API | Real-time notifications |

> **Why Gemini?** Free tier with high rate limits — rapid PoC deployment without budget constraints.

---

## 📈 Results & Impact

### Quantitative
- **Processing time:** 4 days → <1 hour (75% reduction)
- **Cost per recall:** $76 → $3.80 (95% reduction)
- **Annual budget:** $20,000 → $1,000
- **Data freshness:** 15-day lag → Real-time
- **Coverage:** 43 countries, 4,000+ recalls/year

### Qualitative
- ✅ Eliminated 8-year vendor dependency (sustainable in-house system)
- ✅ Instant OECD data sharing (international cooperation model)
- ✅ Proactive marketplace surveillance (prevent re-distribution of recalled products)
- ✅ Built organizational AI transformation capacity (100+ staff trained)
- ✅ Demonstrated viability of government in-house development

---

## 🏢 Public-Private Ecosystem

**Product Safety Information Open Forum** — 50+ companies, 4 subcommittees

| Subcommittee | Members | Focus |
|---|---|---|
| E-Commerce Distribution | 25 internet shopping mall companies | AI-based distribution surveillance |
| Software | 10 software development companies | Workflow automation feedback |
| Product Safety Industry | 18 manufacturers, importers & distributors | Safety data utilization |
| Intermediary Distribution | 5 wholesale/retail intermediaries | B2B illegal product prevention |

---

## 🎓 Organizational AI Transformation (AX)

Beyond building systems, this project cultivated AI capabilities within the organization:

- 6 hands-on workshops (100+ staff trained)
- Low-code/no-code tool adoption across departments
- 27 AI reference books distributed

**Key Insight:** Civil servants can build AI solutions when given the right tools and training — breaking the "civil servants can't code" stereotype.

---

## 📂 Sample Workflows

This repository includes **de-identified sample workflows** for educational purposes:

1. [Domestic Recall OECD Registration](./workflows/domestic-recall-oecd-sample.json)
2. [Overseas Recall Collection (US CPSC)](./workflows/overseas-recall-sample.json)
3. [Marketplace Surveillance](./workflows/marketplace-surveillance-sample.json)

> ⚠️ **Note:** Sensitive data (API keys, credentials, internal URLs) have been removed.

---

## 🌏 International Standards Alignment

- **OECD Global Recall Portal** standards (XML schema)
- **GPC** GS1 Global Product Classification taxonomy
- **ISO/IEC standards** for data quality and governance
- **EU AI Act** principles (transparency, human oversight)

---

## 👤 About the Author

**Seungho Bae** | Deputy Director, Korea Agency for Technology and Standards (KATS)

- M.S. in Information and Communications (GIST, 2006)
- 10+ years in government product safety regulation
- Former 3GPP/IEEE standards, verification & certification expert (mobile communications)
- Recipient: Prime Minister's Commendation (2021)
- 2025 Ministry of Trade Proactive Administration Excellence (Q3)

**Background:** Not a software engineer — a domain expert who leveraged AI and automation tools to solve real-world government challenges.

---

## 📜 License

MIT License — see [LICENSE](./LICENSE). Documentation and de-identified samples only. Production credentials are not included.

---

## 🤝 Connect

- **Portfolio:** [houuya.github.io/ai-product-safety-automation](https://houuya.github.io/ai-product-safety-automation/)
- **GitHub:** [github.com/HouuYa](https://github.com/HouuYa)
- **Email:** givnhevn@gmail.com
- **Project:** [safetykorea.kr](https://www.safetykorea.kr)
