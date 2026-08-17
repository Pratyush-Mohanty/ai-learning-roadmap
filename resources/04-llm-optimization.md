# LLM Optimization (Advanced)

Production serving, inference speed, and cost. Only after you can build working LLM apps.

## Techniques (in impact order)
1. **Continuous batching** — up to 23x throughput vs static batching
2. **PagedAttention / KV cache management** — 2-4x, near-zero memory waste
3. **Quantization** — FP8 (gold standard, near-lossless, ~30% speed) → AWQ/GPTQ INT4 (3-4x memory cut) → GGUF (edge)
4. **FlashAttention-3** — 1.5-2x faster attention
5. **Speculative decoding** — EAGLE-3, MTP (DeepSeek V3); 2-3x latency speedup without quality loss
6. **Parallelism** — tensor, pipeline, expert parallelism + disaggregated prefill/decode (70B+ models)

## Inference Engines (GitHub)
- **vLLM** — https://github.com/vllm-project/vllm
  PagedAttention, continuous batching, prefix caching, speculative decoding. Production standard.
- **SGLang** — https://github.com/sgl-project/sglang
  RadixAttention; highest throughput for agent/RAG workloads.
- **TensorRT-LLM** — https://github.com/NVIDIA/TensorRT-LLM
  Peak NVIDIA GPU performance; FP8/NVFP4 quantization.
- **llama.cpp** — https://github.com/ggml-org/llama.cpp
  CPU/edge inference, GGUF quantization.
- **Hugging Face Optimum** — https://github.com/huggingface/optimum
  Quantization (GPTQ, AWQ, bitsandbytes) across frameworks.
- **Megatron-LM** — https://github.com/NVIDIA/Megatron-LM
  Tensor/pipeline parallelism for very large models.
- **LLM Inference Solution Architectures** — https://github.com/ludwig-ai/llm-inference-solution-architectures
  Real-world deployment blueprints.

## Guides & Deep Dives
- **NVIDIA: Mastering LLM Techniques — Inference Optimization** — https://developer.nvidia.com/blog/mastering-llm-techniques-inference-optimization/
  The canonical reference.
- **Red Hat: Optimizing Distributed AI Inference** — https://developers.redhat.com/articles/2026/06/24/optimizing-distributed-ai-inference-advanced-deployment-patterns
  Disaggregation, cache architecture, EAGLE/MTP speculative decoding.
- **zylos.ai: LLM Inference Optimization** — https://zylos.ai/research/2026-01-15-llm-inference-optimization/
  Quantified impact rankings of each technique.
- **youngju.dev: Complete Guide to LLM Inference Optimization** — https://www.youngju.dev/blog/llm/2026-03-14-llm-inference-optimization-vllm-tensorrt-speculative-decoding.en
  KV cache internals, PagedAttention, speculative decoding with production configs.

## Decision Guidance
- Start with vLLM (easy, large community, sufficient for most)
- Use TensorRT-LLM when TTFT matters most on NVIDIA GPUs
- Start quantization at FP8; drop to INT4 only when memory-constrained
- Monitor: GPU cache usage, TTFT, throughput, P99 latency

## Milestone
Serve an LLM with vLLM, benchmark latency/throughput, then apply quantization + speculative decoding and measure the before/after.