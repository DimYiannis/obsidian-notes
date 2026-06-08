# Resources — call me maybe

## Papers

### Core — read these first

- **Efficient Guided Generation for Large Language Models** — Willard & Louf (2023) → [[Willard & Louf (2023) — Outlines]]
  [https://arxiv.org/abs/2307.09702](https://arxiv.org/abs/2307.09702)
  The theoretical foundation of this project. Shows how token generation can be modelled as FSM transitions, leading to an efficient approach to constrained decoding using a vocabulary index.

- **Don't Fine-Tune, Decode: Syntax Error-Free Tool Use via Constrained Decoding** (2023) → [[Don't Fine-Tune, Decode (2023)]]
  [https://arxiv.org/abs/2310.07075](https://arxiv.org/abs/2310.07075)
  Directly about function calling + constrained decoding. Argues for decoding-time constraints over fine-tuning. Mistral-Instruct: 0% → 52% tool use accuracy with constrained decoding.

### Supplementary

- **Draft-Conditioned Constrained Decoding (DCCD)** (2025) → [[Draft-Conditioned Constrained Decoding (2025)]]
  [https://arxiv.org/pdf/2603.03305](https://arxiv.org/pdf/2603.03305)
  Shows how constrained decoding guarantees that every emitted token preserves global validity. Good for understanding why masking to `-inf` works.

- **JSONSchemaBench — Generating Structured Outputs from Language Models** (2024) → [[JSONSchemaBench (2024)]]
  [https://arxiv.org/html/2501.10868v1](https://arxiv.org/html/2501.10868v1)
  Benchmarks six constrained decoding frameworks (Guidance, Outlines, Llamacpp, XGrammar, OpenAI, Gemini) on real-world JSON schemas. Useful for the performance analysis section.

- **Thinking Before Constraining: A Unified Decoding Framework** (2025) → [[Thinking Before Constraining (2025)]]
  [https://arxiv.org/pdf/2601.07525](https://arxiv.org/pdf/2601.07525)
  Discusses the limitations of unconstrained LLMs for structured output and the role of grammar engines like Outlines and XGrammar.

- **Awesome LLM Constrained Decoding** — curated paper list
  [https://github.com/Saibo-creator/Awesome-LLM-Constrained-Decoding](https://github.com/Saibo-creator/Awesome-LLM-Constrained-Decoding)
  Good starting point if you want to go deeper into the literature.

---

## Videos

### Tokenization

- **A visual introduction to tokenization in LLMs — BPE Algorithm**
  [https://www.youtube.com/watch?v=APnKbi448O4](https://www.youtube.com/watch?v=APnKbi448O4)
  Visual and clear. Essential before working on `vocabulary.py`.

- **Let's Build the GPT Tokenizer — Andrej Karpathy** → [[Karpathy — GPT Tokenizer]]
  [https://www.fast.ai/posts/2025-10-16-karpathy-tokenizers.html](https://www.fast.ai/posts/2025-10-16-karpathy-tokenizers.html)
  Builds a BPE tokenizer from scratch. Longer but the gold standard for understanding how tokens and vocabulary files actually work.

### Function Calling

- **Tool Calling Explained: Turn Your LLM into an AI Agent**
  [https://www.youtube.com/watch?v=KJf7SqPCRXg](https://www.youtube.com/watch?v=KJf7SqPCRXg)
  Explains how function/tool calling enables LLMs to interact with external tools. Good first watch.

---

## Articles

### Function Calling

- **Function calling using LLMs — Martin Fowler** → [[Martin Fowler — Function Calling]]
  [https://www.martinfowler.com/articles/function-call-LLM.html](https://www.martinfowler.com/articles/function-call-LLM.html)
  Well written, practical. Explains that the LLM never executes the function — it just outputs a structured description of the call.

- **Function calling in LLMs — Xe Iaso (talk transcript)** → [[Xe Iaso — Function Calling Talk]]
  [https://xeiaso.net/talks/2024/llm-function-calling/](https://xeiaso.net/talks/2024/llm-function-calling/)
  Short and punchy. Clears up the most common misconception: the model isn't calling anything itself.

- **Function Calling — Prompt Engineering Guide**
  [https://www.promptingguide.ai/applications/function_calling](https://www.promptingguide.ai/applications/function_calling)
  Good reference for JSON schema format for function definitions.

- **Guide to Tool Calling in LLMs — Analytics Vidhya**
  [https://www.analyticsvidhya.com/blog/2024/08/tool-calling-in-llms/](https://www.analyticsvidhya.com/blog/2024/08/tool-calling-in-llms/)
  Practical overview with examples across multiple providers.

### Constrained Decoding

- **Beyond Free-Form Text: How Constrained Decoding is Reshaping Structured Generation** → [[Beyond Free-Form Text — Constrained Decoding Survey]]
  [https://medium.com/@brijeshrn/beyond-free-form-text-how-constrained-decoding-is-reshaping-structured-generation-in-llms-5f7a38bef259](https://medium.com/@brijeshrn/beyond-free-form-text-how-constrained-decoding-is-reshaping-structured-generation-in-llms-5f7a38bef259)
  Good survey of the landscape: DOMINO, Outlines, XGrammar, Guidance. Explains the subword alignment problem well.

- **Outlines paper explained — Medium**
  [https://id2thomas.medium.com/nlg-outlines-efficient-guided-generation-for-large-language-models-willard-louf-2023-3c9463543901](https://id2thomas.medium.com/nlg-outlines-efficient-guided-generation-for-large-language-models-willard-louf-2023-3c9463543901)
  Easier digest of the Willard & Louf paper. Read this alongside the paper itself.

### Tokenization

- **How LLM Tokenization Works: Build a BPE Tokenizer**
  [https://machinelearningplus.com/gen-ai/build-bpe-tokenizer/](https://machinelearningplus.com/gen-ai/build-bpe-tokenizer/)
  Hands-on with interactive code. Useful for understanding why the same string can map to different token IDs depending on context.

- **Tokenization in large language models, explained** → [[Sean Trott — Tokenization Explained]]
  [https://seantrott.substack.com/p/tokenization-in-large-language-models](https://seantrott.substack.com/p/tokenization-in-large-language-models)
  Accessible explanation of BPE and why subword splits matter.

---

## Libraries (forbidden to use — read only for understanding)

- **Outlines** — [https://github.com/outlines-dev/outlines](https://github.com/outlines-dev/outlines)
  The reference implementation of FSM-based constrained decoding.

- **XGrammar** — [https://github.com/mlc-ai/xgrammar](https://github.com/mlc-ai/xgrammar)
  More recent grammar engine. Fast and efficient subword-aligned constrained decoding.

- **Guidance** — [https://github.com/guidance-ai/guidance](https://github.com/guidance-ai/guidance)
  Microsoft's constrained generation library.

---

## Suggested Reading Order

| Step | Resource | Why |
|---|---|---|
| 1 | Xe Iaso article | Clear up function calling basics (10 min) |
| 2 | Tokenization video (BPE) | Understand tokens before touching vocabulary.py |
| 3 | Willard & Louf paper | Core theory behind your state machine |
| 4 | Outlines Medium explainer | Easier digest of the same paper |
| 5 | [[Don't Fine-Tune, Decode (2023)]] | Directly about function calling + constrained decoding |
| 6 | Draft-Conditioned paper | Production context, why masking works |
| 7 | JSONSchemaBench | For the README performance analysis section |
