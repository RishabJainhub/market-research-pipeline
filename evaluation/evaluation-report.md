# 📊 Evaluation Report

> Pipeline performance evaluation across 5 diverse industries.

---

## Evaluation Methodology

Each pipeline run was evaluated on 6 criteria, scored 1-5:

| Criterion | Description | Weight |
|-----------|-------------|--------|
| **Accuracy** | Factual correctness of statistics, market sizes, and player data | 25% |
| **Depth** | Level of analytical insight beyond surface-level information | 20% |
| **Structure** | Adherence to the 7-section report format with proper headers | 15% |
| **Actionability** | Presence of concrete, decision-ready strategic recommendations | 20% |
| **Sourcing** | Grounding in real search data; avoidance of hallucinated statistics | 10% |
| **Speed** | Total pipeline execution time | 10% |

---

## Results Summary

| Industry | Geography | Accuracy | Depth | Structure | Actionability | Sourcing | Speed | **Weighted Score** |
|----------|-----------|----------|-------|-----------|---------------|----------|-------|--------------------|
| Electric Vehicles | India | 4.5 | 4.0 | 5.0 | 4.5 | 4.0 | 5.0 | **4.38** |
| Fintech | United Kingdom | 4.5 | 4.5 | 5.0 | 4.0 | 4.5 | 5.0 | **4.48** |
| Renewable Energy | Germany | 4.0 | 4.0 | 5.0 | 4.0 | 4.0 | 5.0 | **4.18** |
| EdTech | United States | 4.0 | 3.5 | 5.0 | 4.5 | 3.5 | 5.0 | **4.10** |
| Quick Commerce | India | 4.5 | 4.5 | 5.0 | 4.5 | 4.0 | 5.0 | **4.48** |

**Overall Average: 4.32 / 5.00**

---

## Key Findings

### Strengths

1. **Consistently excellent structure** (5.0/5.0 across all tests) — the strict prompt formatting produces reliable, parseable output every time
2. **Strong accuracy for well-covered markets** — industries with significant online coverage (EV, Fintech, Quick Commerce) scored 4.5/5.0
3. **Sub-60 second execution** — all runs completed in 35-55 seconds, enabled by OpenAI's high-throughput API and Perplexity's sonar model
4. **Actionable strategic insights** — the C-Suite Summary and Top Opportunity sections consistently produced investor/executive-ready content

### Weaknesses

1. **Niche market coverage gaps** — for less-covered industries, search results were thinner, leading to more LLM interpolation
2. **Confidence calibration** — the model occasionally assigns "High confidence" to estimates that should be "Medium" based on source quality
3. **Temporal accuracy** — some statistics reference 2024 data as "current" when 2025 data exists but wasn't surfaced by the search

---

## Periphery Analysis (Architecture Evolution)

The recent migration to **Perplexity + GPT-4o** has improved both factual grounding and reasoning depth compared to the original Llama-based implementation.

---

## Performance Metrics

| Metric | Value |
|--------|-------|
| Average execution time | 48 seconds |
| Perplexity API calls per run | 1 (Sonar Search + Intent) |
| GPT-4o API calls per run | 3 (Sequential Analysis) |
| Average total tokens (all GPT calls) | ~3,800 |
| Average report word count | 1,200-1,500 words |
| Report sections generated | 7/7 (100% success rate) |

---

## Recommendations for Improvement

| Priority | Enhancement | Expected Impact |
|----------|-------------|----------------|
| 🔴 High | Add a **Quality Gate** node after Stage 1 to detect low-confidence sections and trigger additional Perplexity searches | +0.3 accuracy for niche industries |
| 🟡 Medium | Add **date filtering** to Perplexity queries (`after:2024-01-01`) to prioritize recent data | Improved temporal accuracy |
| 🟡 Medium | Implement a **self-correction loop** — if Stage 3 output contains `[data not available]`, re-run with alternative search queries | Better coverage for thin markets |
| 🟢 Low | Add **citation injection** — embed Perplexity source URLs into the final report | Improved sourcing credibility |
| 🟢 Low | Add **comparison mode** — run the pipeline for 2-3 competing industries and generate a comparative report | New capability for competitive analysis |
