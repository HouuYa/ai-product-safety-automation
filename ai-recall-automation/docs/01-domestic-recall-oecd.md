# Domestic Recall → OECD Registration Automation

## Problem Statement

### Background
Since 2017, Korea has been sharing domestic product recall information with the OECD Global Recall Portal (`globalrecalls.oecd.org`). This portal serves as an international database for product safety information across 47+ countries.

### Manual Process Challenges
**Before automation (2017-2024):**
- **Outsourced to translation agency:** Annual contract $20,000
- **Processing time:** 15+ days delay per batch
- **Frequency:** Bi-weekly batches (falling behind)
- **Quality issues:** Inconsistent translation, human errors
- **Rigid process:** No flexibility for urgent recalls

**Pain points:**
1. Translation agency had no product safety domain knowledge
2. GPC (Global Product Classification) codes manually assigned
3. XML file generation required technical expertise
4. No automated change detection (updates/cancellations)
5. Expensive and slow

---

## Solution Architecture

### Workflow Overview

```
┌─────────────────────────────────────────────────────────────┐
│                     Daily Automation                        │
└─────────────────────────────────────────────────────────────┘

1️⃣  Monitor safetykorea.kr API
    ├─ Check for new recalls (9:00 AM daily)
    ├─ Check for updated recalls
    └─ Check for cancelled recalls

2️⃣  Extract Recall Data
    ├─ Product name & category
    ├─ Manufacturer info
    ├─ Hazard description
    ├─ Recall measures
    └─ Product images

3️⃣  AI-Powered Processing
    ├─ Translate to English (GPT-4)
    ├─ Assign GPC code (AI classification)
    ├─ Generate OECD-compliant XML
    └─ Download & process product images

4️⃣  Human Review (5 minutes)
    ├─ Review translation quality
    ├─ Verify GPC code assignment
    └─ Approve for submission

5️⃣  OECD Portal Submission
    ├─ POST XML via API
    ├─ Upload product images
    └─ Confirm successful registration

6️⃣  Record Keeping
    └─ Log submission in internal database
```

---

## Technical Implementation

### 1. API Monitoring (n8n Schedule Trigger)

```javascript
// Trigger: Daily at 9:00 AM KST
// Node: HTTP Request

GET https://www.safetykorea.kr/api/recalls
Headers:
  - Accept: application/json
  - Authorization: Bearer [API_KEY]
Parameters:
  - date_from: {{ $today() }}
  - status: active
```

**Response Example:**
```json
{
  "results": [
    {
      "recall_id": "2024-E-0123",
      "recall_type": "mandatory",
      "product_name": "이동식 전기히터",
      "manufacturer": "ABC전자",
      "hazard": "과열로 인한 화재 위험",
      "recall_date": "2024-11-01",
      "image_url": "https://safetykorea.kr/.../image.jpg"
    }
  ]
}
```

---

### 2. Data Extraction & Enrichment

**n8n Function Node:**
```javascript
// Extract and structure data
const recalls = $input.all();

return recalls.map(item => ({
  recall_number: item.json.recall_id,
  product_name_ko: item.json.product_name,
  manufacturer_ko: item.json.manufacturer,
  hazard_ko: item.json.hazard,
  recall_date: item.json.recall_date,
  images: [item.json.image_url],
  // Add metadata
  source_country: 'Korea',
  source_language: 'ko'
}));
```

---

### 3. AI-Powered Translation (GPT-4)

**Prompt Engineering:**
```
System: You are a professional translator specializing in product safety and recall notices. Translate the following Korean product recall information to English.

Guidelines:
- Maintain technical accuracy
- Use standard product safety terminology
- Keep product names transliterated (e.g., "이동식 전기히터" → "Portable Electric Heater")
- Preserve manufacturer names as-is unless widely known (e.g., Samsung, LG)

Input:
Product Name (KO): {{ $json.product_name_ko }}
Manufacturer (KO): {{ $json.manufacturer_ko }}
Hazard Description (KO): {{ $json.hazard_ko }}

Output Format (JSON):
{
  "product_name_en": "...",
  "manufacturer_en": "...",
  "hazard_description_en": "...",
  "translation_notes": "..."
}
```

**GPT-4 Configuration:**
```javascript
{
  model: "gpt-4-turbo-preview",
  temperature: 0.2,  // Low for consistency
  max_tokens: 1500,
  response_format: { type: "json_object" }
}
```

**Sample Output:**
```json
{
  "product_name_en": "Portable Electric Heater",
  "manufacturer_en": "ABC Electronics",
  "hazard_description_en": "Fire hazard due to overheating",
  "translation_notes": "Standard electrical appliance terminology used"
}
```

---

### 4. GPC Code Assignment (AI Classification)

**GPC (Global Product Classification):** UN-backed standard for product categorization used by OECD.

**AI Prompt:**
```
System: You are an expert in GPC (Global Product Classification) code assignment for product safety recalls.

Task: Assign the most appropriate 8-digit GPC code to this product.

Product Information:
- Name: {{ $json.product_name_en }}
- Category: {{ $json.category }}
- Description: {{ $json.hazard_description_en }}

GPC Reference (Top Categories):
- 10000000: Baby Care (e.g., cribs, strollers, toys)
- 52000000: Household Appliances (e.g., heaters, fans, vacuum cleaners)
- 53000000: Hardware (e.g., power tools, hand tools)
- 56000000: Electrical & Electronics (e.g., adapters, batteries, chargers)
- 57000000: Furniture (e.g., tables, chairs, shelving)

Response Format (JSON):
{
  "gpc_code": "52161500",
  "gpc_description": "Space Heaters/Electric Heaters",
  "confidence": "high",
  "reasoning": "Product is an electric heater, falls under Household Appliances > Space Heaters"
}
```

**Sample Output:**
```json
{
  "gpc_code": "52161500",
  "gpc_description": "Space Heaters/Electric Heaters",
  "confidence": "high",
  "reasoning": "Portable electric heater clearly falls into space heaters category"
}
```

**Validation:**
- Cross-check against official GPC database
- Flag low-confidence assignments for human review

---

### 5. OECD XML Generation

**XML Template (OECD Recall Schema v2.0):**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<recall xmlns="http://globalrecalls.oecd.org/v2.0">
  <recallId>{{ $json.recall_number }}</recallId>
  <country>KR</country>
  <recallDate>{{ $json.recall_date }}</recallDate>
  
  <product>
    <name>{{ $json.product_name_en }}</name>
    <manufacturer>{{ $json.manufacturer_en }}</manufacturer>
    <gpcCode>{{ $json.gpc_code }}</gpcCode>
    <images>
      <image>{{ $json.image_url }}</image>
    </images>
  </product>
  
  <hazard>
    <description>{{ $json.hazard_description_en }}</description>
    <type>{{ $json.hazard_type }}</type>
  </hazard>
  
  <recallMeasures>
    <measure>{{ $json.recall_measures_en }}</measure>
  </recallMeasures>
</recall>
```

**n8n XML Node:**
```javascript
// Generate XML from JSON
const xml = `<?xml version="1.0" encoding="UTF-8"?>
<recall xmlns="http://globalrecalls.oecd.org/v2.0">
  <recallId>${$json.recall_number}</recallId>
  <country>KR</country>
  <recallDate>${$json.recall_date}</recallDate>
  ...
</recall>`;

return { xml };
```

---

### 6. Human Review Interface (Google Sheets)

**Review Spreadsheet Columns:**
```
| Date | Recall # | Product Name (KO) | Product Name (EN) | GPC Code | Hazard (EN) | Status | Reviewer | Notes |
```

**n8n Google Sheets Integration:**
```javascript
// Append to review sheet
await googleSheets.append({
  spreadsheetId: 'xxx',
  range: 'OECD Review!A:I',
  values: [[
    new Date(),
    $json.recall_number,
    $json.product_name_ko,
    $json.product_name_en,
    $json.gpc_code,
    $json.hazard_description_en,
    'Pending Review',
    '',
    ''
  ]]
});
```

**Review Process:**
1. Reviewer checks translation quality (2 min)
2. Verifies GPC code is appropriate (1 min)
3. Checks image quality/relevance (1 min)
4. Marks status as "Approved" or "Needs Revision" (1 min)

**Total review time: ~5 minutes per recall**

---

### 7. OECD Portal Submission

**API Endpoint:**
```
POST https://globalrecalls.oecd.org/api/v2/recalls
```

**Headers:**
```javascript
{
  'Content-Type': 'application/xml',
  'Authorization': 'Bearer [OECD_API_KEY]',
  'X-Country-Code': 'KR',
  'X-Agency': 'KATS'
}
```

**Body:**
```xml
[Generated XML from step 5]
```

**Response:**
```json
{
  "status": "success",
  "oecd_recall_id": "OECD-2024-KR-0123",
  "url": "https://globalrecalls.oecd.org/recalls/OECD-2024-KR-0123"
}
```

**Error Handling:**
- Retry 3 times on failure
- Log errors to database
- Send Telegram alert to admin

---

## Results & Impact

### Quantitative Metrics

| Metric | Before (Manual) | After (AI) | Improvement |
|--------|----------------|-----------|-------------|
| **Processing Time** | 30 min/recall | 5 min/recall | **83% reduction** |
| **Batch Frequency** | 15 days | Real-time | **Real-time** |
| **Annual Cost** | $20,000 | $1,000 | **95% savings** |
| **Translation Quality** | Variable | Consistent | **Standardized** |
| **Staff Time** | 2 people | 1 person | **50% reduction** |

### Qualitative Benefits

✅ **Immediate international sharing:** Other countries can access Korean recall data within 24 hours

✅ **Flexibility:** Can prioritize urgent recalls (e.g., fire hazards, child products)

✅ **Change tracking:** Automated detection of updates and cancellations

✅ **Scalability:** Can handle 2x-3x growth without additional resources

✅ **Knowledge transfer:** System is maintainable by non-technical staff

✅ **Vendor independence:** No dependency on external contractors

---

## Sample Data Flow

**Example: Electric Heater Recall**

**Input (Korean):**
```
제품명: 이동식 전기히터 모델 EH-2024
제조사: ABC전자주식회사
위해내용: 장시간 사용 시 과열로 인한 화재 발생 위험
조치사항: 사용 중지 및 환불 조치
리콜일자: 2024-11-01
```

**AI Processing:**

1. **Translation (GPT-4):**
```
Product Name: Portable Electric Heater Model EH-2024
Manufacturer: ABC Electronics Co., Ltd.
Hazard: Fire hazard due to overheating during extended use
Remedy: Stop use immediately and return for full refund
Recall Date: 2024-11-01
```

2. **GPC Classification:**
```
Code: 52161500
Category: Space Heaters/Electric Heaters
Confidence: High
```

3. **XML Generation:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<recall xmlns="http://globalrecalls.oecd.org/v2.0">
  <recallId>2024-E-0123</recallId>
  <country>KR</country>
  <recallDate>2024-11-01</recallDate>
  <product>
    <name>Portable Electric Heater Model EH-2024</name>
    <manufacturer>ABC Electronics Co., Ltd.</manufacturer>
    <gpcCode>52161500</gpcCode>
  </product>
  <hazard>
    <description>Fire hazard due to overheating during extended use</description>
    <type>FIRE</type>
  </hazard>
  <recallMeasures>
    <measure>Stop use immediately and return for full refund</measure>
  </recallMeasures>
</recall>
```

4. **Human Review:** ✅ Approved (5 minutes)

5. **OECD Submission:** ✅ Success
   - OECD ID: OECD-2024-KR-0123
   - URL: https://globalrecalls.oecd.org/recalls/OECD-2024-KR-0123

---

## Lessons Learned

### What Worked Well
1. **GPT-4 translation quality:** Near-native English, consistent terminology
2. **Low-code approach:** Non-developers can maintain and modify
3. **Human-in-the-loop:** 5-minute review catches edge cases
4. **Incremental rollout:** Started with 1 recall/day, scaled to 10+/day

### Challenges & Solutions
1. **Challenge:** GPC codes have 5,000+ categories
   - **Solution:** Fine-tuned prompts with top 100 categories + examples

2. **Challenge:** Images sometimes missing or low quality
   - **Solution:** Fallback to text-only submission, flag for manual image upload

3. **Challenge:** Translation of brand names
   - **Solution:** Maintain brand name glossary, auto-suggest transliterations

4. **Challenge:** OECD API occasional timeouts
   - **Solution:** Retry logic with exponential backoff

---

## Code Availability

This workflow is **not open-sourced** due to:
- API keys and credentials
- Internal government URLs
- Sensitive product data

However, **de-identified samples** are available in [`/workflows`](../workflows/domestic-recall-oecd-sample.json) for educational purposes.

---

## Contact

For questions about this system:
- **Email:** sh.bae@kats.go.kr
- **Organization:** Korea Agency for Technology and Standards (KATS)

---

*Last updated: 2025-11-14*
