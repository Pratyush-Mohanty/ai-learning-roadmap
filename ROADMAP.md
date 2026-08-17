# Roadmap

Structured by phase. Each phase = 2-4 weeks. Order matters: foundation → building → advanced.

| Phase | Focus | Resources | Outcome (project) |
|---|---|---|---|
| **0. Cloud Foundations** | GCP Associate Cloud Engineer | [07-gcp-cloud-engineering.md](resources/07-gcp-cloud-engineering.md) | Deploy a data pipeline on GCP |
| **1. AI/LLM Foundations** | LLM theory, prompt engineering | [02-ai-llm-fundamentals.md](resources/02-ai-llm-fundamentals.md) | Notebook: build a mini-LLM / call LLM APIs |
| **2. System Design** | Distributed systems fundamentals | [01-system-design.md](resources/01-system-design.md) | Design a system, whiteboard it out loud |
| **3. LLM Engineering** | RAG, agents, LLMOps, deployment | [03-llm-engineering.md](resources/03-llm-engineering.md) | Production RAG app on your own data |
| **4. GCP Data Engineering** | Professional Data Engineer cert | [06-gcp-data-engineering.md](resources/06-gcp-data-engineering.md) | Streaming + batch pipeline on GCP |
| **5. Advanced (elective, ongoing)** | LLM optimization, agent architectures | [04-llm-optimization.md](resources/04-llm-optimization.md), [05-agent-architectures.md](resources/05-agent-architectures.md) | Serve an LLM at scale / multi-agent system |

## Detailed Phase Plan

### Phase 0 — Cloud Foundations (2-3 weeks)
- Complete Google Cloud Skills Boost **Cloud Engineer path** (free credits included)
- Take the Associate Cloud Engineer exam
- **Project:** Deploy a batch data pipeline (Cloud Storage → BigQuery) with Cloud Composer orchestration

### Phase 1 — AI/LLM Foundations (3-4 weeks)
- DeepLearning.AI *LLM Engineering for Everyone* (free, ~1hr — start here)
- *Generative AI with LLMs* (Coursera, DeepLearning.AI + AWS)
- Prompt engineering basics (learn once, then move on)
- Optional math: Karpathy *Zero to Hero* or 3Blue1Brown
- **Project:** Build a Python notebook that calls an LLM API, does structured extraction, and handles rate limits

### Phase 2 — System Design (4-6 weeks, parallel with others)
- Read *Designing Data-Intensive Applications* (your data background = fast read)
- `system-design-primer` as reference, not cover-to-cover
- Practice 10-12 classic designs (URL shortener, rate limiter, etc.) explaining out loud
- **Project:** Design a system for your own past data pipeline at scale; record yourself explaining it

### Phase 3 — LLM Engineering (4-6 weeks, highest priority)
- `DataTalksClub/llm-zoomcamp` — 10-week free bootcamp on production RAG
- `HandsOnLLM/Hands-On-Large-Language-Models` — tokens, embeddings, fine-tuning, deployment
- Build with: LangChain/LlamaIndex → pgvector → Langfuse (observability) → Dify (production platform)
- **Project:** RAG app over your own documents with eval + monitoring

### Phase 4 — GCP Data Engineering (4-6 weeks)
- Official exam guide → Coursera GCP Data Engineering cert → Skills Boost practice + 2 timed mocks
- Master: BigQuery, Dataflow, Pub/Sub, Dataproc, Composer, Bigtable vs Firestore vs Spanner, IAM
- **Project:** Streaming pipeline (Pub/Sub → Dataflow → BigQuery) with monitoring and cost optimization

### Phase 5 — Advanced (ongoing, after phases 1-4)
- LLM optimization: vLLM, quantization, speculative decoding (see 04)
- Agent architectures: LangGraph, design patterns (see 05)
- Follow `ai-agent-papers` biweekly for research
- **Project:** Serve a model with vLLM at scale, then build a supervisor/worker multi-agent system

## Key Principles
1. **Consume less, build more** — every phase ends with a deployable project
2. **Practice out loud** — especially for system design
3. **Do mocks** — one struggle session is worth 10 hours of reading
4. **Start simple** — single-agent RAG beats a fragile multi-agent system 9 times out of 10