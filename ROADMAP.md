# Roadmap — 6 Phases

Order matters: foundation → building → advanced. Each phase is 2-4 weeks and ends with a deployable project.

## Phase 0 — Cloud Foundations (2-3 weeks)
**Goal:** get hands-on with GCP before anything else. Free credits make this low-risk.
- [ ] Work through [08-gcp-cloud-engineering.md](topics/08-gcp-cloud-engineering.md) concepts
- [ ] Google Cloud Skills Boost: Cloud Engineer path (free credits included)
- [ ] Take the **Associate Cloud Engineer** exam
- **Project:** Batch pipeline Cloud Storage → BigQuery, orchestrated with Cloud Composer

## Phase 1 — AI/LLM Fundamentals (3-4 weeks)
**Goal:** understand how LLMs work so later decisions make sense.
- [ ] [02-ai-llm-fundamentals.md](topics/02-ai-llm-fundamentals.md) — transformers, tokens, RLHF, eval
- [ ] DeepLearning.AI *LLM Engineering for Everyone* (free, ~1 hr) → https://www.deeplearning.ai/short-courses/llm-engineering-for-everyone/
- [ ] Optional math: Karpathy *Zero to Hero* → https://www.youtube.com/playlist?list=PLAqhIrjkxbuWI23v9cThsA9GvCAUhRvKZ
- **Project:** Notebook that calls an LLM API, extracts structured JSON, handles rate limits, logs cost

## Phase 2 — System Design (4-6 weeks, parallel)
**Goal:** distributed systems reasoning; your data background makes this fast.
- [ ] [01-system-design.md](topics/01-system-design.md)
- [ ] Read *Designing Data-Intensive Applications* (Kleppmann)
- [ ] Whiteboard 10-12 classic designs out loud
- **Project:** Redesign one of your real past pipelines at 10x scale; explain the tradeoffs

## Phase 3 — LLM Engineering (4-6 weeks, highest priority)
**Goal:** ship production LLM apps. This is the skill that gets hired.
- [ ] [03-llm-engineering.md](topics/03-llm-engineering.md)
- [ ] `DataTalksClub/llm-zoomcamp` (free 10-week bootcamp) → https://github.com/DataTalksClub/llm-zoomcamp
- [ ] Stack: LangChain → pgvector → Langfuse → deploy with FastAPI
- **Project:** RAG app over your documents with eval + monitoring

## Phase 4 — GCP Data Engineering (4-6 weeks)
**Goal:** Professional Data Engineer cert. Your existing data skills become certified.
- [ ] [07-gcp-data-engineering.md](topics/07-gcp-data-engineering.md)
- [ ] Official exam guide → Coursera cert → Skills Boost practice → 2 timed mocks
- **Project:** Streaming pipeline Pub/Sub → Dataflow → BigQuery with IAM + cost optimization

## Phase 5 — Advanced (ongoing)
**Goal:** differentiate on the hard problems: serving efficiency and multi-agent systems.
- [ ] [05-llm-optimization.md](topics/05-llm-optimization.md) — vLLM, quantization, speculative decoding
- [ ] [04-ai-agents-advanced.md](topics/04-ai-agents-advanced.md) — LangGraph, memory, guardrails
- [ ] [06-cloud-data-engineering-advanced.md](topics/06-cloud-data-engineering-advanced.md) — lakehouse, CDC, streaming patterns
- **Project:** Serve an LLM with vLLM and measure quantization + spec-decoding gains; then build a supervisor/worker multi-agent system

## Principles
1. Consume less, build more
2. Practice out loud (system design especially)
3. Do mock exams/interviews — one struggle session > 10 hours of reading
4. Start simple — single-agent RAG beats a fragile multi-agent system 9 times out of 10