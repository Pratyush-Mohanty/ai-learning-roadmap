# AI / LLM Fundamentals

Theoretical foundation. Keep this phase short — go deep only if you want the math.

## Start Here (free, fast)
- DeepLearning.AI — **LLM Engineering for Everyone** (~1hr, free). Skip if you already grasp tokens/embeddings/attention at a high level.
- **Karpathy's *Neural Networks: Zero to Hero*** (YouTube) — transformers from first principles, if you want the real math

## Courses
- Coursera: **Generative AI with LLMs** (DeepLearning.AI + AWS) — fine-tuning, RLHF, deployment
- Google Cloud: *Introduction to LLMs* (Coursera) — clear conceptual explanation without jargon

## GitHub (learn by building)
- `github.com/rasbt/LLMs-from-scratch` — build a ChatGPT-like LLM in PyTorch step by step (deepest hands-on)
- `github.com/mlabonne/llm-course` — roadmap: fundamentals → RAG → fine-tuning with Colab notebooks
- `github.com/dair-ai/Prompt-Engineering-Guide` — prompt engineering reference (learn once)
- `github.com/Hannibal046/Awesome-LLM` — curated papers, courses, benchmarks (use as index)

## Concepts to master
- Tokenization, embeddings, attention, transformer architecture
- Pre-training vs fine-tuning vs RLHF
- Context windows, temperature/sampling, structured output
- Hallucination, grounding, evaluation basics

## Milestone
A notebook that: loads a model or calls an API, extracts structured data (JSON), handles rate limiting/retries, and logs cost.