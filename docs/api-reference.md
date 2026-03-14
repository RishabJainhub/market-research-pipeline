# 🔌 API Reference

> Documentation for all external APIs used in the Market Research Pipeline.

---

## Tavily Search API

The pipeline makes **2 parallel calls** to Tavily for web intelligence.

### Endpoint

```
POST https://api.tavily.com/search
```

### Headers

| Header | Value |
|--------|-------|
| `Content-Type` | `application/json` |
| `Authorization` | `Bearer YOUR_TAVILY_API_KEY` |

### Request Body

| Parameter | Type | Value | Description |
|-----------|------|-------|-------------|
| `query` | string | Dynamic | Search query built from industry + geography |
| `search_depth` | string | `"advanced"` | Uses Tavily's advanced search for higher quality results |
| `max_results` | integer | `5` | Number of search results to return |
| `include_answer` | string | `"advanced"` | Returns an AI-synthesized answer alongside raw results |
| `topic` | string | `"general"` | Search topic category |

### Search Queries Used

| Call | Query Template | Example |
|------|---------------|---------|
| Market Size & Players | `"{industry} market size players {geography} 2025"` | `"Electric vehicles market size players India 2025"` |
| Trends & Consumer Data | `"{industry} consumer trends growth drivers challenges {geography} 2025"` | `"Electric vehicles consumer trends growth drivers challenges India 2025"` |

### Response (Key Fields)

```json
{
  "answer": "AI-synthesized summary of search results...",
  "results": [
    {
      "title": "Article Title",
      "url": "https://example.com/article",
      "content": "Relevant excerpt...",
      "score": 0.95
    }
  ]
}
```

### Rate Limits

| Tier | Searches/Month | Cost |
|------|---------------|------|
| Free | 1,000 | $0 |
| Pro | 10,000 | $50/mo |
| Enterprise | Custom | Custom |

> 📖 Full docs: [docs.tavily.com](https://docs.tavily.com)

---

## Groq Chat Completions API

The pipeline makes **4 calls** to Groq (1 for intent extraction via AI Agent, 3 for analysis).

### Endpoint

```
POST https://api.groq.com/openai/v1/chat/completions
```

### Headers

| Header | Value |
|--------|-------|
| `Content-Type` | `application/json` |
| `Authorization` | `Bearer YOUR_GROQ_API_KEY` |

### Request Body

```json
{
  "model": "llama-3.3-70b-versatile",
  "temperature": 0.3,
  "max_tokens": 800,
  "messages": [
    {
      "role": "system",
      "content": "You are a senior market research analyst. Be precise. Never fabricate statistics."
    },
    {
      "role": "user",
      "content": "Dynamic prompt content..."
    }
  ]
}
```

### Configuration Rationale

| Parameter | Value | Why |
|-----------|-------|-----|
| `model` | `llama-3.3-70b-versatile` | Best quality/speed ratio on Groq; 70B parameter model handles complex market analysis |
| `temperature` | `0.3` | Low creativity — prioritizes factual accuracy over creative writing |
| `max_tokens` | `800` | Enforces concise output; prevents verbose, unfocused responses |

### Response (Key Fields)

```json
{
  "choices": [
    {
      "message": {
        "role": "assistant",
        "content": "## Section 1 — Market Overview\n..."
      },
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 450,
    "completion_tokens": 680,
    "total_tokens": 1130
  }
}
```

### Rate Limits (Free Tier)

| Metric | Limit |
|--------|-------|
| Requests/minute | 30 |
| Requests/day | 14,400 |
| Tokens/minute | 6,000 |

> 📖 Full docs: [console.groq.com/docs](https://console.groq.com/docs)

---

## Gmail API (via n8n)

### Configuration

| Property | Value |
|----------|-------|
| Node | `n8n-nodes-base.gmail` v2.2 |
| Auth | OAuth2 |
| Send To | Configurable email address |
| Subject | Dynamic: `"{industry} Market Research — {Geography}"` |
| Body | HTML formatted report |

### OAuth2 Setup

1. Create a Google Cloud project at [console.cloud.google.com](https://console.cloud.google.com)
2. Enable the **Gmail API**
3. Create **OAuth2 credentials** (Web application type)
4. Add n8n's redirect URL: `https://your-n8n-instance/rest/oauth2-credential/callback`
5. Configure the credential in n8n with your Client ID and Client Secret
