# LLM Engineering — RAG, Apps, Infra, LLMOps

Your strongest lane: you already understand data, pipelines, and storage. This phase turns that into production LLM applications.

## Courses (primary)
- **LLM Zoomcamp** — https://github.com/DataTalksClub/llm-zoomcamp
  Free 10-week bootcamp on production RAG over your own data. Do this first.
- **Hands-On Large Language Models** — https://github.com/HandsOnLLM/Hands-On-Large-Language-Models
  O'Reilly book + notebooks: tokens, embeddings, fine-tuning, deployment.

## Frameworks (pick one primary, one secondary)
- **LangChain** — https://github.com/langchain-ai/langchain
  Chains, tools, retrieval. Most tutorials and documentation. Primary choice.
- **LlamaIndex** — https://github.com/run-llama/llama_index
  RAG-specialized; simpler mental model for data engineers.
- **Dify** — https://github.com/langgenius/dify
  Production LLM app platform (visual RAG + agents + LLMOps). Ships a real product fast.
- **Langflow** — https://github.com/langflow-ai/langflow
  Low-code prototyping of RAG/agent flows.
- **RAGFlow** — https://github.com/infiniflow/ragflow
  RAG engine with deep document understanding (PDFs, tables).

## Local Models (offline / cost control)
- **Ollama** — https://github.com/ollama/ollama
  Local LLM runtime.
- **Open WebUI** — https://github.com/open-webui/open-webui
  ChatGPT-style UI for local models.
- **LiteLLM** — https://github.com/BerriAI/litellm
  One API for 100+ LLMs with routing/cost control.

## Vector & Data Infrastructure
- **pgvector** — https://github.com/pgvector/pgvector
  Postgres extension. Start here; you know SQL.
- **Qdrant** — https://github.com/qdrant/qdrant
  Fast vector search with filtering. Scale-up path.
- **Milvus** — https://github.com/milvus-io/milvus
  Large-scale, GPU-accelerated vector DB.
- **Supabase** — https://github.com/supabase/supabase
  Postgres + auth + realtime + vectors. Full backend for AI apps.
- **Firecrawl** — https://github.com/firecrawl/firecrawl
  Web to LLM-ready data. RAG ingestion.

## LLMOps: Evaluation, Monitoring, Observability
- **Langfuse** — https://github.com/langfuse/langfuse
  Traces, prompt versions, eval datasets, cost dashboards. Add in production from day one.
- **Evidently AI** — https://github.com/evidentlyai/evidently
  ML/LLM drift + evaluation.
- **Instructor** — https://github.com/instructor-ai/instructor
  Structured, typed output from LLMs (validation).
- **n8n** — https://github.com/n8n-io/n8n
  Open-source AI automation workflows (SaaS + LLM steps).

## RAG Techniques (learn in order)
1. Basic RAG: embed → store → retrieve → generate
2. Chunking strategies + hybrid search (vector + keyword)
3. Re-ranking
4. Evaluation (retrieval quality + generation quality)
5. Agentic RAG (query decomposition, multi-hop)

## Production Checklist
- Streaming responses (SSE), caching, rate limiting, retries
- Observability (Langfuse) from day one
- Cost tracking per request
- Eval before deploy