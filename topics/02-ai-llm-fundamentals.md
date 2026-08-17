# AI / LLM Fundamentals — The Mental Model

Understand how LLMs work so downstream engineering decisions (cost, latency, eval, agents) make sense.

## 1. The Core Pipeline
```
Text → Tokenize → Embed → Transformer stack → Predict next token → Decode
```
- **Tokenization:** text is split into tokens (subwords, ~4 chars each). Vocabulary ~50k-200k. `len(text) in tokens` drives every cost/latency number.
- **Embedding:** each token → a high-dimensional vector capturing meaning. Similar meanings → nearby vectors (this powers RAG).
- **Transformer:** the architecture that made LLMs work. Every token attends to every other token (self-attention). Two families: **encoder** (BERT: read-only, classification) and **decoder** (GPT: generate). Modern LLMs are decoder-only.
- **Attention:** for each output position, weigh how much to "look at" each input position. O(n²) cost — this is why long context is expensive (→ [05-llm-optimization.md](05-llm-optimization.md)).
- **Decode:** predict the next token probability distribution, sample from it (temperature, top-k/top-p).

## 2. The Training Phases
1. **Pre-training** — predict next token on trillions of tokens. Expensive (10⁶ GPU-hours), produces base model. This is where the "world knowledge" lives.
2. **Supervised fine-tuning (SFT)** — train on instruction/response pairs → the model learns to follow instructions.
3. **RLHF / Preference optimization** — humans rank outputs; a reward model is trained; the policy is tuned to maximize reward. DPO is a cheaper alternative used widely in 2025+.

**Engineer's takeaway:** you rarely pre-train. You **fine-tune** when behavior/cost/quality on your data demands it, and you use **prompting/RAG** for most things first. Fine-tuning for *format, style, domain tone*; RAG for *facts and freshness*.

## 3. Inference-Time Concepts
- **Context window:** max tokens in + out. Long context is now 128k-2M, but attention cost grows, and *"needle in haystack"* retrieval quality still degrades.
- **Sampling params:** temperature (0 = greedy/deterministic, high = creative), top-p (nucleus), max tokens, stop sequences.
- **Structured output:** force JSON/schema via constrained decoding or tool-calling — essential for engineering. Learn Instructor (https://github.com/instructor-ai/instructor).
- **Hallucination:** fluent but false output. Mitigations: grounding (RAG), citations, lower temperature for factual tasks, eval harnesses. You cannot eliminate it; you bound it.

## 4. Evaluating an LLM (the part everyone skips)
- **Offline:** golden datasets + metrics (accuracy, F1, RAGAS for RAG), LLM-as-judge, human eval.
- **Online:** A/B tests, guardrails monitoring, user feedback signals.
- **Red-teaming:** adversarial prompts for safety/robustness.
- Rule: **evaluate before and after every change** (prompt tweak, model swap, chunking change). Without evals you're guessing.

## 5. Cost & Latency Quick Math (internalize these)
- Cost ≈ tokens_in × input_price + tokens_out × output_price (output ~3-4x input price).
- Latency ≈ prefill (process input) + decode (generate output, ~1 token/few ms).
- Reducing cost: shorter prompts, caching, smaller model routing (LiteLLM), quantization.

## Go Deeper
- **Course:** DeepLearning.AI — LLM Engineering for Everyone → https://www.deeplearning.ai/short-courses/llm-engineering-for-everyone/
- **Course:** Generative AI with LLMs (Coursera, DeepLearning.AI+AWS) → https://www.coursera.org/learn/generative-ai-with-llms
- **Video:** Karpathy, Neural Networks: Zero to Hero → https://www.youtube.com/playlist?list=PLAqhIrjkxbuWI23v9cThsA9GvCAUhRvKZ
- **Repo:** rasbt/LLMs-from-scratch (build one in PyTorch) → https://github.com/rasbt/LLMs-from-scratch
- **Repo:** mlabonne/llm-course → https://github.com/mlabonne/llm-course
- **Repo:** Prompt Engineering Guide (dair-ai) → https://github.com/dair-ai/Prompt-Engineering-Guide
- **Index:** Awesome-LLM → https://github.com/Hannibal046/Awesome-LLM