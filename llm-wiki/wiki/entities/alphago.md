---
title: "AlphaGo"
type: entity
entity_type: project
tags: [reinforcement-learning, deepmind, game-ai, historical]
source_count: 1
---

## Overview

DeepMind's Go-playing AI. First system to defeat top human Go players using reinforcement learning. Key historical precedent for [[reinforcement-learning-llm]]: demonstrates that RL can exceed human-level performance in a way that imitation learning (SFT equivalent) cannot.

## Key Facts

- **Creator**: DeepMind (Google)
- **Technique**: RL via self-play; samples many move sequences, reinforces sequences that lead to winning
- **Key result**: ELO rating far exceeds top human players (including Lee Sedol)
- **SL vs RL comparison**: supervised learning (imitating human expert games) plateaus below top humans; RL via self-play blows past them
- **Move 37**: during Lee Sedol match, AlphaGo played a move with ~1-in-10,000 probability of being played by a human — in retrospect brilliant. Demonstrates RL discovering non-human strategies.
- **Analogy to LLMs**: RL for LLMs can discover reasoning strategies humans wouldn't think to write down in SFT data

## Appearances

- Intro to Large Language Models (2026-06-01) — cited as precedent for RL exceeding human performance; Move 37 discussed as analogy for emergent LLM strategies

## Connections

- [[reinforcement-learning-llm]] — AlphaGo is the historical proof-of-concept for this paradigm applied to LLMs
- [[supervised-fine-tuning]] — SL equivalent in AlphaGo (imitating human games) shows the ceiling; RL blows past it
- [[deepseek-r1]] — first LLM paper to demonstrate analogous RL breakthroughs in open domain
- [[chain-of-thought]] — LLM equivalent of Move 37: strategies no human would explicitly author
