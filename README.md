# Automated Market Research AI Pipeline

<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1218,50:4c1d95,100:8b5cf6&height=220&section=header&text=AI%20Market%20Research%20Pipeline&fontSize=38&fontColor=ffffff&animation=twinkling&fontAlignY=35&desc=n8n%20Automation%20%7C%20Llama%203.1%20Synthesis%20%7C%20Perplexity%20API&descSize=16&descAlignY=55&descColor=cccccc" width="100%" />
</div>

---

### 🚀 Overview
A production-grade AI automation pipeline built with **n8n**, **Groq LLaMA 3.3 70B**, and **Perplexity Sonar API**. This system transforms a simple Google Sheets trigger into a structured, multi-stage market intelligence report in under 60 seconds.

### ✨ Key Results
- ⏱️ **Speed**: <60 seconds for a full 7-section report.
- 📉 **Efficiency**: 90% reduction in manual research effort.
- 👥 **Adoption**: Deployed and used by 8+ active users for real-world strategy synthesis.
- 📊 **Scalability**: Capable of processing dozens of concurrent research tasks via n8n's asynchronous workflow architecture.

---

### 🏗️ Architecture

```mermaid
graph LR
    A[Google Sheets Trigger] --> B[n8n Workflow Engine]
    B --> C[Perplexity Search API]
    C --> D[Groq LLaMA 3.3 70B Synthesis]
    D --> E[Quality Gate / Self-Correction]
    E --> F[Google Docs / Airtable Export]
```

---

### 🛠️ Technology Stack
- **Orchestration**: n8n (Low-code Automation)
- **Large Language Models**: Groq LLaMA 3.3 70B, Llama-3.1
- **Real-time Search**: Perplexity Sonar API
- **Data Delivery**: Google Docs, Airtable, Google Sheets

---

### 📊 Features
- **Multi-Stage Synthesis**: Goes beyond simple summarization to provide fundamental analysis, consumer intelligence, and executive synthesis.
- **Auto-Formatting**: Deliver reports directly to Google Docs with professional heading structures.
- **Logging & Persistence**: Every research run is logged into Airtable for historical tracking and audit.
- **Self-Correction**: Integrated If/Else quality gates to ensure high-fidelity outputs.

---

### 📋 Sample Report Structure

1. **Executive Summary**
2. **Market Fundamentals**
3. **Consumer Intelligence**
4. **Competitive Landscape**
5. **Technological Trends**
6. **Risk Assessment**
7. **Actionable Recommendations**

---

### 💻 Local Setup
1. **Import Workflow**: Import the provided `.json` workflow into your n8n instance.
2. **Set Credentials**: Link your Groq, Perplexity, and Google Workspace APIs.
3. **Trigger**: Add a new row to your Google Sheet to initiate the pipeline.

---

<div align="center">
  <img src="https://img.shields.io/github/license/RishabJainhub/market-research-pipeline?style=for-the-badge" />
  <img src="https://img.shields.io/github/stars/RishabJainhub/market-research-pipeline?style=for-the-badge" />
</div>
