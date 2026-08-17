# LLM Engineering — RAG, Apps, LLMOps

The core skill: turning an LLM into a reliable product. This is where your data engineering background is a superpower.

## 1. RAG Pipeline Anatomy
```
Documents → Ingest → Chunk → Embed → Vector DB
Query → Embed query → Retrieve (top-k) → Rerank → Augment prompt → LLM → Answer + citations
```

### The parts and how to get each right

**Ingestion & chunking (most underestimated step)**
- Split documents on semantic boundaries, not fixed char counts. Overlap chunks (~10-20%) so context isn't cut mid-sentence.
- Preserve structure: headers, tables, code blocks. Use document-aware chunkers (LlamaIndex has good ones; RAGFlow specializes in PDFs/tables).
- Store **metadata** (source, date, section) per chunk — enables filtering and citations.

**Embeddings**
- Embed the chunk that will be *searched*, not necessarily the whole document.
- Model choice: open (BGE, nomic-embed) vs commercial (OpenAI, Cohere, Voyage). Dimension vs quality vs cost tradeoff.
- Recompute embeddings when the chunking changes — stale vectors silently break retrieval.

**Vector database (start here if you know SQL)**
- `pgvector` (Postgres extension): start here — you already know the DB. Good to ~10M vectors.
- Scale-up: Qdrant (fast + filtering), Milvus (huge scale), Weaviate (hybrid + multimodal).
- **Hybrid search beats pure vector** for most real data: combine vector (semantic) + BM25 (keyword) via reciprocal rank fusion. Typos, IDs, exact terms need lexical search.

**Retrieval & reranking**
- Retrieve more than you feed (top-30), then **rerank** to top-3/5 with a cross-encoder/reranker (better precision than raw cosine).
- Query rewriting: expand, decompose multi-part queries (agentic RAG).

**Augmented prompt**
- Insert retrieved context + instructions: *"Answer using ONLY the context. Cite source. If not present, say you don't know."*
- Cite with metadata → trust + auditability.

## 2. When RAG vs Fine-tuning vs Prompting
| Need | Use |
|---|---|
| New/fresh/private facts | RAG |
| Style/format/domain behavior | Fine-tuning |
| Small tweak, quick | Prompting |
| Both facts AND style | RAG + fine-tune (separate concerns) |

RAG first, always. Fine-tune only when it's justified by quality/cost.

## 3. LLMOps — the production layer
- **Observability:** traces of every call (input, output, tokens, latency, cost, retrieved chunks). Langfuse is the open-source standard — https://github.com/langfuse/langfuse
- **Evaluation:** golden dataset + RAGAS (retrieval + generation metrics) — https://github.com/explodinggradients/ragas. Run on every prompt/chunking/model change.
- **Cost control:** per-request token logging, model routing (cheap model for easy queries), caching exact/repeated queries.
- **Guardrails:** input/output filters (PII, prompt injection, unsafe content) — NeMo Guardrails (https://github.com/NVIDIA/NeMo-Guardrails) or Llama Guard.
- **Structured output:** Instructor for typed extraction — https://github.com/instructor-ai/instructor

## 4. Deployment Stack
- **API:** FastAPI + Pydantic. Stream responses (SSE) for UX.
- **Inference:** hosted API (OpenAI/Anthropic/Gemini) for most; self-host with vLLM for cost/control at scale (→ [05-llm-optimization.md](05-llm-optimization.md)); local with Ollama for prototyping/offline.
- **Orchestration:** LangChain (chains/tools/retrieval) → https://github.com/langchain-ai/langchain · LlamaIndex (RAG-specialized) → https://github.com/run-llama/llama_index. LangGraph when you need stateful/agentic flows (→ [04-ai-agents-advanced.md](04-ai-agents-advanced.md)).
- **No-code fast path:** Dify (visual RAG + agents + LLMOps) → https://github.com/langgenius/dify

## 5. The Data-Engineer Advantage
Your existing skills map directly:
- **Pipelines →** ingest/embed pipelines (Dataflow, Airflow, dbt for embeddings/vector tables)
- **Warehouse →** BigQuery/Postgres as the source of truth for structured context
- **CDC →** keep the vector store fresh when source data changes
- **Data quality →** eval + guardrails IS data quality for LLM apps

## Go Deeper
- **Bootcamp:** DataTalksClub/llm-zoomcamp (free, 10 weeks) → https://github.com/DataTalksClub/llm-zoomcamp
- **Book+code:** Hands-On Large Language Models → https://github.com/HandsOnLLM/Hands-On-Large-Language-Models
- **Framework:** LangChain → https://github.com/langchain-ai/langchain
- **Framework:** LlamaIndex → https://github.com/run-llama/llama_index
- **Vector:** pgvector → https://github.com/pgvector/pgvector · Qdrant → https://github.com/qdrant/qdrant
- **Observability:** Langfuse → https://github.com/langfuse/langfuse
- **Eval:** RAGAS → https://github.com/explodinggradients/ragas
- **Runnable examples:** Shubhamsaboo/awesome-llm-apps → https://github.com/Shubhamsaboo/awesome-llm-apps