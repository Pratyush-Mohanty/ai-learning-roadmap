# Roadmap

Structured by phase. Each phase = 2-4 weeks. Order matters: foundation → building → advanced.

## Phase Overview

| Phase | Focus | Duration | Resources | Outcome |
|---|---|---|---|---|
| 0 | GCP Cloud Engineer | 2-3 weeks | `resources/07-gcp-cloud-engineering.md` | Deploy a pipeline on GCP |
| 1 | AI/LLM theory | 3-4 weeks | `resources/02-ai-llm-fundamentals.md` | Notebook: LLM API + structured extraction |
| 2 | System Design | 4-6 weeks (parallel) | `resources/01-system-design.md` | Whiteboard 10 designs out loud |
| 3 | LLM Engineering | 4-6 weeks | `resources/03-llm-engineering.md` | Production RAG app on your data |
| 4 | GCP Data Engineer | 4-6 weeks | `resources/06-gcp-data-engineering.md` | Streaming + batch pipeline |
| 5 | Advanced (elective) | ongoing | `resources/04-llm-optimization.md`, `resources/05-agent-architectures.md` | vLLM serving + multi-agent system |

## Phase 0 — Cloud Foundations (2-3 weeks)
- Complete Google Cloud Skills Boost **Cloud Engineer path** (free credits included)
- Take the Associate Cloud Engineer exam
- **Project:** Batch pipeline (Cloud Storage → BigQuery) with Cloud Composer orchestration

## Phase 1 — AI/LLM Fundamentals (3-4 weeks)
- DeepLearning.AI *LLM Engineering for Everyone* (free, ~1 hr — start here)
- *Generative AI with LLMs* (Coursera, DeepLearning.AI + AWS)
- Prompt engineering basics (learn once, then move on)
- Optional math: Karpathy *Zero to Hero*
- **Project:** Python notebook calling an LLM API, structured extraction, rate-limit handling

## Phase 2 — System Design (4-6 weeks, parallel with other phases)
- Read *Designing Data-Intensive Applications* (your data background = fast read)
- `system-design-primer` as reference, not cover-to-cover
- Practice 10-12 classic designs out loud
- **Project:** Redesign one of your past data pipelines at 10x scale

## Phase 3 — LLM Engineering (4-6 weeks, highest priority)
- `DataTalksClub/llm-zoomcamp` — 10-week free bootcamp on production RAG
- `HandsOnLLM/Hands-On-Large-Language-Models` — tokens, embeddings, fine-tuning, deployment
- Stack: LangChain → pgvector → Langfuse → Dify
- **Project:** RAG app over your documents with eval + monitoring

## Phase 4 — GCP Data Engineering (4-6 weeks)
- Official exam guide → Coursera GCP Data Engineering cert → Skills Boost practice + 2 timed mocks
- Master: BigQuery, Dataflow, Pub/Sub, Dataproc, Composer, Bigtable/Firestore/Spanner, IAM
- **Project:** Streaming pipeline (Pub/Sub → Dataflow → BigQuery) with cost optimization

## Phase 5 — Advanced (ongoing)
- LLM optimization: vLLM, quantization, speculative decoding
- Agent architectures: LangGraph, design patterns
- Follow `masamasa59/ai-agent-papers` biweekly
- **Project:** Serve an LLM with vLLM at scale; then a supervisor/worker multi-agent system

## Key Principles
1. Consume less, build more — every phase ends with a deployable project
2. Practice out loud — especially for system design
3. Do mocks — one struggle session is worth 10 hours of reading
4. Start simple — single-agent RAG beats a fragile multi-agent system 9 times out of 10