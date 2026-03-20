# 🧠 Prompt Engineering

> Documentation of the 3-stage GPT-4o synthesis chain.

---

## Design Philosophy

The pipeline uses **Context Inheritance** to build long-form reports (3000+ words) without token fragmentation or repetitive content.

### Stage 1: Market Fundamentals
- **Context**: Perplexity Sonar search results.
- **Goal**: Establish TAM/SAM/SOM and competitive mapping.
- **Model**: `gpt-4o` (Temp: 0.3)

### Stage 2: Consumer Intelligence
- **Context**: Stage 1 Output + Perplexity data.
- **Goal**: Identify behavior shifts and data-driven growth drivers.
- **Model**: `gpt-4o` (Temp: 0.4)

### Stage 3: Strategic Synthesis
- **Context**: Full report context from Stages 1 & 2.
- **Goal**: 12-month outlook and board-ready C-suite summary.
- **Model**: `gpt-4o` (Temp: 0.5)

---

## Anti-Hallucination Guardrails

1. **Grounded In Search**: Perplexity data is fed as the *only* source of truth in the system prompt.
2. **Deterministic Formatting**: Use of exact section headers prevents LLM drift.
3. **Quality Gate**: A logic-based check at the end of the workflow flags any report under 700 words for human verification.
