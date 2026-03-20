# 📖 Setup Guide

> Step-by-step instructions to get the Market Research Pipeline running in your n8n instance.

---

## Prerequisites

| Requirement | Details |
|-------------|---------|
| **n8n** | Self-hosted (Docker/npm) or [n8n Cloud](https://n8n.io/cloud) |
| **Perplexity API Key** | Sign up at [perplexity.ai](https://perplexity.ai) — Uses the `sonar` model for real-time web search |
| **OpenAI API Key** | Sign up at [platform.openai.com](https://platform.openai.com) — Uses `gpt-4o` for report synthesis |
| **Google Workspace** | Credentials for Google Sheets and Google Docs |
| **Airtable Token** | For logging and persistence |

---

## Step 1: Clone the Repository

```bash
git clone https://github.com/RishabJainhub/market-research-pipeline.git
cd market-research-pipeline
```

---

## Step 2: Get Your API Keys

### Perplexity API Key

1. Go to [perplexity.ai/settings/api](https://www.perplexity.ai/settings/api)
2. Generate a new API key (format: `pplx-...`)

### OpenAI API Key

1. Go to [platform.openai.com](https://platform.openai.com)
2. Create am API key (format: `sk-...`)

---

## Step 3: Import the Workflow into n8n

1. Open your n8n instance
2. Click **Workflows** → **Import from File**
3. Select `workflow.json` from this repository
4. The workflow will appear with the **Google Sheets Trigger** and **Perplexity** nodes.

---

## Step 4: Configure API Credentials

### Perplexity (HTTP Request Node)

1. Open node **"2. Perplexity — Live Web Search"**
2. In **Header Parameters**, replace `YOUR_PERPLEXITY_API_KEY` with your actual key.

### OpenAI (GPT-4o Nodes)

1. Open any of the **"GPT Call"** nodes.
2. Click **Create New Credential** → **OpenAI API**.
3. Enter your OpenAI key.
4. Set model to `gpt-4o`.

### Google Sheets & Docs

1. Open the **"Google Sheets Trigger"** and **"Create Google Doc"** nodes.
2. Create a **Google OAuth2** credential following n8n's standard documentation.

---

## Step 5: Activate for Production

1. Update the `documentId` in the Sheets and Docs nodes to point to your specific files.
2. Toggle the **Active** switch.
3. Add a new row to your Google Sheet to trigger the pipeline.
