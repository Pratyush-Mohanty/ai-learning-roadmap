# AI / LLM / System Design / GCP — Advanced Learning Repository

A content-rich, **deduplicated** learning repo for a **Data Engineer (4 yrs)** moving into AI Engineering, System Design, and Google Cloud.

This is not a link list. Every file teaches the concepts **directly** — definitions, tradeoffs, architecture decisions, patterns, and gotchas — with a short "go deeper" link section at the end of each file.

## Repository Map

| File | What it covers | Level |
|---|---|---|
| [ROADMAP.md](ROADMAP.md) | 6-phase learning plan with weekly cadence and projects | All |
| [topics/01-system-design.md](topics/01-system-design.md) | Scalability, CAP, consistency, storage engines, caching, load balancing, tradeoff-driven design | Intermediate |
| [topics/02-ai-llm-fundamentals.md](topics/02-ai-llm-fundamentals.md) | Transformers, tokens, attention, pre-training, fine-tuning, RLHF, eval | Foundation |
| [topics/03-llm-engineering.md](topics/03-llm-engineering.md) | RAG pipeline anatomy, chunking, retrieval, reranking, LLMOps, cost control | Core |
| [topics/04-ai-agents-advanced.md](topics/04-ai-agents-advanced.md) | Agent loop, tool design, memory systems, multi-agent orchestration, guardrails, protocols (A2A/MCP), eval | Advanced |
| [topics/05-llm-optimization.md](topics/05-llm-optimization.md) | Continuous batching, PagedAttention, KV cache, quantization (FP8/INT4), speculative decoding, parallelism | Advanced |
| [topics/06-cloud-data-engineering-advanced.md](topics/06-cloud-data-engineering-advanced.md) | Cloud compute/networking/storage, data lakehouse, streaming, CDC, dbt, orchestration, governance | Advanced |
| [topics/07-gcp-data-engineering.md](topics/07-gcp-data-engineering.md) | BigQuery internals, BigLake + Iceberg, Dataflow/Beam, Pub/Sub, Composer, Bigtable vs Spanner, Vertex AI | Advanced |
| [topics/08-gcp-cloud-engineering.md](topics/08-gcp-cloud-engineering.md) | GCP compute, networking, IAM, Terraform, Cloud Run/GKE, cost management | Intermediate |
| [projects/](projects/) | Hands-on projects per phase | All |

## Core Principles

1. **One resource, one home.** Each concept appears once; cross-referenced by link.
2. **Learn → Build → Ship.** Every phase ends with a deployable project.
3. **Most leverage first.** Highest-impact techniques (e.g., continuous batching ≈ 23x throughput) before micro-optimizations.
4. **Understand tradeoffs, not just names.** The skill is choosing between options under constraints.