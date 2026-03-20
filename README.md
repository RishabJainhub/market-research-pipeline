<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1218,50:4c1d95,100:8b5cf6&height=220&section=header&text=AI%20Market%20Research%20Pipeline&fontSize=38&fontColor=ffffff&animation=twinkling&fontAlignY=35&desc=n8n%20Automation%20%7C%20Groq%20LLaMA%203.3%20%7C%20Tavily%20Search%20API&descSize=16&descAlignY=55&descColor=cccccc" width="100%" />

  **Enterprise-Grade AI Automation for Market Intelligence powered by n8n, Groq LLaMA 3.3, and Tavily Search API.**

  <p align="center">
    <a href="https://github.com/RishabJainhub/market-research-pipeline/stargazers"><img src="https://img.shields.io/github/stars/RishabJainhub/market-research-pipeline?color=4c1d95&labelColor=0d1218&style=for-the-badge" alt="stars"></a>
    <a href="https://github.com/RishabJainhub/market-research-pipeline/network/members"><img src="https://img.shields.io/github/forks/RishabJainhub/market-research-pipeline?color=4c1d95&labelColor=0d1218&style=for-the-badge" alt="forks"></a>
    <a href="https://github.com/RishabJainhub/market-research-pipeline/issues"><img src="https://img.shields.io/github/issues/RishabJainhub/market-research-pipeline?color=4c1d95&labelColor=0d1218&style=for-the-badge" alt="issues"></a>
    <a href="https://github.com/RishabJainhub/market-research-pipeline/blob/main/LICENSE"><img src="https://img.shields.io/github/license/RishabJainhub/market-research-pipeline?color=4c1d95&labelColor=0d1218&style=for-the-badge" alt="license"></a>
  </p>

  [📖 Architecture](#architecture) · [🚀 Quick Start](#quick-start) · [📊 Sample Output](#sample-output) · [📚 Docs](docs/)
</div>

---

## ✨ Key Features

| Feature | Description |
|---|---|
| 🧠 **AI Intent Extraction** | Natural language input → automatic industry & geography detection via Groq LLaMA 3.3 |
| 🌐 **Dual Web Intelligence** | Parallel Tavily searches for market sizing + consumer trends with advanced search depth |
| 🔗 **3-Stage LLM Chain** | Market Fundamentals → Consumer Intelligence → Strategic Synthesis — each stage builds on the last |
| 📧 **Auto-Delivery** | Formatted HTML report delivered to Gmail + webhook response for integrations |
| ⚡ **Sub-60s Execution** | Full pipeline from prompt to report in under a minute using Groq's blazing-fast inference |
| 🛡️ **Production-Grade** | Error handling with `neverError` flags, structured prompt engineering, deterministic outputs |

---

## 🏗️ Architecture

```mermaid
flowchart TD
    subgraph INGESTION ["📥 Ingestion Layer"]
        A["💬 Chat Trigger\n(Webhook/UI)"] --> B["🤖 AI Agent\n(Intent Extraction)"]
        B --> C["⚙️ Code: Parse & Prepare\nContext Variables"]
    end

    subgraph DISCOVERY ["🔍 Web Intelligence"]
        C --> D["🌐 Tavily: Market Sizing\n(Advanced Search)"]
        C --> E["🌐 Tavily: Consumer Data\n(Trend Analysis)"]
        D --> F["🔀 Merge: Context Alignment\n(SQL Cross Join)"]
        E --> F
    end

    subgraph ANALYSIS ["🧠 Analytical Engine"]
        F --> G["📊 Groq: LLaMA 3.3\n(Market Fundamentals)"]
        G --> H["🧠 Groq: LLaMA 3.3\n(Consumer Intelligence)"]
        H --> I["🎯 Groq: LLaMA 3.3\n(Strategic Synthesis)"]
    end

    subgraph DELIVERY ["📤 Report Delivery"]
        I --> J["📋 Code: HTML Assembly\n(Dynamic Formatting)"]
        J --> K["📧 Gmail API\n(Instant Delivery)"]
        J --> L["🔗 Webhook\n(API Integration)"]
    end

    style INGESTION fill:#0d1218,stroke:#4c1d95,color:#fff
    style DISCOVERY fill:#0d1218,stroke:#1e3a5f,color:#fff
    style ANALYSIS fill:#0d1218,stroke:#7c3aed,color:#fff
    style DELIVERY fill:#0d1218,stroke:#059669,color:#fff
    
    style A fill:#1a1a2e,stroke:#4c1d95,color:#fff
    style B fill:#1a1a2e,stroke:#4c1d95,color:#fff
    style C fill:#1a1a2e,stroke:#4c1d95,color:#fff
    style D fill:#1e3a5f,stroke:#4F46E5,color:#fff
    style E fill:#1e3a5f,stroke:#4F46E5,color:#fff
    style F fill:#1e3a5f,stroke:#4F46E5,color:#fff
    style G fill:#2d1b3d,stroke:#7C3AED,color:#fff
    style H fill:#2d1b3d,stroke:#7C3AED,color:#fff
    style I fill:#2d1b3d,stroke:#7C3AED,color:#fff
    style J fill:#1b3d2d,stroke:#059669,color:#fff
    style K fill:#1b3d2d,stroke:#059669,color:#fff
    style L fill:#1b3d2d,stroke:#059669,color:#fff
```

### Pipeline Stages

| Stage | Node | Purpose |
|-------|------|---------|
| **1. Input** | Chat Trigger | Receives natural language query (e.g., *"Electric vehicles in India"*) |
| **2. Extract** | AI Agent + Groq | Extracts `industry` and `geography` from free-text input |
| **3. Search** | 2× Tavily API | Parallel advanced web searches for market data + consumer trends |
| **4. Merge** | SQL Cross Join | Combines both Tavily search results into unified context |
| **5. Analysis** | 3× Groq LLM Calls | Chained analysis: Fundamentals → Consumer Intel → Strategic Synthesis |
| **6. Assembly** | Code Node | Merges all sections into formatted HTML report |
| **7. Delivery** | Gmail + Webhook | Sends report via email and returns via API |

> 📖 For a deep-dive into each node, see **[ARCHITECTURE.md](ARCHITECTURE.md)**

---

## 🚀 Quick Start

### Prerequisites

- [n8n](https://n8n.io) (self-hosted or cloud)
- [Tavily API Key](https://tavily.com) — free tier available
- [Groq API Key](https://console.groq.com) — free tier available
- Gmail account with OAuth2 configured in n8n

### Setup

```bash
# 1. Clone the repository
git clone https://github.com/RishabJainhub/market-research-pipeline.git
cd market-research-pipeline

# 2. Copy environment template
cp .env.example .env

# 3. Add your API keys to .env
# TAVILY_API_KEY=tvly-your-key-here
# GROQ_API_KEY=gsk_your-key-here
```

```bash
# 4. Import the workflow into n8n
#    Open n8n → Workflows → Import from File → select workflow.json

# 5. Update credentials in n8n
#    Replace placeholder API keys in each HTTP Request node

# 6. Activate the workflow and test!
```

> 📖 For detailed step-by-step instructions, see **[docs/setup-guide.md](docs/setup-guide.md)**

---

## 📊 Sample Output

<details>
<summary><b>📄 Electric Vehicles — India (click to expand)</b></summary>

### Section 1 — Market Overview
India's electric vehicle market encompasses battery electric vehicles (BEVs), hybrid electric vehicles (HEVs), and plug-in hybrid electric vehicles (PHEVs) across two-wheeler, three-wheeler, passenger car, and commercial vehicle segments. The market is distinguished by its **dominance of two- and three-wheelers** (which account for over 90% of EV sales) and **strong government policy support** through the FAME II subsidy scheme and state-level incentives.

### Section 2 — Market Size
- **TAM**: $7.1 billion (2025), projected to reach $113.99 billion by 2029 at a CAGR of 66.52%
- **SAM**: $3.2 billion — BEV two-wheelers and three-wheelers *(High confidence)*
- **SOM**: $450 million — urban metro markets with charging infrastructure *(Medium confidence)*

### Section 3 — Competitive Landscape
| Player | Share/Revenue | Primary Advantage | Vulnerability |
|--------|-------------|-------------------|---------------|
| Tata Motors | ~70% passenger EV share | First-mover, Nexon EV brand recognition | Narrow model range |
| Ola Electric | ~35% e-scooter share | Vertical integration, Gigafactory | Quality/service complaints |
| TVS Motor | ~18% e-scooter share | Legacy dealer network | Late EV entry |
| Ather Energy | ~12% e-scooter share | Premium positioning, own charging grid | Limited geographic reach |
| MG Motor (SAIC) | ~8% passenger EV share | Competitive pricing, ZS EV | Import dependency |

*... [7 sections total including Growth Drivers, Barriers & Risks, Consumer Intelligence, and Strategic Outlook]*

</details>

> 📖 Full sample reports available in **[samples/](samples/)**

---

## 🛠️ Tech Stack & Toolkit

<div align="center">
  <table style="width:100%; border-collapse: collapse; border: none;">
    <tr>
      <td width="33%" align="center">
        <img src="https://img.icons8.com/external-flat-icons-inmotus-design/64/external-Workflow-workflow-flat-icons-inmotus-design.png" width="40" height="40"/><br/>
        <h4>Orchestration</h4>
        <p><b>n8n</b><br/>Multi-node workflow automation</p>
      </td>
      <td width="33%" align="center">
        <img src="https://img.icons8.com/external-flaticons-flat-flat-icons/64/external-search-engine-digital-marketing-flaticons-flat-flat-icons.png" width="40" height="40"/><br/>
        <h4>Web Intelligence</h4>
        <p><b>Tavily Search API</b><br/>Real-time market discovery</p>
      </td>
      <td width="33%" align="center">
        <img src="https://img.icons8.com/external-flat-icons-inmotus-design/64/external-Inference-artificial-intelligence-flat-icons-inmotus-design.png" width="40" height="40"/><br/>
        <h4>Inference</h4>
        <p><b>Groq API</b><br/>Ultra-fast LLM execution</p>
      </td>
    </tr>
    <tr>
      <td width="33%" align="center">
        <img src="https://img.icons8.com/external-flat-icons-inmotus-design/64/external-Model-artificial-intelligence-flat-icons-inmotus-design.png" width="40" height="40"/><br/>
        <h4>LLM Backbone</h4>
        <p><b>LLaMA 3.3 70B</b><br/>Strategic market synthesis</p>
      </td>
      <td width="33%" align="center">
        <img src="https://img.icons8.com/external-flat-icons-inmotus-design/64/external-Agent-artificial-intelligence-flat-icons-inmotus-design.png" width="40" height="40"/><br/>
        <h4>Reasoning</h4>
        <p><b>n8n AI Agent</b><br/>NLP intent extraction</p>
      </td>
      <td width="33%" align="center">
        <img src="https://img.icons8.com/external-flat-icons-inmotus-design/64/external-Delivery-artificial-intelligence-flat-icons-inmotus-design.png" width="40" height="40"/><br/>
        <h4>Delivery</h4>
        <p><b>Gmail API</b><br/>Instant HTML reporting</p>
      </td>
    </tr>
  </table>
</div>

---

## 🧠 Prompt Engineering

The pipeline uses a **3-stage progressive prompt chain** where each LLM call builds on the output of the previous one:

```
Stage 1: Market Fundamentals    → Sections 1-3 (Overview, Sizing, Competition)
Stage 2: Consumer Intelligence  → Sections 4-6 (Drivers, Risks, Consumer Shifts)
Stage 3: Strategic Synthesis    → Section 7   (Outlook, Projections, C-Suite Summary)
```

Each prompt is carefully engineered with:
- **Strict section headers** for deterministic parsing
- **Word count limits** (500 / 500 / 300 words) for concise output
- **System role**: *"Senior market research analyst — be precise, never fabricate statistics"*
- **Temperature 0.3** for factual, low-creativity output

> 📖 Full prompt documentation in **[docs/prompt-engineering.md](docs/prompt-engineering.md)**

---

## 📁 Repository Structure

```
market-research-pipeline/
├── README.md                     # You are here
├── ARCHITECTURE.md               # Deep-dive technical documentation
├── LICENSE                       # MIT License
├── .env.example                  # API key template
├── .gitignore                    # Git ignore rules
├── workflow.json                 # n8n workflow (keys redacted)
├── docs/
│   ├── setup-guide.md            # Step-by-step setup instructions
│   ├── api-reference.md          # API endpoint documentation
│   └── prompt-engineering.md     # LLM prompt design rationale
├── samples/
│   ├── electric-vehicles-india.md    # Sample report
│   └── fintech-uk.md                # Sample report
└── evaluation/
    └── evaluation-report.md      # Pipeline evaluation results
```

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Ideas for Contribution
- Add more sample industry reports
- Integrate additional data sources (e.g., Crunchbase, Statista)
- Add a quality-gate module with automated scoring
- Build a web UI frontend for the pipeline

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 👤 Author

**Rishab Jain** — [GitHub](https://github.com/RishabJainhub)

---

<div align="center">

*Built with ❤️ using n8n, Tavily, and Groq*

**⭐ Star this repo if you found it useful!**

</div>
