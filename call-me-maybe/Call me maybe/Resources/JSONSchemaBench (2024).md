← [[00 Reading Guide]]

# Generating Structured Outputs from LLMs: Benchmark and Studies
**Geng et al. (2024)** — EPFL, Microsoft · [arxiv.org/html/2501.10868v1](https://arxiv.org/html/2501.10868v1)

---

## What it is

10,000 real-world JSON schemas across 10 domains (function calls, service APIs, Kubernetes configs, analytics events, news publishing). Benchmarks constrained decoding frameworks on efficiency, schema coverage, and output quality.

## Systems evaluated

Guidance · Outlines · llama.cpp · XGrammar · OpenAI · Gemini

## Key findings

- **Speed**: constrained decoding can be 50% faster than unconstrained decoding. Guidance had the fastest token generation rates.
- **Coverage**: best framework supports twice as many real-world schemas as the worst — coverage varies substantially.
- **Quality**: constrained decoding consistently improves downstream task performance by up to 4%, even on minimally structured problems like mathematical reasoning.

## Relevance

Validates the approach used in this project. The 30% → 100% JSON validity result is consistent with the benchmark's findings that structural guidance dramatically improves reliability.
