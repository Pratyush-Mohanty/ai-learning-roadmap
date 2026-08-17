# LLM Optimization (Advanced)

Production serving, inference speed, and cost. Only after you can build working LLM apps.

## Concepts & Techniques (impact order)
1. **Continuous batching** — up to 23x throughput vs static batching
2. **PagedAttention / KV cache management** — 2-4x, near-zero memory waste
3. **Quantization** — FP8 (gold standard, near-lossless, ~30% speed) → AWQ/GPTQ INT4 (3-4x memory cut) → GGUF (edge)
4. **FlashAttention-3** — 1.5-2x faster attention
5. **Speculative decoding** — EAGLE-3, MTP (DeepSeek V3); 2-3x latency speedup without quality loss
6. **Parallelism** — tensor, pipeline, expert parallelism + disaggregated prefill/decode (for 70B+ models)

## GitHub (inference engines)
- `vllm-project/vllm` — PagedAttention, continuous batching, prefix caching, speculative decoding. Production standard.
- `sgl-project/sglang` — RadixAttention; highest throughput for agent/RAG workloads
- `NVIDIA/TensorRT-LLM` — peak NVIDIA GPU performance; FP8/NVFP4 quantization
- `ggml-org/llama.cpp` — CPU/edge inference, GGUF
- `huggingface/optimum` — quantization (GPTQ, AWQ, bitsandbytes)
- `NVIDIA/Megatron-LM` — tensor/pipeline parallelism for very large models
- `ludwig-ai/llm-inference-solution-architectures` — real-world deployment blueprints

## Guides & deep dives
- NVIDIA Developer Blog: **Mastering LLM Techniques: Inference Optimization** (canonical)
- Red Hat Developer: **Optimizing distributed AI inference** — disaggregation, cache architecture, EAGLE/MTP spec decoding
- `zylos.ai/research/llm-inference-optimization` — quantified impact rankings of each technique
- `youngju.dev` LLM inference guide — KV cache internals, PagedAttention, spec decoding with production configs

## Decision guidance
- Start with vLLM (easy, large community, sufficient for most)
- Use TensorRT-LLM when TTFT matters most on NVIDIA GPUs
- Start quantization at FP8; drop to INT4 only when memory-constrained
- Monitor: GPU cache usage, TTFT, throughput, P99 latency

## Milestone
Serve an LLM with vLLM, benchmark latency/throughput, then apply quantization + speculative decoding and measure the before/after.