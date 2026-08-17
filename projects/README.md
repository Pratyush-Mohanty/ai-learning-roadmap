# Hands-on Projects

Every phase ends with a deployable project. One strong project per phase beats five half-finished ones. Full schedule: [ROADMAP.md](../ROADMAP.md).

## Weeks 1-2 — Cloud Foundations
- [ ] **Batch pipeline on GCP:** Cloud Storage -> BigQuery with Cloud Composer orchestration (free credits)
- [ ] Deploy a service on Cloud Run via Terraform, behind a global HTTPS load balancer

## Weeks 3-4 — AI/LLM Fundamentals
- [ ] **LLM API notebook:** structured JSON extraction, rate-limit/retry handling, cost logging
- [ ] Golden dataset: 50 Q&A pairs to reuse for evals in the RAG phase
- [ ] Optional: fine-tune a small model on your own dataset with LlamaFactory (https://github.com/hiyouga/LlamaFactory)

## Weeks 5-6 — LLM Engineering: RAG (flagship)
- [ ] **RAG over your own documents:** pgvector + LangChain/LlamaIndex, chunking experiments, hybrid search, reranking
- [ ] Add eval (RAGAS) + Langfuse tracing; document the quality before/after
- [ ] Deploy as FastAPI with streaming (SSE)
- [ ] Optional fast win: production chatbot on Dify (https://github.com/langgenius/dify)

## Weeks 7-8 — LLM Engineering: Agents + Serving
- [ ] **Supervisor/worker multi-agent system:** LangGraph, checkpointing, memory, guardrails, eval
- [ ] **vLLM serving:** serve a model, benchmark TTFT/throughput, apply FP8 + prefix caching + speculative decoding, measure before/after

## Weeks 9-11 — GCP Data Engineering
- [ ] **Streaming pipeline:** Pub/Sub -> Dataflow -> BigQuery with windows/watermarks, IAM, cost monitoring
- [ ] **Lakehouse:** Iceberg table via BigLake, ingested with Storage Write API, queried by Spark + BigQuery
- [ ] **Gemini in BigQuery demo** — run an LLM inside the warehouse (your differentiator)
- [ ] dbt + Dataform project with data-quality tests

## Week 12 — Advanced + Portfolio
- [ ] Finish vLLM/SGLang deep dive (speculative decoding, disaggregation notes)
- [ ] Portfolio: every project gets README + architecture diagram + demo
- [ ] Write one short post per flagship project; publish repos to your GitHub profile

## Portfolio Rules
- Every project: README, architecture diagram, demo (screenshot or video), deployed URL where possible
- Write one short post per project (teach to retain)
- Publish each on your GitHub profile — the repos are the resume