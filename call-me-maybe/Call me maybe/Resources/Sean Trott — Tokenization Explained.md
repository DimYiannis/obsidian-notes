← [[00 Reading Guide]]

# Tokenization in Large Language Models, Explained — Sean Trott
[seantrott.substack.com/p/tokenization-in-large-language-models](https://seantrott.substack.com/p/tokenization-in-large-language-models)

---

## Core point

LLMs predict **tokens**, not words. Tokenization breaks sequences into discrete units forming the model's vocabulary.

## Three approaches

| Approach | Vocabulary | Weakness |
|---|---|---|
| Word-based | Large | Poor on unknown words |
| Character-based | Small | Hard to learn word-level patterns |
| Subword (BPE) | Balanced | Splits don't match linguistic structure |

## BPE

Starts with individual characters, iteratively merges frequent combinations until target vocabulary size. Creates tokens like `##et` as subword pieces.

## Surprising finding

**Subwords ≠ morphemes.** Decomposition is driven by frequency, not linguistic structure. "racket" splits into two tokens; "dogs" stays as one — regardless of morphological boundaries. The model learns statistical patterns, not grammar.

## Why this matters for this project

Function names like `fn_add_numbers` split based on BPE frequency statistics, not semantics. That's why prefix matching against the full vocabulary is necessary — you can't predict the split from the function name alone.
