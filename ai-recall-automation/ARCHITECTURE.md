# System Architecture

## Overview

This document provides a detailed technical architecture of the AI-powered product recall information management system.

---

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     External Data Sources                       │
├─────────────────────────────────────────────────────────────────┤
│  • 13 Government Recall Websites (US, EU, JP, CN, AU, NZ, etc) │
│  • Korea Product Safety Portal (safetykorea.kr)                 │
│  • Online Marketplaces (Coupang, Naver Shopping, etc)          │
│  • News Portals (Naver News, Fire Safety News, etc)            │
└────────────────────┬────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Data Collection Layer                        │
├─────────────────────────────────────────────────────────────────┤
│  • RSS Feed Readers          • Web Scrapers (ScrapingBee)      │
│  • REST API Clients          • Browser Automation (Puppeteer)  │
│  • Scheduled Triggers (Cron) • Webhook Receivers               │
└────────────────────┬────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                   Processing Layer (n8n)                        │
├─────────────────────────────────────────────────────────────────┤
│  • HTML Parsing & Extraction                                    │
│  • Data Transformation & Normalization                          │
│  • AI-Powered Analysis (GPT-4, GPT-4 Vision)                   │
│  • Multi-language Translation (5 languages)                     │
│  • Classification & Tagging                                     │
│  • Quality Validation & Error Handling                          │
└────────────────────┬────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                     Data Storage Layer                          │
├─────────────────────────────────────────────────────────────────┤
│  • Supabase (PostgreSQL): Structured recall data                │
│  • Vector Database: AI-friendly recall embeddings               │
│  • File Storage: Images, PDFs, attachments                      │
└────────────────────┬────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Integration Layer                            │
├─────────────────────────────────────────────────────────────────┤
│  • OECD Global Recall Portal (XML API)                          │
│  • National Product Safety Portal (REST API)                    │
│  • Telegram (Real-time Alerts)                                  │
│  • Google Sheets (Reporting & Review)                           │
└────────────────────┬────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                   Human Review Interface                        │
├─────────────────────────────────────────────────────────────────┤
│  • Staff Review Dashboard (Google Sheets)                       │
│  • Quality Assurance Checkpoints                                │
│  • Manual Override Capability                                   │
└─────────────────────────────────────────────────────────────────┘
```

---

## Component Details

### 1. Data Collection Layer

#### 1.1 Recall Website Monitoring
- **Trigger:** Scheduled (daily at 9:00 AM KST)
- **Method:** RSS feeds, REST APIs, web scraping
- **Coverage:**
  - US CPSC (Consumer Product Safety Commission)
  - US NHTSA (Vehicle recalls)
  - Health Canada
  - EU Safety Gate
  - Japan METI, Recall Plus
  - China SAMR
  - Australia ACCC
  - New Zealand Commerce Commission
  - OECD Global Recall Portal

#### 1.2 Domestic Recall Monitoring
- **Source:** safetykorea.kr API
- **Frequency:** Every 15 minutes
- **Data:** New recalls, updates, cancellations

#### 1.3 Marketplace Scraping
- **Platforms:** Coupang, Naver Shopping, 11st
- **Method:** Cookie-based authentication + ScrapingBee
- **Data:** Product listings, images, reviews, Q&A

---

### 2. Processing Layer (n8n Workflows)

#### 2.1 Domestic Recall → OECD Pipeline

```
┌─────────────┐    ┌──────────────┐    ┌──────────────┐
│  Monitor    │───▶│   Extract    │───▶│  Translate   │
│ safetykorea │    │ Recall Data  │    │  (GPT-4)     │
└─────────────┘    └──────────────┘    └──────────────┘
                           │
                           ▼
                   ┌──────────────┐    ┌──────────────┐
                   │  Assign GPC  │───▶│ Generate XML │
                   │  Code (AI)   │    │  (OECD fmt)  │
                   └──────────────┘    └──────────────┘
                           │
                           ▼
                   ┌──────────────┐    ┌──────────────┐
                   │Human Review  │───▶│ Submit OECD  │
                   │  (5 min)     │    │   Portal     │
                   └──────────────┘    └──────────────┘
```

**Key Technologies:**
- **Translation:** GPT-4 Turbo (context-aware, maintains technical accuracy)
- **GPC Classification:** Fine-tuned prompts + GPC taxonomy lookup
- **XML Generation:** Template-based with dynamic field population
- **Validation:** Schema validation + human review

---

#### 2.2 Overseas Recall Collection Pipeline

```
┌─────────────┐    ┌──────────────┐    ┌──────────────┐
│  Scrape 13  │───▶│  Extract     │───▶│  Translate   │
│  Websites   │    │  Recall Info │    │  Multi-lang  │
└─────────────┘    └──────────────┘    └──────────────┘
       │                                        │
       │ (images, PDFs)                        ▼
       │                                ┌──────────────┐
       └───────────────────────────────▶│  Classify    │
                                        │  Hazards     │
                                        └──────────────┘
                                               │
                                               ▼
                                        ┌──────────────┐
                                        │ Store in DB  │
                                        │  + Telegram  │
                                        └──────────────┘
```

**Hazard Classification (AI-powered):**
- Chemical substances (phthalates, lead, cadmium, etc.)
  - Substance name
  - Detected amount
  - Regulatory limit
- Physical hazards (choking, laceration, burn, etc.)
- Electrical hazards (shock, fire, overheating, etc.)

**Multi-language Support:**
- English (US, Canada, Australia, NZ, EU)
- Chinese (Simplified - China)
- Japanese (Japan)
- German (Germany, Austria, Switzerland)
- French (France, Belgium, Canada)

---

#### 2.3 Marketplace Surveillance Pipeline

```
┌─────────────┐    ┌──────────────┐    ┌──────────────┐
│  Search by  │───▶│  Scrape Full │───▶│  Multimodal  │
│ Model Number│    │  Product Page│    │  AI Analysis │
└─────────────┘    └──────────────┘    └──────────────┘
                           │                    │
                           │                    ▼
                    (images, text,      ┌──────────────┐
                     reviews, Q&A)      │ Cross-Check  │
                                        │  Recall DB   │
                                        └──────────────┘
                                               │
                                               ▼
                                        ┌──────────────┐
                                        │  Risk Report │
                                        │  + Follow-up │
                                        └──────────────┘
```

**GPT-4 Vision Analysis:**
1. **Product Matching:**
   - Extract manufacturer, model number, KC certification number
   - Compare product images with recall notice images
   - Analyze product descriptions

2. **Consumer Sentiment Analysis:**
   - Scan reviews for incident keywords
   - Detect patterns: "overheating", "broke", "injured", "unsafe"
   - Flag products with multiple incident reports

3. **Risk Assessment:**
   - Match score (0-100%)
   - Incident signal strength (low/medium/high)
   - Recommended action (monitor/investigate/block)

---

### 3. Data Storage Layer

#### 3.1 Supabase Database Schema (Simplified)

```sql
-- Overseas Recalls
CREATE TABLE overseas_recalls (
    id UUID PRIMARY KEY,
    country VARCHAR(50),
    source_url TEXT,
    recall_date DATE,
    product_name TEXT,
    model_number TEXT,
    manufacturer TEXT,
    hazard_type VARCHAR(100),
    hazard_description TEXT,
    images TEXT[],
    pdf_url TEXT,
    translated_ko TEXT,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Domestic Recalls
CREATE TABLE domestic_recalls (
    id UUID PRIMARY KEY,
    recall_number VARCHAR(50) UNIQUE,
    recall_type VARCHAR(20), -- 'mandatory' or 'voluntary'
    product_category VARCHAR(100),
    product_name TEXT,
    manufacturer TEXT,
    recall_reason TEXT,
    oecd_submitted BOOLEAN DEFAULT FALSE,
    oecd_id VARCHAR(100),
    created_at TIMESTAMP DEFAULT NOW()
);

-- Marketplace Surveillance
CREATE TABLE marketplace_checks (
    id UUID PRIMARY KEY,
    platform VARCHAR(50), -- 'coupang', 'naver', '11st'
    product_url TEXT,
    model_number VARCHAR(100),
    is_recalled BOOLEAN,
    match_confidence DECIMAL(5,2),
    incident_signals TEXT[],
    checked_at TIMESTAMP DEFAULT NOW()
);
```

---

### 4. AI Integration Details

#### 4.1 GPT-4 Turbo Configuration
```javascript
{
  "model": "gpt-4-turbo-preview",
  "temperature": 0.2,  // Low for consistency
  "max_tokens": 4000,
  "top_p": 0.9
}
```

**Use Cases:**
- Translation (context-aware, technical terms)
- Classification (hazard types, product categories)
- Extraction (structured data from unstructured text)

#### 4.2 GPT-4 Vision Configuration
```javascript
{
  "model": "gpt-4-vision-preview",
  "temperature": 0.1,  // Very low for accuracy
  "max_tokens": 1000,
  "detail": "high"     // High-resolution image analysis
}
```

**Use Cases:**
- Product image comparison
- OCR (extract text from product labels)
- Visual hazard detection

---

### 5. Error Handling & Resilience

#### 5.1 Retry Logic
- **Web scraping failures:** 3 retries with exponential backoff
- **AI API errors:** 2 retries with 5-second delay
- **Database errors:** Immediate retry + fallback to CSV export

#### 5.2 Data Quality Checks
- **Schema validation:** All data validated before storage
- **Duplicate detection:** Check by recall number / source URL
- **Completeness check:** Flag records missing critical fields

#### 5.3 Human Oversight
- **Daily review queue:** Flagged items sent to Google Sheets
- **Alert thresholds:** High-risk items trigger immediate Telegram alerts
- **Audit trail:** All AI decisions logged for review

---

### 6. Security & Privacy

#### 6.1 Data Handling
- **Personal data:** Removed during scraping (names, emails, phone numbers)
- **Commercial data:** Used only for government safety purposes
- **API keys:** Stored in environment variables, never in code

#### 6.2 Access Control
- **n8n workflows:** Password-protected, IP-restricted
- **Database:** Row-level security (RLS) enabled
- **OECD portal:** Dedicated API credentials per user

---

### 7. Scalability Considerations

#### 7.1 Current Capacity
- **Recalls processed:** 4,000+ per year (100 per day peak)
- **Marketplace checks:** 200+ products per day
- **News monitoring:** 48 checks per day (every 2 hours)

#### 7.2 Scaling Strategy
- **Horizontal:** Add more n8n worker nodes
- **Vertical:** Increase AI API rate limits
- **Caching:** Cache GPC classifications and translations

---

### 8. Monitoring & Observability

#### 8.1 Metrics
- Workflows executed / failed per day
- Average processing time per recall
- AI API usage and costs
- Database storage growth

#### 8.2 Alerts
- **Telegram:** Critical failures, high-risk recalls
- **Email:** Daily summary reports
- **Google Sheets:** Review queues for staff

---

## Technology Choices: Rationale

| Technology | Why Chosen |
|-----------|-----------|
| **n8n** | Open-source, self-hosted, visual workflow builder (no vendor lock-in) |
| **GPT-4** | Best-in-class multilingual translation and structured extraction |
| **Supabase** | PostgreSQL + real-time + generous free tier |
| **ScrapingBee** | Handles JavaScript rendering, rotating proxies, CAPTCHA solving |
| **Telegram** | Instant notifications, easy API, mobile-friendly |

---

## Future Enhancements

1. **Vector DB Integration:** Enable semantic search over recall descriptions
2. **MCP Protocol:** Allow external AI agents to query recall data
3. **Real-time Dashboard:** Public-facing recall trends visualization
4. **Predictive Analytics:** Identify products at risk of future recalls
5. **Multi-agency Expansion:** Extend to food, pharma, vehicle recalls

---

*Last updated: 2025-11-14*
