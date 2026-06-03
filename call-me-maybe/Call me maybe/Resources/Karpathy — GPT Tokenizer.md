← [[00 Reading Guide]]

# Let's Build the GPT Tokenizer — Andrej Karpathy
[fast.ai/posts/2025-10-16-karpathy-tokenizers.html](https://www.fast.ai/posts/2025-10-16-karpathy-tokenizers.html)

---

## What it covers

Builds a BPE tokenizer from scratch. The gold standard for understanding how tokens and vocabulary files actually work.

## BPE algorithm

Starts with 256 individual byte tokens. Iteratively:
1. Find the most common consecutive pair
2. Create a new token ID for that pair
3. Replace all occurrences in the sequence
4. Repeat until target vocabulary size

Balances vocabulary size with sequence length efficiency.

## GPT-2 vs GPT-4 differences

**GPT-2**: regex-based pre-tokenization, ~50,257 tokens
**GPT-4**: case-insensitive contraction matching, better newline handling, ~100,277 tokens

## SentencePiece (Llama)

Works at Unicode code point level rather than bytes. Better for non-English languages. Byte fallback for rare characters ensures no information loss.

## LLM struggles traced to tokenization

Spelling difficulties, string processing limitations, non-English problems, arithmetic struggles, inconsistent special token handling — all stem from the token representation, not model capability.

## Relevance to this project

Direct foundation for understanding `vocabulary.py`. The vocabulary JSON file in this project maps token IDs to their string representations — built from exactly this BPE process.
