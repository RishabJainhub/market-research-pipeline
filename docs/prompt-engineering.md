# 🧠 Prompt Engineering

> Documentation of the LLM prompt design strategy used across the 3-stage analysis chain.

---

## Design Philosophy

The pipeline's prompts follow four core principles:

1. **Progressive Context Building** — Each stage receives the output of the previous stage, creating a compounding knowledge base
2. **Structural Determinism** — Strict section headers and format instructions ensure consistent, parseable output
3. **Scope Constraints** — Word limits and specific sub-section requirements prevent verbose, unfocused responses
4. **Anti-Hallucination Guardrails** — The system role explicitly prohibits fabricating statistics

---

## System Prompt (Applied to All LLM Calls)

```
You are a senior market research analyst. Be precise. Never fabricate statistics.
```

**Why this works:**
- **"Senior market research analyst"** — establishes domain expertise, producing industry-standard terminology and frameworks
- **"Be precise"** — reduces filler content and hedging language
- **"Never fabricate statistics"** — critical guardrail; when the model doesn't know a number, it will say so instead of inventing one

---

## Stage 1 — Market Fundamentals

### Input Context
- Tavily "Market Size & Players" search results (AI-synthesized answer)
- Tavily "Trends & Consumer Data" search results (AI-synthesized answer)

### Prompt

```
Using the following market research data:

MARKET SIZE & PLAYERS:
{tavily_answer_1}

TRENDS & CONSUMER DATA:
{tavily_answer_2}

Produce Sections 1-3 for {industry} in {geography}:

## Section 1 — Market Overview (150 words)
Define market scope, key segments, geographic coverage, 2 distinguishing characteristics.

## Section 2 — Market Size (150 words)
TAM, SAM, SOM estimates with confidence levels.

## Section 3 — Competitive Landscape (200 words)
Top 5 players. For each: revenue/share, primary advantage, one vulnerability.

500 words max. Use exact headers. Bullets.
```

### Design Rationale

| Element | Purpose |
|---------|---------|
| `Using the following market research data:` | Grounds the LLM in real search data instead of parametric knowledge |
| `Produce Sections 1-3` | Explicit section numbering ensures deterministic structure |
| `(150 words)` per section | Individual word budgets prevent any single section from dominating |
| `TAM, SAM, SOM estimates with confidence levels` | Forces standard market sizing framework + epistemic calibration |
| `For each: revenue/share, primary advantage, one vulnerability` | Structured per-player template ensures consistent competitive analysis |
| `Use exact headers. Bullets.` | Ensures parseable output for the Markdown→HTML converter |

---

## Stage 2 — Consumer Intelligence

### Input Context
- Stage 1 output (Market Fundamentals)

### Prompt

```
Using this market context:
{stage_1_output}

Now produce Sections 4-6 for {industry} in {geography}:

## Section 4 — Growth Drivers
3 growth factors. For each: name, mechanism, data point.

## Section 5 — Barriers & Risks
3 challenges. For each: name, impact, mitigation.

## Section 6 — Consumer Intelligence
3 behavior shifts, one pain point, one emerging expectation.

500 words max. Use exact headers.
```

### Design Rationale

| Element | Purpose |
|---------|---------|
| `Using this market context:` | Feeds Stage 1 output as context — the model builds on its own prior analysis |
| `3 growth factors / 3 challenges` | Fixed count prevents both undercoverage and list bloat |
| `name, mechanism, data point` | Structured triplet format for each growth driver |
| `name, impact, mitigation` | Forces actionable risk analysis rather than just listing problems |
| `one pain point, one emerging expectation` | Targets specific consumer insights beyond broad trends |

---

## Stage 3 — Strategic Synthesis

### Input Context
- Stage 1 output (Market Fundamentals)
- Stage 2 output (Consumer Intelligence)

### Prompt

```
You are completing a market research report for {industry} in {geography}.

MARKET FUNDAMENTALS:
{stage_1_output}

CONSUMER INTELLIGENCE:
{stage_2_output}

Produce Section 7 — Strategic Outlook:

**12-Month Outlook (75 words):** What will change or consolidate? Name one specific inflection point.
**3-Year Projection (75 words):** Market size or CAGR estimate for 2028. State confidence level.
**Top Opportunity (50 words):** Single highest-conviction opportunity. Gap, mechanism, prize size.
**Top Risk (50 words):** One material disruption risk. Quantify impact. Early warning signal.
**C-Suite Summary (1 sentence):** One sentence a CEO can repeat in a board meeting.

300 words maximum.
```

### Design Rationale

| Element | Purpose |
|---------|---------|
| `MARKET FUNDAMENTALS: + CONSUMER INTELLIGENCE:` | Full context from both prior stages enables true synthesis |
| `12-Month Outlook / 3-Year Projection` | Dual timeframe forces both tactical and strategic thinking |
| `Name one specific inflection point` | Prevents vague predictions — demands concrete named events |
| `State confidence level` | Epistemic calibration — the model must self-assess certainty |
| `Single highest-conviction opportunity` | Forces prioritization over listing many options |
| `Quantify impact. Early warning signal.` | Makes risk analysis actionable rather than theoretical |
| `One sentence a CEO can repeat in a board meeting` | The ultimate test of synthesis quality — forces extreme distillation |
| `300 words maximum` | Tightest word limit for the final stage — synthesis demands brevity |

---

## Prompt Evolution Notes

### What Was Tried and Abandoned

| Approach | Issue | Fix |
|----------|-------|-----|
| Single mega-prompt | Output quality degraded after ~600 tokens; sections became superficial | Split into 3 staged calls |
| `temperature: 0.7` | Too much creative interpolation; invented market statistics | Reduced to `0.3` |
| No word limits | Stage 1 would use 1200+ words, leaving no room for later stages | Added per-section word budgets |
| `max_tokens: 2000` | LLM would pad output with caveats and disclaimers | Reduced to `800` |
| Generic system prompt | Output read like a textbook instead of a market report | Added "senior analyst" role |

---

## Extending the Prompts

To add new sections or modify existing ones:

1. **Add a new stage**: Create a new Prepare Node + Code Node + HTTP Request chain
2. **Feed prior context**: Reference previous node outputs using `$('Node Name').item.json.choices[0].message.content`
3. **Maintain structure**: Use `## Section N — Title` headers for consistent parsing
4. **Set word limits**: Include per-section word budgets to prevent bloat
5. **Test with diverse industries**: Verify the prompt generalizes across sectors
