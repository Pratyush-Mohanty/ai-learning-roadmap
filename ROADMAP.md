# Roadmap - 3-Month / 12-Week Plan

Order matters: foundation -> build -> advanced. Every week has concrete output. Total ~12 weeks, roughly 10-15 focused hours per week.

```
W1-2  System Design + Cloud Foundations  (parallel, foundations)
W3-4  AI / LLM Fundamentals
W5-8  LLM Engineering  (flagship - RAG + agents)
W9-11 GCP Data Engineering  (cert prep)
W12   Advanced + polish  (vLLM, agents, portfolio)
```

---

## Weeks 1-2 — System Design + Cloud Foundations (parallel)

### System Design
- [topics/01-system-design.md](topics/01-system-design.md) — the full 3-week guide; compress to 2 weeks
- Read: scalability ladder, CAP, storage engines, caching, queues, consistency, availability
- Whiteboard 4 designs out loud (URL shortener, rate limiter, chat, news feed)
- Read DDIA chapters on replication + partitions

### Cloud Foundations
- [topics/08-gcp-cloud-engineering.md](topics/08-gcp-cloud-engineering.md) — compute/networking/storage/IAM decision trees
- Terraform basics: modules, state, plan/apply
- Skills Boost Cloud Engineer path (free credits): https://www.cloudskillsboost.google/paths/11

### Output this week
- [ ] 4 whiteboarded designs, recorded
- [ ] Terraform module deploying a service to Cloud Run behind a global LB
- [ ] **Project:** batch pipeline Cloud Storage -> BigQuery via Composer

---

## Weeks 3-4 — AI / LLM Fundamentals

- [topics/02-ai-llm-fundamentals.md](topics/02-ai-llm-fundamentals.md) — tokens, attention, training phases, inference concepts, eval, cost math
- DeepLearning.AI LLM Engineering for Everyone (free): https://www.deeplearning.ai/short-courses/llm-engineering-for-everyone/
- Optional math: Karpathy Zero to Hero: https://www.youtube.com/playlist?list=PLAqhIrjkxbuWI23v9cThsA9GvCAUhRvKZ
- Build your prompting + structured-output muscle: Instructor (https://github.com/instructor-ai/instructor)

### Output this week
- [ ] **Project:** notebook calling an LLM API — structured JSON extraction, retries, rate limits, cost logging
- [ ] Golden dataset of 50 Q&A pairs you'll reuse for evals in Week 6

---

## Weeks 5-6 — LLM Engineering: RAG

- [topics/03-llm-engineering.md](topics/03-llm-engineering.md) — the full 4-week guide; you have 2 weeks
- Build RAG over your own documents: pgvector + LangChain/LlamaIndex
- Chunking experiments (size, overlap, boundaries) — document the results
- Hybrid search (vector + BM25), reranking, augmented prompt, citations
- Add eval (RAGAS) + Langfuse tracing; record before/after baselines
- Deploy as FastAPI with streaming (SSE)

### Output this week
- [ ] **Flagship project v1:** deployed RAG app with eval + monitoring + citations

---

## Weeks 7-8 — LLM Engineering: Agents + Advanced

- [topics/04-ai-agents-advanced.md](topics/04-ai-agents-advanced.md) — agent loop, tool design, memory, orchestration patterns, guardrails, MCP/A2A
- [topics/05-llm-optimization.md](topics/05-llm-optimization.md) — vLLM serving, quantization, prefix caching, speculative decoding
- Build supervisor/worker system in LangGraph with checkpointing + memory
- Serve the RAG app's model with vLLM; measure TTFT/throughput before/after optimization

### Output this week
- [ ] **Flagship project v2:** supervisor/worker agent system with guardrails + eval
- [ ] vLLM benchmark notes (FP8, prefix cache, spec decode measured)

---

## Weeks 9-11 — GCP Data Engineering (Professional Data Engineer cert)

- [topics/07-gcp-data-engineering.md](topics/07-gcp-data-engineering.md) — BigQuery internals, BigLake + Iceberg, Dataflow, Pub/Sub, Composer vs Dataform, security
- [topics/06-cloud-data-engineering-advanced.md](topics/06-cloud-data-engineering-advanced.md) — lakehouse, batch vs streaming, CDC, data quality
- Official exam guide: https://cloud.google.com/learn/certification/data-engineer
- Skills Boost practice labs: https://www.cloudskillsboost.google/paths/16
- Coursera cert (optional, weeknights): https://www.coursera.org/professional-certificates/gcp-data-engineering
- 2 timed mock exams (Whizlabs) in week 11

### Output this week
- [ ] **Project:** streaming pipeline Pub/Sub -> Dataflow -> BigQuery with windows/watermarks + IAM + cost monitoring
- [ ] **Project:** BigLake Iceberg lakehouse ingested via Storage Write API, queried by Spark + BigQuery
- [ ] Gemini in BigQuery demo (LLM inside the warehouse)
- [ ] Pass the Professional Data Engineer exam (or be booked)

---

## Week 12 — Advanced + Portfolio Polish

- [topics/06-cloud-data-engineering-advanced.md](topics/06-cloud-data-engineering-advanced.md) — dbt, CDC, data contracts
- [topics/05-llm-optimization.md](topics/05-llm-optimization.md) — finish vLLM / SGLang deep dive
- Optional: schedule Professional Cloud Architect (another 4-5 weeks after this plan)

### Output this week
- [ ] Portfolio: every project has README + architecture diagram + demo
- [ ] Write one post per flagship project (teach to retain)
- [ ] Repos published to your GitHub profile

---

## If You Need to Cut (keep these)

1. **Weeks 5-6 RAG project** — the single highest-value deliverable
2. **Weeks 7-8 agent project** — the differentiator
3. **Professional Data Engineer cert** — validates your existing data skills
4. **Cloud foundations project** — cheap but keeps GCP skills sharp

## Principles

1. Consume less, build more
2. Practice out loud (system design especially)
3. Do mock exams/interviews - one struggle session > 10 hours of reading
4. Start simple - single-agent RAG beats a fragile multi-agent system 9 times out of 10