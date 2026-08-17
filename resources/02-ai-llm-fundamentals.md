# AI / LLM Fundamentals

Theoretical foundation. Keep this phase short — go deep only if you want the math.

## Start Here (free, fast)
- **DeepLearning.AI — LLM Engineering for Everyone** — https://www.deeplearning.ai/short-courses/llm-engineering-for-everyone/
  ~1 hr, free. Skip if you already grasp tokens/embeddings/attention at a high level.
- **Karpathy: Neural Networks: Zero to Hero** — https://www.youtube.com/playlist?list=PLAqhIrjkxbuWI23v9cThsA9GvCAUhRvKZ
  Transformers from first principles, if you want the real math.

## Courses
- **Generative AI with LLMs** (DeepLearning.AI + AWS, Coursera) — https://www.coursera.org/learn/generative-ai-with-llms
  Fine-tuning, RLHF, deployment. One course, high signal-to-noise.
- **Google Cloud: Introduction to Large Language Models** — https://www.coursera.org/learn/introduction-to-large-language-models
  Clear conceptual explanation: tokenization, embeddings, training dynamics, inference.

## GitHub (learn by building)
- **Build a Large Language Model (From Scratch)** — https://github.com/rasbt/LLMs-from-scratch
  Build a ChatGPT-like LLM in PyTorch step by step. Deepest hands-on.
- **LLM Course** — https://github.com/mlabonne/llm-course
  Roadmap: fundamentals → RAG → fine-tuning with Colab notebooks.
- **Prompt Engineering Guide** — https://github.com/dair-ai/Prompt-Engineering-Guide
  Prompt engineering reference. Learn once, then move on.
- **Awesome-LLM** — https://github.com/Hannibal046/Awesome-LLM
  Index of seminal papers, courses, benchmarks. Use as a map, not a book.

## Concepts to master
- Tokenization, embeddings, attention, transformer architecture
- Pre-training vs fine-tuning vs RLHF
- Context windows, temperature/sampling, structured output
- Hallucination, grounding, evaluation basics

## Milestone
A notebook that: loads a model or calls an API, extracts structured data (JSON), handles rate limiting/retries, and logs cost.