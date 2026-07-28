> **[한국어 →](./README_KR.md)**

<div align="center">

# AI-Powered Product Safety Automation

### Government product-safety operations, automated with n8n · LLMs · Supabase

[![2025 Results](https://img.shields.io/badge/2025-Results-00CFA8?style=for-the-badge)](https://houuya.github.io/ai-product-safety-automation/)
[![2026 Plan](https://img.shields.io/badge/2026-Plan-FFB400?style=for-the-badge)](https://houuya.github.io/ai-product-safety-automation/2026.html)

</div>

---

Automating how product recall and incident information is collected, classified, analyzed and shared — built in-house with low-code tools rather than outsourced development.

## 📖 Detail Pages

| | |
|---|---|
| **[2025 — What was built →](https://houuya.github.io/ai-product-safety-automation/)** | Four automation modules now in daily operation |
| **[2026 — What comes next →](https://houuya.github.io/ai-product-safety-automation/2026.html)** | Six sub-tasks as AI Sub-Agents, converging into one platform in 2027 |

## 2025 — Automation Modules

- [Domestic recall → OECD registration](./docs/01-domestic-recall-oecd.md)
- [Overseas recall collection (43 countries)](./docs/02-overseas-recall-collection.md)
- [Online marketplace surveillance](./docs/03-marketplace-surveillance.md)
- [Product incident news monitoring](./docs/04-news-monitoring.md)

## 2026 — Six Sub-Agents

Each sub-task covers one stage of the information chain — **Collect → Classify → Analyze → Diffuse** — and is designed as a module of the 2027 portal (the Master Agent).

| Stage | Sub-Task |
|---|---|
| Collect | Overseas recall interception · SNS incident-signal detection · AI call center for incident reports |
| Classify | Two-axis hazard classification (ISO 5665) |
| Analyze | KC safety standards digitization (RAG search) |
| Diffuse | Private-sector AX diffusion |

→ [See the full 2026 plan](https://houuya.github.io/ai-product-safety-automation/2026.html)

## 🛠 Tech Stack

n8n · ChatGPT / Gemini (incl. Vision) · Supabase (PostgreSQL) · ScrapingBee · Telegram API

[System architecture →](./ARCHITECTURE.md)

## 📂 Sample Workflow(n8n)

[Domestic Recall OECD Registration](./workflows/domestic-recall-oecd-sample.json) — de-identified sample.

> ⚠️ API keys, credentials and internal URLs have been removed.

## 📜 License

MIT License. Documentation and de-identified samples only.
