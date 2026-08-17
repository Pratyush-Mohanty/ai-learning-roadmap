# Hands-on Projects

Every phase ends with a deployable project. Start small, ship, then extend.

## Phase 0 — Cloud Foundations
- [ ] Deploy a batch pipeline: Cloud Storage → BigQuery, orchestrated with Cloud Composer

## Phase 1 — AI/LLM Fundamentals
- [ ] Notebook: call an LLM API, extract structured JSON, handle rate limits/retries, log cost
- [ ] Optional: fine-tune a small model on your own dataset (LlamaFactory)

## Phase 2 — System Design
- [ ] Redesign one of your real past data pipelines at scale (10x volume) and explain it out loud
- [ ] Complete 5-10 classic designs from the Alex Xu framework

## Phase 3 — LLM Engineering
- [ ] **RAG app over your own documents** — pgvector + LangChain/LlamaIndex, with eval + Langfuse monitoring
- [ ] Deploy it as an API (FastAPI) with streaming responses
- [ ] Ship a production chatbot on Dify (if you want a no-code fast win)

## Phase 4 — GCP Data Engineering
- [ ] Streaming pipeline: Pub/Sub → Dataflow → BigQuery, with IAM + cost monitoring
- [ ] Build a Gemini-in-BigQuery analytics demo (LLM + warehouse = your differentiator)

## Phase 5 — Advanced
- [ ] Serve an LLM with vLLM; apply FP8 quantization + speculative decoding; measure before/after
- [ ] Supervisor/worker multi-agent system in LangGraph with checkpointing + observability

## Portfolio Tips
- Put every project on GitHub with a README, architecture diagram, and demo
- One strong project per phase beats five half-finished ones
- Teaching (write a post per project) consolidates learning