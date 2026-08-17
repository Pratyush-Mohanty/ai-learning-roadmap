# LLM Engineering — RAG, Apps, Infra, LLMOps

Your strongest lane: you already understand data, pipelines, and storage. This phase turns that into production LLM applications.

## Courses (primary)
- `github.com/DataTalksClub/llm-zoomcamp` — free 10-week bootcamp on production RAG over your own data. Do this first.
- `github.com/HandsOnLLM/Hands-On-Large-Language-Models` — O'Reilly book + notebooks: tokens, embeddings, fine-tuning, deployment

## Frameworks (pick one primary, one secondary)
- `langchain-ai/langchain` — chains, tools, retrieval (primary; most tutorials)
- `run-llama/llama_index` — RAG-specialized, simpler mental model for data engineers
- `langgenius/dify` — production LLM app platform (visual RAG + agents + LLMOps). Ships a real product fast
- `langflow-ai/langflow` — low-code prototyping of RAG/agent flows
- `infiniflow/ragflow` — RAG engine with deep document understanding (PDFs, tables)

## Local models (offline / cost control)
- `ollama/ollama` — local LLM runtime
- `open-webui/open-webui` — ChatGPT-style UI for local models
- `BerriAI/litellm` — one API for 100+ LLMs with routing/cost control

## Vector & data infrastructure
- `pgvector/pgvector` — Postgres extension (start here; you know SQL)
- `qdrant/qdrant` — fast vector search with filtering (at scale)
- `milvus-io/milvus` — large-scale, GPU-accelerated
- `supabase/supabase` — Postgres + auth + realtime + vectors (full backend for AI apps)
- `firecrawl/firecrawl` — web to LLM-ready data (RAG ingestion)

## LLMOps: evaluation, monitoring, observability
- `langfuse/langfuse` — traces, prompt versions, eval datasets, cost dashboards (add in production)
- `evidentlyai/evidently` — ML/LLM drift + evaluation
- `instructor-ai/instructor` — structured, typed output from LLMs (validation)
- `n8n-io/n8n` — open-source AI automation workflows (SaaS + LLM steps)

## RAG techniques (learn in order)
1. Basic RAG: embed, store, retrieve, generate
2. Chunking strategies + hybrid search (vector + keyword)
3. Re-ranking
4. Evaluation (retrieval quality + generation quality)
5. Agentic RAG (query decomposition, multi-hop)

## Production checklist
- Streaming responses (SSE), caching, rate limiting, retries
- Observability (Langfuse) from day one
- Cost tracking per request
- Eval before deploy