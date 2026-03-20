# 🔌 API Reference

> Documentation for all external APIs used in the Market Research Pipeline.

---

## Perplexity Sonar API

The pipeline uses Perplexity's `sonar` model as the primary ground-truth search layer.

### Endpoint

```
POST https://api.perplexity.ai/chat/completions
```

### Headers

| Header | Value |
|--------|-------|
| `Content-Type` | `application/json` |
| `Authorization` | `Bearer YOUR_PERPLEXITY_API_KEY` |

### Key Parameters

| Parameter | Value |
|-----------|-------|
| `model` | `sonar` |
| `max_tokens` | `1024` |
| `search_recency_filter` | `month` |
| `return_citations` | `true` |

---

## OpenAI GPT-4o API

The pipeline makes 3 sequential calls to GPT-4o for high-fidelity report synthesis.

### Endpoint

```
POST https://api.openai.com/v1/chat/completions
```

### Configuration Rationale

| Parameter | Value | Rationale |
|-----------|-------|-----------|
| `model` | `gpt-4o` | State-of-the-art reasoning and instruction following. |
| `temperature` | `0.3 - 0.5` | Balanced for factual synthesis and natural report flow. |
| `max_tokens` | `900 / 700 / 500` | Decreasing limits to force synthesis toward the final "Executive Summary". |

---

## Google Sheets & Docs API

### Google Sheets Trigger
- **Event**: `rowAdded`
- **Polling**: Every 1 minute

### Google Docs Export
- **Operation**: `create`
- **Content**: Assembled HTML/Markdown from the GPT-4o chain.

---

## Airtable API

### Configuration
- **Base ID**: Required (configured in node)
- **Table**: `Market Research Runs`
- **Columns**: `Industry`, `Date Run`, `Status`, `Report Link`, `Word Count`, `Quality Pass`.
