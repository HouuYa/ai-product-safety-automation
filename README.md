# AI-Powered Product Recall Information Management System

> **Transforming government operations through AI automation: 95% cost reduction, 83% time savings**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![n8n](https://img.shields.io/badge/Built%20with-n8n-FF6D5A)](https://n8n.io)
[![OpenAI](https://img.shields.io/badge/Powered%20by-OpenAI-412991)](https://openai.com)

## 🎯 Overview

An end-to-end AI-powered automation system that manages product recall information across 43 countries, integrates with OECD's Global Recall Portal, and monitors online marketplaces for recalled products.

**Built by a government official, not a professional developer** — demonstrating that modern AI and low-code tools can enable non-technical experts to build enterprise-grade automation systems.

### Key Achievements
- ⚡ **83% reduction** in processing time (30 min → 5 min per recall)
- 💰 **95% cost savings** ($20K → $1K annually)
- 🌍 **43 countries** automated recall data collection
- 🔄 **Real-time** OECD portal integration
- 🤖 **AI-driven** marketplace surveillance with multimodal analysis

---

## 📊 System Architecture

```
┌─────────────────┐    ┌──────────────┐    ┌─────────────────┐
│  13 Government  │───▶│  n8n + AI    │───▶│ OECD Portal     │
│  Recall Sites   │    │  Pipeline    │    │ (47 countries)  │
└─────────────────┘    └──────────────┘    └─────────────────┘
         │                     │                      │
         ▼                     ▼                      ▼
   ┌──────────┐         ┌──────────┐         ┌──────────────┐
   │ Web      │────────▶│ Supabase │◀────────│ Marketplace  │
   │ Scraping │         │ Database │         │ Surveillance │
   └──────────┘         └──────────┘         └──────────────┘
```

[View detailed architecture →](./ARCHITECTURE.md)

---

## 🚀 Key Features

### 1. Domestic Recall → OECD Registration
Automated pipeline for registering Korean product recalls to OECD Global Recall Portal.

**Process:**
1. Monitor national recall portal (`safetykorea.kr`) daily
2. Extract recall information via API
3. Translate to English using GPT-4
4. Assign GPC (Global Product Classification) codes
5. Generate OECD-compliant XML
6. Auto-submit to `globalrecalls.oecd.org`

**Impact:** Replaced 8-year outsourcing contract, saving $19K annually

[Learn more →](./docs/01-domestic-recall-oecd.md)

---

### 2. Overseas Recall Collection (43 Countries)
Multi-language scraping and AI analysis of international recall data.

**Countries covered:** US, Canada, Japan, China, Australia, New Zealand, EU (27 members)

**Process:**
1. Daily scraping of 13 government websites
2. Extract recall notices, images, PDFs
3. Translate (English, Chinese, Japanese, German, French)
4. Classify hazard types and chemical substances
5. Store in structured database
6. Real-time Telegram alerts for critical recalls

**Impact:** Processing time reduced from 4 days to <1 hour

[Learn more →](./docs/02-overseas-recall-collection.md)

---

### 3. Marketplace Surveillance
AI-powered detection of recalled products on e-commerce platforms.

**Process:**
1. Scrape product listings (model numbers, images, reviews)
2. Cross-reference with recall database
3. Multimodal AI analysis (GPT-4 Vision)
   - Compare product images
   - Analyze consumer reviews for incident signals
4. Generate risk assessment report

**Impact:** 60% reduction in manual review time

[Learn more →](./docs/03-marketplace-surveillance.md)

---

### 4. News Monitoring
Automated product incident news detection and dissemination.

**Process:**
1. Monitor news portals every 2 hours
2. AI-powered incident classification
3. Extract product details and hazard information
4. Distribute via Telegram to stakeholders

[Learn more →](./docs/04-news-monitoring.md)

---

## 🛠 Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Automation** | n8n (self-hosted) | Workflow orchestration |
| **AI/ML** | GPT-4, GPT-4 Vision | Translation, classification, analysis |
| **Database** | Supabase (PostgreSQL) | Structured data storage |
| **Scraping** | ScrapingBee, Puppeteer | Web data extraction |
| **API** | REST, RSS, Webhooks | System integrations |
| **Alerts** | Telegram API | Real-time notifications |

---

## 📈 Results & Impact

### Quantitative
- **Processing time:** 4 days → <1 hour (75% reduction)
- **Cost per recall:** $76 → $3.80 (95% reduction)
- **Annual budget:** $20,000 → $1,000
- **Data freshness:** 15-day lag → Real-time
- **Coverage:** 43 countries, 4,000+ recalls/year

### Qualitative
- ✅ Eliminated vendor dependency
- ✅ Instant OECD data sharing (international cooperation model)
- ✅ Enabled proactive marketplace surveillance
- ✅ Built organizational AI transformation capacity
- ✅ Demonstrated viability of government in-house development

---

## 🏢 Public-Private Ecosystem

**Product Safety Information Open Forum**
- 50+ companies across 4 subcommittees
- Knowledge sharing on AI-based safety management
- Developed voluntary industry standards

**Government-Industry Collaboration:**
- Coupang, Kakao, 11st (online marketplaces)
- Daiso, Hanssem (retailers)
- ECT Compliance, Dalpha (AI solution providers)

---

## 🎓 Organizational AI Transformation (AX)

Beyond building systems, this project cultivated AI capabilities within the organization:

- 6 hands-on workshops (100+ staff trained)
- Low-code/no-code tool adoption
- 27 AI reference books distributed
- Government Innovation Award (2025)

**Key Insight:** Civil servants can build AI solutions when given the right tools and training.

---

## 📂 Sample Workflows

This repository includes **de-identified sample workflows** for educational purposes:

1. [Domestic Recall OECD Registration](./workflows/domestic-recall-oecd-sample.json)
2. [Overseas Recall Collection (US CPSC)](./workflows/overseas-recall-sample.json)
3. [Marketplace Surveillance](./workflows/marketplace-surveillance-sample.json)

> ⚠️ **Note:** Sensitive data (API keys, credentials, internal URLs) have been removed.

---

## 🌏 International Standards Alignment

This system aligns with:
- **OECD Global Recall Portal** standards (XML schema)
- **GPC (Global Product Classification)** taxonomy
- **ISO/IEC standards** for data quality and governance
- **EU AI Act** principles (transparency, human oversight)

---

## 👤 About the Author

**Seungho Bae** | Deputy Director, Korea Agency for Technology and Standards (KATS)

- M.S. in Information and Communications (GIST, 2006)
- 10+ years in government product safety regulation
- Former 3GPP/IEEE standards expert (mobile communications)
- Recipient: Prime Minister's Commendation (2021)

**Background:**
Not a software engineer by training, but a telecom standards expert who learned to leverage AI and automation tools to solve real-world government challenges.

[View full CV](./about.md)

---

## 📜 License

This project documentation is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

**Note:** This repository contains documentation and de-identified samples only. Production code and sensitive data are not included.

---

## 🤝 Connect

- LinkedIn: [linkedin.com/in/seungho-bae](#)
- Email: [sh.bae@example.com](#)
- Project: Korea Product Safety Information Portal ([safetykorea.kr](https://www.safetykorea.kr))

---

## 🎯 Key Takeaways for Technical Recruiters

1. **Problem-solving mindset:** Identified inefficiencies and designed solutions
2. **Technical execution:** Built production systems using modern AI/automation stack
3. **Data governance:** Managed cross-border data flows and international standards
4. **Change management:** Led organizational transformation (50+ companies, 100+ staff)
5. **Quantifiable impact:** 95% cost reduction, 83% time savings, 4,000+ recalls/year

**This demonstrates capability to:**
- Design data pipelines for government-scale operations
- Integrate AI into mission-critical workflows
- Balance automation with human oversight
- Navigate complex regulatory environments
- Build sustainable, maintainable systems without vendor lock-in

---

*Built with ❤️ by a civil servant who believes government can innovate*
