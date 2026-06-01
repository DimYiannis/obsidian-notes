---
title: "DeepSeek R1"
type: entity
entity_type: project
tags: [llm, reinforcement-learning, reasoning, open-weights, china]
source_count: 1
---

## Overview

LLM reasoning model from DeepSeek (China). First major public paper to detail reinforcement learning training for LLMs at scale. Released as open weights (MIT license). Reignited public interest in RL for LLMs and demonstrated that RL-trained models exhibit emergent [[chain-of-thought]] reasoning without explicit human supervision.

## Key Facts

- **Creator**: DeepSeek AI (Chinese company)
- **License**: MIT — open weights, anyone can download and host
- **Key finding**: applying RL on verifiable domains (math, code) → models spontaneously develop backtracking, self-checking, multi-perspective reasoning ("aha moments")
- **Evidence**: response length grows during RL training → accuracy improves → longer = more thinking
- **Hosted at**: chat.deepseek.com (requires account); also available on together.ai and other inference providers
- **Open weights concern**: Chinese company → data privacy concerns for sensitive use; mitigated by running open weights locally or on US-based providers
- **Context**: ranked ~#3 on LMSYS Chatbot Arena as of 2026; only open-weight model in top tier

## Appearances

- Intro to Large Language Models (2026-06-01) — analyzed in depth; cited as landmark RL paper

## Connections

- [[reinforcement-learning-llm]] — the technique DeepSeek R1 demonstrated publicly
- [[chain-of-thought]] — the emergent behavior it revealed
- [[alphago]] — historical analogy for RL exceeding human performance; Karpathy draws this connection
- [[rlhf]] — contrasted with R1's verifiable-domain RL
