# AI / LLM Fundamentals - Comprehensive Study Guide

**Estimated time: 2 weeks.** The mental model you need before building anything. Work through sections in order.

---

## 1. What an LLM Actually Does

At the most basic level, an LLM is a **probability machine for the next token**:

```
Input text ──▶ tokenize ──▶ embed ──▶ transformer ──▶ predict next token ──▶ sample ──▶ append
                                                     ▲                                 │
                                                     └─────────────────────────────────┘
```

It generates text one token at a time, feeding its own output back in.

### The 4 core concepts
1. **Tokenization:** text → subword units (~4 chars each). Every cost/latency number is token-based. Learn to estimate: 1 English word ≈ 1.3 tokens; 1000 tokens ≈ 750 words.
2. **Embeddings:** token → high-dim vector. Meaning captured as vector proximity. Powers search, RAG, clustering.
3. **Self-attention:** each token "looks at" every other token, weighted. O(n²) compute → long context is expensive (this drives the optimizations in topic 05).
4. **Decoding:** sample next token from a probability distribution using temperature and top-p.

---

## 2. Attention, Intuitively

For each word in a sentence, attention decides *which other words matter and how much*.

"Mary **gave** the book to John."

- The word "gave" attends to "Mary" (who gave) and "book" (what) and "John" (to whom).
- The model learns these weights during training.

### Why O(n²) matters
For a 10k-token prompt, attention computes 100M pairs. For 100k tokens, 10 billion. This is why:
- Context windows are expensive
- KV caching exists (reuse computations across requests)
- FlashAttention and sparse attention exist

---

## 3. The Training Phases

```
Pre-training ──▶ SFT ──▶ RLHF/DPO ──▶ Deploy
  (predict      (learn to    (align with
   next token)   follow       human
                 instructions) preferences)
```

| Phase | What happens | Cost | Result |
|---|---|---|---|
| Pre-training | Predict next token on trillions of tokens | Millions $ / 10⁶ GPU-hrs | Base model (world knowledge) |
| SFT | Train on instruction/response pairs | Expensive | Chat/instruct model |
| RLHF | Humans rank outputs → reward model → policy tune | Expensive | Aligned, helpful model |
| DPO | Direct preference optimization (no reward model) | Cheaper | Similar alignment |

**The engineer's takeaway:** you never pre-train. You either use a base/instruct model via API, or fine-tune when behavior/cost/quality demands it.

---

## 4. Prompting vs RAG vs Fine-tuning (decide in this order)

```
Need new/fresh/private facts? ──▶ RAG
Need style/format/domain behavior? ──▶ Fine-tuning
Need a small tweak? ──▶ Prompting
Need both facts AND style? ──▶ RAG + Fine-tuning
```

| Approach | What it changes | Cost | When |
|---|---|---|---|
| Prompting | Instructions only | ~0 | Quick tweaks, most cases first |
| RAG | What the model can access | Low-medium | Fresh/private facts, citations |
| Fine-tuning | Model weights | High | Style, format, domain tone, latency/cost optimization |

**Rule:** RAG first, always. Fine-tune only when justified by quality/cost/behavior requirements.

---

## 5. Inference-Time Concepts You Must Know

### Context window
Max tokens in + out. Modern windows are 128k-2M but quality degrades with distance ("lost in the middle" phenomenon).

### Sampling parameters
- **Temperature:** 0 = greedy/deterministic; higher = more creative. Facts want low temp; brainstorming wants high.
- **Top-p (nucleus):** sample from the smallest set of tokens whose cumulative probability ≥ p. Usually 0.9-0.95.
- **Max tokens:** hard cap on output.
- **Stop sequences:** end generation at a marker.

### Structured output
Force the model to produce JSON/schema via constrained decoding or function/tool calling. Essential for engineering — never parse free text. Learn Instructor: https://github.com/instructor-ai/instructor

### Hallucination
Fluent but false output. You cannot eliminate it — you **bound it**:
- Grounding (RAG + citations)
- Lower temperature for factual tasks
- Eval harnesses that catch fabrication
- Instruct "answer only from context; say I don't know otherwise"

---

## 6. Evaluating an LLM (the part everyone skips)

### Offline evaluation
- **Golden dataset:** curated Q&A pairs with expected answers
- **Metrics:** accuracy, F1, ROUGE/BLEU for summaries, RAGAS for RAG quality
- **LLM-as-judge:** a strong model scores outputs (fast, scalable, sometimes biased)
- **Human eval:** gold standard, expensive

### Online evaluation
- A/B tests between model versions
- Guardrail monitoring (refusals, toxicity, hallucination rate)
- User feedback signals (thumbs up/down, escalation rate)

### The rule
Evaluate BEFORE and AFTER every change — prompt tweak, model swap, chunking change, fine-tune. Without evals, you're guessing.

---

## 7. Cost & Latency Math (internalize these numbers)

```
cost = in_tokens × input_price + out_tokens × output_price
       (output tokens cost ~3-4x input price)

latency = prefill (process input, parallelizable)
        + decode (generate tokens, ~1 token / few ms)
```

### Cost levers
1. Shorter prompts (trim system prompt, summarize conversation)
2. Cache repeated queries (exact-match cache)
3. Model routing — cheap model for easy queries (LiteLLM)
4. Quantization (see topic 05)
5. Output limits (max_tokens caps)

---

## 8. Key API Patterns (for your Phase 1 project)

1. **Retries with exponential backoff** — transient errors happen; never retry immediately in a loop.
2. **Rate limiting** — respect provider RPM/TPM; use token bucket locally.
3. **Streaming** — request stream=true, consume tokens as they arrive (better UX + lower perceived latency).
4. **Structured output** — use response_format/JSON schema.
5. **Log everything** — request/response, tokens, latency, cost. This IS your eval data later.

---

## 9. Two-Week Study Plan

**Week 1 — Concepts:**
- Day 1-2: Sections 1-2 (what an LLM does, attention)
- Day 3: Section 3 (training phases) + optional Karpathy video
- Day 4-5: Sections 4-5 (RAG vs FT, inference concepts)
- Day 6-7: Sections 6-7 (eval, cost math)

**Week 2 — Build:**
- Day 8-10: DeepLearning.AI LLM Engineering for Everyone (free)
- Day 11-14: Phase 1 project — notebook with structured extraction, retries, cost logging

---

## Go Deeper

- Course: DeepLearning.AI - LLM Engineering for Everyone - https://www.deeplearning.ai/short-courses/llm-engineering-for-everyone/
- Course: Generative AI with LLMs - https://www.coursera.org/learn/generative-ai-with-llms
- Build one in PyTorch: https://github.com/rasbt/LLMs-from-scratch
- Roadmap: https://github.com/mlabonne/llm-course
- Prompts: https://github.com/dair-ai/Prompt-Engineering-Guide
- Structured output: https://github.com/instructor-ai/instructor