# 📖 Setup Guide

> Step-by-step instructions to get the Market Research Pipeline running in your n8n instance.

---

## Prerequisites

| Requirement | Details |
|-------------|---------|
| **n8n** | Self-hosted (Docker/npm) or [n8n Cloud](https://n8n.io/cloud) |
| **Tavily API Key** | Sign up at [tavily.com](https://tavily.com) — free tier includes 1,000 searches/month |
| **Groq API Key** | Sign up at [console.groq.com](https://console.groq.com) — free tier includes 14,400 requests/day |
| **Gmail Account** | With OAuth2 credentials configured in n8n |

---

## Step 1: Clone the Repository

```bash
git clone https://github.com/RishabJainhub/market-research-pipeline.git
cd market-research-pipeline
```

---

## Step 2: Get Your API Keys

### Tavily API Key

1. Go to [tavily.com](https://tavily.com) and sign up
2. Navigate to your **Dashboard** → **API Keys**
3. Copy your API key (format: `tvly-...`)

### Groq API Key

1. Go to [console.groq.com](https://console.groq.com) and sign up
2. Navigate to **API Keys** → **Create API Key**
3. Copy your API key (format: `gsk_...`)

---

## Step 3: Import the Workflow into n8n

1. Open your n8n instance
2. Click **Workflows** in the left sidebar
3. Click the **⋮** menu → **Import from File**
4. Select `workflow.json` from this repository
5. The workflow will appear with all 16 nodes connected

---

## Step 4: Configure API Credentials

### Tavily (HTTP Request Nodes)

1. Open node **"2a. Tavily — Market Size & Players"**
2. In **Header Parameters**, replace `YOUR_TAVILY_API_KEY` with your actual key:
   ```
   Authorization: Bearer tvly-your-actual-key
   ```
3. Repeat for node **"2b. Tavily — Trends & Consumer Data"**

### Groq (HTTP Request Nodes)

1. Open node **"5. Groq — Market Fundamentals"**
2. In **Header Parameters**, replace `YOUR_GROQ_API_KEY` with your actual key:
   ```
   Authorization: Bearer gsk_your-actual-key
   ```
3. Repeat for nodes **"6. Groq — Consumer Intelligence"** and **"7. Groq — Strategic Synthesis"**

### Groq (AI Agent LLM)

1. Open the **"Groq Chat Model"** node
2. Click **Create New Credential**
3. Enter your Groq API key
4. Select model: `llama-3.3-70b-versatile`

### Gmail

1. Open the **"Send a message"** node
2. Click **Create New Credential** → **Gmail OAuth2**
3. Follow n8n's OAuth2 flow to connect your Gmail account
4. Update the `sendTo` field with your desired recipient email

---

## Step 5: Test the Pipeline

1. Click **Execute Workflow** (or use the chat widget)
2. Enter a test query:
   ```
   Electric vehicles in India
   ```
3. The workflow should execute all nodes in sequence (~30-60 seconds)
4. Check your email for the formatted market research report

---

## Step 6: Activate for Production

1. Toggle the **Active** switch at the top of the workflow
2. The chat trigger webhook is now live and accepting requests
3. Share the webhook URL or embed the chat widget in your applications

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "No JSON found in agent output" | The AI Agent returned unexpected format — check Groq API key and model availability |
| Tavily returns empty results | Verify API key, check your usage quota at tavily.com |
| Gmail fails to send | Re-authenticate Gmail OAuth2 credential in n8n |
| Groq timeout | Groq's free tier may have rate limits — add a 2-second delay between LLM calls |
| Report has missing sections | Check that `neverError: true` isn't masking a failed API call — inspect node outputs |

---

## Optional: Environment Variables

If you're running n8n self-hosted, you can use environment variables instead of hardcoding keys:

```bash
# In your .env or docker-compose environment
N8N_ENCRYPTION_KEY=your-encryption-key
TAVILY_API_KEY=tvly-your-key
GROQ_API_KEY=gsk_your-key
```

Then reference them in n8n nodes using `{{ $env.TAVILY_API_KEY }}`.
