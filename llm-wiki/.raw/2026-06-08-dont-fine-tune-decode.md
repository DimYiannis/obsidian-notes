# Don't Fine-Tune, Decode: Syntax Error-Free Tool Use via Constrained Decoding

*Source: arxiv.org/abs/2310.07075. Zhang et al. (2023). Ingested 2026-06-08. Immutable.*

Authors: Kexun Zhang, Hongqiao Chen, Lei Li, William Wang

Abstract: Instruction-tuned LLMs frequently struggle with external tool use due to syntax complexity. Rather than relying on expensive fine-tuning, the authors propose TOOLDEC, a decoding algorithm employing finite state machines to enforce tool syntax compliance. Testing shows it eliminates syntax errors across multiple models and benchmarks, notably improving Mistral-Instruct's tool use accuracy from 0% to 52%.

Core argument: syntax constraints are only learned implicitly during fine-tuning — models still make frequent syntax errors. Applying constraints explicitly at decode time through finite state machines ensures compliance reliably. This approach avoids expensive fine-tuning while achieving better results than specialized fine-tuned models.
