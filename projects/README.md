# Hands-on Projects

Every phase ends with a deployable project. One strong project per phase beats five half-finished ones.

## Phase 0 — Cloud Foundations
- [ ] **Batch pipeline on GCP:** Cloud Storage → BigQuery with Cloud Composer orchestration (free credits)
- [ ] Deploy a service on Cloud Run via Terraform, behind a global HTTPS load balancer

## Phase 1 — AI/LLM Fundamentals
- [ ] **LLM API notebook:** structured JSON extraction, rate-limit/retry handling, cost logging
- [ ] Optional: fine-tune a small model on your own dataset with LlamaFactory (https://github.com/hiyouga/LlamaFactory)

## Phase 2 — System Design
- [ ] **Redesign a real pipeline at 10x scale** — one you've built; explain tradeoffs out loud, record it
- [ ] 5-10 classic designs from the Alex Xu framework

## Phase 3 — LLM Engineering (flagship)
- [ ] **RAG over your own documents:** pgvector + LangChain/LlamaIndex, chunking experiments, hybrid search, reranking
- [ ] Add eval (RAGAS) + Langfuse tracing; document the quality before/after
- [ ] Deploy as FastAPI with streaming (SSE)
- [ ] Optional fast win: production chatbot on Dify (https://github.com/langgenius/dify)

## Phase 4 — GCP Data Engineering
- [ ] **Streaming pipeline:** Pub/Sub → Dataflow → BigQuery with windows/watermarks, IAM, cost monitoring
- [ ] **Lakehouse:** Iceberg table via BigLake, ingested with Storage Write API, queried by Spark + BigQuery
- [ ] **Gemini in BigQuery demo** — run an LLM inside the warehouse (your differentiator)

## Phase 5 — Advanced
- [ ] **vLLM serving:** serve a model, benchmark TTFT/throughput, apply FP8 + prefix caching + speculative decoding, measure before/after
- [ ] **Multi-agent system:** supervisor/worker in LangGraph with checkpointing, memory, guardrails, eval
- [ ] dbt + Dataform project on the data platform with quality tests

## Portfolio Rules
- Every project: README, architecture diagram, demo (screenshot or video), deployed URL where possible
- Write one short post per project (teach to retain)
- Publish each on your GitHub profile — the repos are the resume