# LLM Optimization - Serving, Inference, Cost (Comprehensive)

**Estimated time: 3 weeks.** The levers for throughput, latency, and cost at scale. Only after you can build working LLM apps.

---

## 1. Where the Time Goes

```
Inference = PREFILL + DECODE

PREFILL:  process the input            → compute-bound (GPUs busy)
DECODE:   generate tokens one by one   → memory-bandwidth-bound (slowest link)
```

Two different bottlenecks → different optimizations:
- **Prefill** → FlashAttention, faster kernels
- **Decode** → KV cache management, quantization

---

## 2. The Technique Stack (ranked by impact)

```
1. Continuous batching ......... up to 23x throughput
2. PagedAttention + prefix cache . 2-4x
3. Quantization (FP8) ........... ~30% speed, 3-4x memory cut
4. FlashAttention ............... 1.5-2x
5. Speculative decoding ......... 2-3x latency (no quality loss)
6. Parallelism + disaggregation . scale to 70B+ models
```

### 2.1 Continuous batching (always, free)
Instead of waiting for a batch to finish, add new requests to running batches continuously. Maximizes GPU utilization. **This alone beats every other optimization.** Comes free with vLLM/TGI/SGLang.

### 2.2 PagedAttention / KV cache
The KV cache (stored attention keys/values for every token) is the memory hog. **PagedAttention** splits it into pages like virtual memory → near-zero waste → bigger effective batch.
- **Prefix caching:** reuse KV for shared system prompts → RAG/agents get 2x+ throughput.
- From vLLM. This is why "vLLM" is the default serving engine.

### 2.3 Quantization (reduce weight precision)
| Method | Bits | Notes |
|---|---|---|
| FP8 | 8 | Gold standard on Hopper/Blackwell; near-lossless, ~30% speed |
| AWQ / GPTQ | 4 | 3-4x memory cut; model-aware, small quality cost |
| GGUF | 4-8 | llama.cpp format for CPU/edge |

Start FP8. Drop to INT4 only when memory-constrained.

### 2.4 FlashAttention
IO-aware attention kernel — fewer GPU memory round-trips. Usually on by default. FA-3 on H100.

### 2.5 Speculative decoding
A small **draft** model predicts the next K tokens; the big model verifies all K in ONE forward pass. Correctness preserved (reject → resample).
- Variants: **EAGLE-3** (uses target hidden states, no separate draft), **MTP** (DeepSeek V3 trains multi-token heads; >80% acceptance).
- **Caveat:** acceptance collapses under constrained decoding (JSON/tool calls) — measure before relying on it.

### 2.6 Parallelism & disaggregation (70B+ models)
| Technique | What | When |
|---|---|---|
| Tensor parallelism | Split layer weights across GPUs | Always for big models |
| Pipeline parallelism | Split layers across GPUs | Large models |
| Expert parallelism | MoE experts across GPUs | MoE (DeepSeek) |
| Disaggregated prefill/decode | Separate GPU pools for each phase | Peak production |

---

## 3. Which Engine (decision-first)

| Engine | When |
|---|---|
| vLLM | Default. Easy, big community, all features. |
| SGLang | Highest throughput for agent/RAG workloads (RadixAttention) |
| TensorRT-LLM | Peak NVIDIA perf; best TTFT; FP8/NVFP4 |
| llama.cpp | CPU / edge / local; GGUF |
| Optimum | Quantization tooling across HF stack |
| Megatron-LM | Parallelism for huge models |

---

## 4. Production Recipe (combine in this order)

```
1. Continuous batching      (vLLM default) — always
2. FP8 quantization         — always on Hopper+
3. FlashAttention           — default on
4. Prefix caching           — RAG/agents with long shared prompts
5. Speculative decoding     — latency-sensitive, non-constrained text
6. Tensor parallelism / disaggregation — 70B+
```

---

## 5. Metrics to Monitor

| Metric | What it tells you |
|---|---|
| TTFT (time to first token) | UX; prefill quality |
| Throughput (tokens/s) | Cost per token |
| P99 latency | Worst-case experience |
| KV cache utilization | Memory efficiency |
| GPU memory | Headroom for bigger batch |
| Spec-decoding acceptance rate | <60% = not working |

---

## 6. Cost Reality (2026)

- H100 ≈ $3-4/hr. **Model + serving choice dominates cost**, not kernel micro-optimizations.
- Eval before/after every optimization. "Optimization without monitoring is blind."

---

## 7. Three-Week Study Plan

**Week 1 — Fundamentals:**
- Days 1-3: Prefill vs decode, KV cache, PagedAttention
- Days 4-7: Continuous batching, FlashAttention

**Week 2 — Quantization + engines:**
- Days 8-10: FP8/AWQ/GPTQ practice with vLLM
- Days 11-14: Serve a model with vLLM; benchmark TTFT/throughput

**Week 3 — Advanced:**
- Prefix caching + speculative decoding, measure before/after
- Try SGLang for an agent/RAG workload
- (Optional) TensorRT-LLM or disaggregation deep dive

---

## Go Deeper

- NVIDIA canonical guide: https://developer.nvidia.com/blog/mastering-llm-techniques-inference-optimization/
- Red Hat distributed inference: https://developers.redhat.com/articles/2026/06/24/optimizing-distributed-ai-inference-advanced-deployment-patterns
- Impact rankings: https://zylos.ai/research/2026-01-15-llm-inference-optimization/
- Deep guide: https://www.youngju.dev/blog/llm/2026-03-14-llm-inference-optimization-vllm-tensorrt-speculative-decoding.en
- Blueprints: https://github.com/ludwig-ai/llm-inference-solution-architectures
- vLLM: https://github.com/vllm-project/vllm
- SGLang: https://github.com/sgl-project/sglang
- TensorRT-LLM: https://github.com/NVIDIA/TensorRT-LLM