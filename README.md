# Automated Market Research AI Pipeline

<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1218,50:4c1d95,100:8b5cf6&height=220&section=header&text=AI%20Market%20Research%20Pipeline&fontSize=38&fontColor=ffffff&animation=twinkling&fontAlignY=35&desc=n8n%20Automation%20%7C%20Llama%203.1%20Synthesis%20%7C%20Perplexity%20API&descSize=16&descAlignY=55&descColor=cccccc" width="100%" />

  **Enterprise-Grade AI Automation for Market Intelligence powered by n8n, Llama-3.1, and Perplexity Search API.**

  <p align="center">
    <a href="https://github.com/RishabJainhub/market-research-pipeline"><img src="https://img.shields.io/badge/n8n-FF6D5A?style=for-the-badge&logo=n8n&logoColor=white" alt="n8n"></a>
    <a href="https://github.com/RishabJainhub/market-research-pipeline"><img src="https://img.shields.io/badge/Groq-F55036?style=for-the-badge&logo=groq&logoColor=white" alt="Groq"></a>
    <a href="https://github.com/RishabJainhub/market-research-pipeline"><img src="https://img.shields.io/badge/Llama--3.1-4285F4?style=for-the-badge&logo=meta&logoColor=white" alt="Llama 3.1"></a>
    <a href="https://github.com/RishabJainhub/market-research-pipeline"><img src="https://img.shields.io/badge/Perplexity-00A67E?style=for-the-badge&logo=perplexity&logoColor=white" alt="Perplexity"></a>
    <a href="https://github.com/RishabJainhub/market-research-pipeline"><img src="https://img.shields.io/badge/Airtable-18BFFF?style=for-the-badge&logo=airtable&logoColor=white" alt="Airtable"></a>
  </p>
</div>

---

### 🚀 Overview
A production-grade AI automation pipeline built with **n8n**, **OpenAI GPT-4o**, and **Perplexity Sonar API**. This system transforms a simple Google Sheets trigger into a structured, multi-stage market intelligence report in under 60 seconds.

### ✨ Key Results
- ⏱️ **Speed**: <60 seconds for a full 7-section report.
- 📉 **Efficiency**: 90% reduction in manual research effort.
- 🧪 **Scalability**: Integrated with Google Sheets and Airtable for zero-touch batch processing.
- 🕵️ **Real-time Accuracy**: Powered by Perplexity Sonar for zero-hallucination web intelligence.

---

### 🏗️ Architecture

```mermaid
graph LR
    A[Google Sheets Trigger] --> B[n8n Workflow Engine]
    B --> C[Perplexity Sonar API]
    C --> D[GPT-4o Sequential Synthesis]
    D --> E[Quality Gate / Self-Correction]
    E --> F[Google Docs / Airtable Export]
```

---

### 🛠️ Technology Stack
- **Orchestration**: n8n (Production Automation)
- **Large Language Model**: OpenAI GPT-4o
- **Real-time Search**: Perplexity Sonar API
- **Data Persistence**: Google Docs, Airtable, Google Sheets

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
