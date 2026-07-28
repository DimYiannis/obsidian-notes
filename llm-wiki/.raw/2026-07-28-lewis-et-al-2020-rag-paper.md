# Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks

*Source: arxiv.org/abs/2005.11401. Lewis et al. (2020, NeurIPS; Facebook AI Research). Ingested 2026-07-28. Immutable.*

Authors: Patrick Lewis, Ethan Perez, Aleksandra Piktus, Fabio Petroni, Vladimir Karpukhin, Naman Goyal, Heinrich Küttler, Mike Lewis, Wen-tau Yih, Tim Rocktäschel, Sebastian Riedel, Douwe Kiela

Abstract: Large pre-trained language models store factual knowledge in their parameters but can't easily access or precisely manipulate it, and can't straightforwardly update that knowledge or provide provenance for their decisions. The paper introduces RAG models — combining a pre-trained parametric seq2seq model (BART) with a non-parametric memory (a dense vector index over Wikipedia, accessed via a pre-trained neural retriever, DPR) — and fine-tunes the whole thing end-to-end. Two formulations are compared: RAG-Sequence, which uses the same retrieved document to generate the entire output, and RAG-Token, which can draw on different documents per generated token.

Architecture: retriever (DPR, BERT bi-encoder, MIPS over 21M Wikipedia passages) + generator (BART, 400M params) combined probabilistically, marginalizing over the top-k retrieved documents. Document encoder and index are frozen during fine-tuning; only the query encoder and BART are fine-tuned. No supervision on which document should be retrieved — retrieval quality emerges from end-task training via the negative marginal log-likelihood objective.

Results: state-of-the-art on four open-domain QA benchmarks (Natural Questions 44.5% EM, WebQuestions 68.0%, TriviaQA 56.8%, CuratedTrec 45.2%), beating both purely parametric (T5-11B+SSM) and purely extractive (DPR) baselines. On MS-MARCO, +2.6 BLEU/ROUGE-L over BART. On Jeopardy question generation, human evaluators judged RAG more factual than BART in 42.7% vs 7.1% of cases. FEVER fact verification: 72.5% accuracy (3-way) with no retrieval supervision.

Key qualitative findings: (1) swapping the retrieval index (2016 vs 2018 Wikipedia dump) correctly updates the model's answers to time-sensitive questions without retraining — the paper's core argument for non-parametric over parametric memory; (2) RAG answers 11.8% of NQ questions correctly even when the retrieved documents don't contain the answer, vs 0% for extractive models, since the parametric side still contributes; (3) RAG-Sequence produces more diverse generations (83.5% distinct trigrams vs BART's 70.7%) without diversity-promoting decoding tricks.

Limitations: on tasks without a clear factual grounding need (e.g. open-ended story generation), the retriever can "collapse" — learns to retrieve the same document regardless of input, so retrieval becomes a no-op. Some MS-MARCO questions are unanswerable from Wikipedia alone.
