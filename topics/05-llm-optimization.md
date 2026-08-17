# LLM Optimization — Serving, Inference, Cost

Only after you can build working LLM apps. These are the levers for throughput, latency, and cost at scale.

## 1. Where the Time Goes
LLM inference = **prefill** (process the input, parallelizable) + **decode** (generate token-by-token, sequential). Two different bottlenecks, different optimizations:
- Prefill: compute-bound (FlashAttention, faster kernels)
- Decode: memory-bandwidth-bound (KV cache, quantization)

## 2. The Techniques (ranked by impact)

### a. Continuous batching — up to 23x
Instead of waiting for a batch to finish, add new requests to running batches continuously. Maximizes GPU utilization. **This alone beats every other optimization.** Comes free with vLLM/TGI/SGLang.

### b. PagedAttention / KV cache management — 2-4x
The KV cache (stored attention keys/values for every token) is the memory hog. **PagedAttention** splits it into pages like virtual memory → near-zero memory waste → bigger effective batch. From vLLM. Prefix caching reuses KV for shared system prompts (huge for RAG/agents).

### c. Quantization — ~30% speed, 3-4x memory cut
Reduce weight precision:
| Method | Bits | Notes |
|---|---|---|
| FP8 | 8 | Gold standard on Hopper/Blackwell; near-lossless, ~30% faster |
| AWQ / GPTQ | 4 | 3-4x memory cut, small quality cost; model-aware |
| GGUF | 4-8 | llama.cpp format for CPU/edge |
Start at FP8; drop to INT4 only when memory-constrained.

### d. FlashAttention — 1.5-2x
IO-aware attention kernel: fewer GPU memory round-trips. Usually on by default in modern engines. FA-3 on H100.

### e. Speculative decoding — 2-3x latency
A small **draft** model guesses the next K tokens; the big model verifies all K in ONE forward pass. Correctness preserved (reject → resample). Best for predictable text (code, structured). Variants: EAGLE-3 (uses target hidden states, no separate draft memory), MTP (DeepSeek V3 trains multi-token heads). **Caveat:** acceptance drops under constrained decoding (JSON/tool calls) — measure before relying on it.

### f. Parallelism & disaggregation (70B+ models)
- **Tensor parallelism:** split a layer's weights across GPUs.
- **Pipeline parallelism:** split layers across GPUs (stage per GPU).
- **Expert parallelism:** MoE models route experts across GPUs (DeepSeek V3/R1).
- **Disaggregated prefill/decode:** separate pools of GPUs for prefill vs decode — each tuned for its bottleneck; KV transferred over network. Big production trend.

## 3. Which Engine (decision-first)
| Engine | When |
|---|---|
| **vLLM** → https://github.com/vllm-project/vllm | Default. Easy, big community, all the above features. |
| **SGLang** → https://github.com/sgl-project/sglang | Highest throughput for agent/RAG workloads (RadixAttention, structured output) |
| **TensorRT-LLM** → https://github.com/NVIDIA/TensorRT-LLM | Peak NVIDIA perf; FP8/NVFP4; best when TTFT matters most |
| **llama.cpp** → https://github.com/ggml-org/llama.cpp | CPU/edge/local; GGUF |
| **Optimum** → https://github.com/huggingface/optimum | Quantization tooling across HF stack |
| **Megatron-LM** → https://github.com/NVIDIA/Megatron-LM | Training + tensor/pipeline parallelism for huge models |

## 4. Production Recipe (combine in this order)
1. Continuous batching (always) — via vLLM
2. FP8 quantization (always on Hopper+)
3. FlashAttention (default on)
4. Prefix caching (long shared system prompts — RAG/agents get 2x+)
5. Speculative decoding (latency-sensitive, non-constrained text)
6. Tensor parallelism / disaggregation (70B+)

## 5. Metrics to Monitor
- **TTFT** (time to first token) — UX-critical
- **Throughput** (tokens/s) — cost-critical
- **P99 latency**, KV cache utilization, GPU memory, spec-decoding acceptance rate (<60% = not working)

## 6. Cost Reality (2026)
- H100 ≈ $3-4/hr. Model + serving choice dominates cost, not kernels.
- Eval before/after every optimization — "optimization without monitoring is blind."

## Go Deeper
- **Canonical:** NVIDIA — Mastering LLM Techniques: Inference Optimization → https://developer.nvidia.com/blog/mastering-llm-techniques-inference-optimization/
- **Distributed:** Red Hat — Optimizing Distributed AI Inference → https://developers.redhat.com/articles/2026/06/24/optimizing-distributed-ai-inference-advanced-deployment-patterns
- **Impact ranking:** zylos.ai research notes → https://zylos.ai/research/2026-01-15-llm-inference-optimization/
- **Deep guide:** youngju.dev — Complete Guide to LLM Inference Optimization → https://www.youngju.dev/blog/llm/2026-03-14-llm-inference-optimization-vllm-tensorrt-speculative-decoding.en
- **Blueprints:** ludwig-ai/llm-inference-solution-architectures → https://github.com/ludwig-ai/llm-inference-solution-architectures