<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1218,50:4c1d95,100:8b5cf6&height=220&section=header&text=Market%20Intelligence%20Architecture&fontSize=38&fontColor=ffffff&animation=twinkling&fontAlignY=35&desc=AI%20Agent%20%7C%20n8n%20Orchestration%20%7C%20Sequential%20LLM%20Chains&descSize=16&descAlignY=55&descColor=cccccc" width="100%" />

  **Technical Deep-Dive into the n8n Market Research Pipeline powered by AI Agents & Groq.**
</div>

---

## Table of Contents

- [System Overview](#system-overview)
- [Data Flow](#data-flow)
- [Node-by-Node Breakdown](#node-by-node-breakdown)
- [Prompt Chaining Strategy](#prompt-chaining-strategy)
- [Error Handling](#error-handling)
- [Design Decisions](#design-decisions)

---

## System Overview

The pipeline is a **16-node n8n workflow** that transforms a natural language query (e.g., *"Electric vehicles in India"*) into a structured 7-section market research report. It combines **real-time web intelligence** (Tavily) with **LLM analytical reasoning** (Groq + LLaMA 3.3 70B) through a sequential, context-building architecture.

```mermaid
graph TD
    subgraph INPUT ["🎯 Input Layer"]
        A[Chat Trigger] --> B[AI Agent]
        C[Groq Chat Model] -.->|LLM Provider| B
        B --> D[JSON Parser]
        D --> E[Field Preparation]
    end

    subgraph SEARCH ["🔍 Search Layer"]
        E --> F[Tavily: Market Size]
        E --> G[Tavily: Consumer Trends]
        F --> H[SQL Merge]
        G --> H
    end

    subgraph ANALYSIS ["🧠 Analysis Layer"]
        H --> I[Context Variables]
        I --> J[Prompt Builder 1]
        J --> K[Groq: Market Fundamentals]
        K --> L[Prompt Builder 2]
        L --> M[Groq: Consumer Intelligence]
        M --> N[Prompt Builder 3]
        N --> O[Groq: Strategic Synthesis]
    end

    subgraph OUTPUT ["📤 Output Layer"]
        O --> P[Report Assembly]
        P --> Q[Gmail Delivery]
        P --> R[Webhook Response]
    end

    style INPUT fill:#1a1a2e,stroke:#e63946,color:#fff
    style SEARCH fill:#1e3a5f,stroke:#4F46E5,color:#fff
    style ANALYSIS fill:#2d1b3d,stroke:#7C3AED,color:#fff
    style OUTPUT fill:#1b3d2d,stroke:#059669,color:#fff
```

---

## Data Flow

```
User Input: "Electric vehicles in India"
         │
         ▼
┌─────────────────────────────────┐
│  AI Agent (Groq LLaMA 3.3)     │
│  Output: {"industry": "Electric │
│  vehicles", "geography":"India"} │
└─────────────────────────────────┘
         │
         ├──────────────────────────────┐
         ▼                              ▼
┌─────────────────────┐  ┌──────────────────────────┐
│ Tavily Search #1    │  │ Tavily Search #2         │
│ "EV market size     │  │ "EV consumer trends      │
│  revenue players    │  │  behavior India 2025"    │
│  India 2025"        │  │                          │
│ → 5 results + AI    │  │ → 5 results + AI         │
│   summary           │  │   summary                │
└─────────────────────┘  └──────────────────────────┘
         │                              │
         └──────────┬───────────────────┘
                    ▼
         ┌─────────────────────┐
         │  SQL CROSS JOIN     │
         │  Merged context     │
         └─────────────────────┘
                    │
                    ▼
         ┌─────────────────────┐
         │  Groq Call #1       │   Sections 1-3
         │  Market Fundamentals│   Overview, Size, Competition
         └─────────────────────┘
                    │
                    ▼
         ┌─────────────────────┐
         │  Groq Call #2       │   Sections 4-6
         │  Consumer Intel     │   Drivers, Risks, Behavior
         └─────────────────────┘
                    │
                    ▼
         ┌─────────────────────┐
         │  Groq Call #3       │   Section 7
         │  Strategic Synthesis│   Outlook, Projections, C-Suite
         └─────────────────────┘
                    │
                    ▼
         ┌─────────────────────┐
         │  HTML Assembly      │
         │  Full Report        │──→  📧 Gmail  +  🔗 Webhook
         └─────────────────────┘
```

---

## Node-by-Node Breakdown

### 1. Chat Trigger (`When chat message received`)

| Property | Value |
|----------|-------|
| Type | `@n8n/n8n-nodes-langchain.chatTrigger` |
| Version | 1.4 |
| Public | `true` |

The entry point of the pipeline. Accepts a natural language message via n8n's built-in chat widget or webhook endpoint.

---

### 2. AI Agent (`AI Agent`)

| Property | Value |
|----------|-------|
| Type | `@n8n/n8n-nodes-langchain.agent` |
| LLM | Groq LLaMA 3.3 70B |
| System Prompt | *"Extract industry and geography from the user message. Respond with ONLY this JSON, nothing else: {"industry": "...", "geography": "..."}"* |

Uses a constrained system prompt to extract exactly two fields from free-text. The strict JSON-only instruction ensures reliable parsing downstream.

---

### 3. JSON Parser (`Code in JavaScript`)

```javascript
const raw = $input.first().json.output;
const match = raw.match(/\{[\s\S]*\}/);
if (!match) throw new Error('No JSON found in agent output: ' + raw);
const parsed = JSON.parse(match[0]);
return [{ json: { industry: parsed.industry.trim(), geography: parsed.geography.trim() } }];
```

Extracts JSON from the AI agent's response using regex. Handles cases where the LLM wraps JSON in additional text.

---

### 4. Field Preparation (`Edit Fields`)

Structures the extracted data and pre-builds Tavily search queries:
- `tavily_query_1`: `"{industry} market size revenue players {geography} 2025"`
- `tavily_query_2`: `"{industry} consumer trends behavior {geography} 2024 2025"`

---

### 5a. Tavily — Market Size & Players

| Property | Value |
|----------|-------|
| Endpoint | `POST https://api.tavily.com/search` |
| Search Depth | `advanced` |
| Max Results | `5` |
| Include Answer | `advanced` |
| Topic | `general` |

Retrieves market sizing data, key players, and revenue figures.

---

### 5b. Tavily — Trends & Consumer Data

Same configuration as 5a but with a trend-focused query. Both searches run **in parallel** for speed.

---

### 6. Merge Results (`SQL CROSS JOIN`)

Combines both Tavily results using an SQL cross join, creating a unified context object. This ensures both data streams are available to all downstream nodes.

---

### 7–9. Three-Stage Groq LLM Chain

See [Prompt Chaining Strategy](#prompt-chaining-strategy) below.

---

### 10. Report Assembly (`Assembly Full Report`)

A JavaScript Code node that:
1. Collects output from all three Groq calls
2. Converts Markdown to HTML using regex-based transformation
3. Wraps the report in styled HTML with branded headers
4. Outputs `full_report_html`, `industry`, and `Geography`

---

### 11. Gmail Delivery + Webhook Response

The assembled report is sent to **two parallel outputs**:
- **Gmail**: HTML email with subject line `"{industry} Market Research — {Geography}"`
- **Webhook**: Full JSON response for API integrations

---

## Prompt Chaining Strategy

The pipeline uses **progressive context accumulation** — each LLM call receives the output of the previous call as additional context:

```mermaid
flowchart LR
    subgraph Stage1 ["Stage 1: Market Fundamentals"]
        direction TB
        I1["📥 Input: Tavily search results"] --> P1["📝 Prompt: Produce Sections 1-3"]
        P1 --> O1["📤 Output: Overview + Size + Competition"]
    end

    subgraph Stage2 ["Stage 2: Consumer Intelligence"]
        direction TB
        I2["📥 Input: Stage 1 output"] --> P2["📝 Prompt: Produce Sections 4-6"]
        P2 --> O2["📤 Output: Drivers + Risks + Behavior"]
    end

    subgraph Stage3 ["Stage 3: Strategic Synthesis"]
        direction TB
        I3["📥 Input: Stage 1 + Stage 2 output"] --> P3["📝 Prompt: Produce Section 7"]
        P3 --> O3["📤 Output: Outlook + Projections"]
    end

    Stage1 --> Stage2 --> Stage3

    style Stage1 fill:#F55036,stroke:#DC2626,color:#fff
    style Stage2 fill:#7C3AED,stroke:#5B21B6,color:#fff
    style Stage3 fill:#059669,stroke:#047857,color:#fff
```

### Why Chain Instead of Single Call?

| Approach | Pros | Cons |
|----------|------|------|
| **Single prompt** | Simpler | Token overflow, inconsistent structure, weak synthesis |
| **3-stage chain** ✅ | Better quality, progressive refinement, reliable structure | 3× API calls (mitigated by Groq's speed) |

### LLM Configuration

| Parameter | Value | Rationale |
|-----------|-------|-----------|
| Model | `llama-3.3-70b-versatile` | Best balance of quality and speed on Groq |
| Temperature | `0.3` | Low creativity — prioritizes factual accuracy |
| Max Tokens | `800` | Prevents verbose output, forces conciseness |
| System Role | Senior market research analyst | Establishes domain expertise and anti-hallucination guardrails |

---

## Error Handling

| Mechanism | Implementation |
|-----------|---------------|
| **API Resilience** | All HTTP Request nodes use `neverError: true` — the workflow continues even if an API call fails |
| **JSON Parsing Safety** | Regex-based JSON extraction with explicit error throws and context messages |
| **Prompt Guardrails** | Strict section headers, word limits, and format instructions prevent unparseable LLM output |
| **Idempotency** | Each run is stateless — failed runs can be safely retried |

---

## Design Decisions

### 1. Tavily over Perplexity/SerpAPI

- **Tavily's `include_answer: advanced`** provides an AI-synthesized summary alongside raw results — reducing the need for an extra LLM call to summarize search results
- Native JSON response format eliminates scraping/parsing overhead

### 2. Groq over OpenAI/Anthropic

- **Inference speed**: Groq processes LLaMA 3.3 70B at ~500 tokens/second — enabling sub-60s total pipeline execution
- **Cost**: Groq's free tier is more generous for prototyping
- **Quality**: LLaMA 3.3 70B's analytical capabilities match GPT-4-class performance for structured market analysis

### 3. SQL CROSS JOIN for Merging

Using n8n's built-in SQL merge (rather than a Code node) ensures:
- Declarative data combination
- Consistent behavior regardless of result ordering
- Easy extensibility if more data sources are added

### 4. Markdown-to-HTML in Assembly

The final report uses a lightweight regex-based Markdown→HTML converter rather than a library:
- Zero dependencies
- Handles the specific subset of Markdown the LLM produces
- Produces clean, email-compatible HTML
