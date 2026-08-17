# LLM Engineering - RAG, Apps, LLMOps (Comprehensive)

**Estimated time: 4-6 weeks (your flagship phase).** Turn an LLM into a reliable product. Your data background is a superpower here.

---

## 1. The RAG Pipeline, End to End

```
INGESTION (offline, batch):
  Documents ──▶ Clean ──▶ Chunk ──▶ Embed ──▶ Store in vector DB (+ metadata)

QUERY (online, per request):
  User query ──▶ Embed ──▶ Retrieve top-k ──▶ Rerank ──▶ Augment prompt ──▶ LLM ──▶ Answer + citations
```

---

## 2. The Parts, and How to Get Each Right

### 2.1 Chunking (the most underestimated step)
- Split on **semantic boundaries** (paragraphs, sections), not fixed char counts.
- Overlap chunks 10-20% so context isn't cut mid-sentence.
- Preserve structure: headers, tables, code blocks. Table-aware chunkers exist (LlamaIndex, RAGFlow).
- Store **metadata** per chunk (source, date, section, doc ID) — enables filtering and citations.

### 2.2 Embeddings
- Embed what will be **searched** (the chunk), not the whole document.
- Model choice: open (BGE, nomic-embed) vs commercial (OpenAI, Cohere, Voyage). Tradeoff: quality vs dimension vs cost.
- **Re-embed when chunking changes** — stale vectors silently break retrieval.

### 2.3 Vector database (start with what you know)
| DB | When |
|---|---|
| pgvector | Start here. Postgres + vectors. Up to ~10M vectors. |
| Qdrant | Fast + filtering, scale-up path |
| Milvus | Very large scale, GPU-accelerated |
| Weaviate | Hybrid search, multimodal |

### 2.4 Hybrid search (the quality unlock)
Pure vector misses typos, exact IDs, rare terms. Combine:
```
score = rank_fusion(vector_score, BM25_keyword_score)
```
Reciprocal rank fusion merges the two rankings. Do this from day one — it's cheap and fixes most retrieval gaps.

### 2.5 Retrieval + reranking
- Retrieve MORE than you feed: top-30.
- **Rerank** to top-3/5 with a cross-encoder/reranker — much better precision than raw cosine.

### 2.6 The augmented prompt
```
System: "Answer using ONLY the context below. Cite the source for each claim.
        If the context doesn't answer, say 'I don't know'. Do not invent facts."
User: [retrieved context chunks with metadata]
      [original question]
```

### 2.7 Evaluation loop
- Retrieval quality: hit rate, MRR (does the right chunk appear in top-k?)
- Generation quality: faithfulness, answer relevance (RAGAS)
- Run before/after every chunking/embedding/prompt change

---

## 3. The Decision: RAG vs Fine-tuning vs Prompting

| Need | Use |
|---|---|
| New/fresh/private facts | RAG |
| Style/format/domain behavior | Fine-tuning |
| Quick small tweak | Prompting |
| Both facts AND style | RAG + Fine-tune |

RAG first, always. Fine-tune only when justified.

---

## 4. Agentic RAG (level up)

```
              ┌─────────────────────────────┐
              │          Query             │
              └─────────────┬───────────────┘
                            ▼
                   Route / decompose
                    ┌──────┴──────┐
               simple│            │complex
                    ▼            ▼
          Single-shot     Multi-hop: decompose
          retrieval        into sub-queries
                    │            │
                    └──────┬─────┘
                           ▼
                  Combine + critique
                           │
                           ▼
               Generate grounded answer
```

1. **Query rewriting** — expand/rewrite the user query for better retrieval
2. **Multi-hop** — decompose into sub-questions, answer iteratively
3. **Corrective RAG** — if retrieval quality is low, rewrite or fall back
4. **Self-reflection** — critique the answer, re-retrieve if needed

This is the bridge between plain RAG and full agents (see topic 04).

---

## 5. LLMOps - The Production Layer

### Observability (add from day one)
- Trace every call: input, output, tokens, latency, cost, retrieved chunks
- Langfuse: https://github.com/langfuse/langfuse (open source, self-hostable)
- You cannot debug what you cannot see

### Evaluation
- Golden dataset + RAGAS: https://github.com/explodinggradients/ragas
- CI-style: run evals on every change

### Cost control
- Log tokens per request
- **Model routing:** cheap model for easy queries, frontier for hard (LiteLLM)
- Cache repeated/exact queries
- Limit output tokens

### Guardrails
- Input: PII, prompt injection detection
- Output: unsafe content, hallucination risk
- Tools: NeMo Guardrails, Llama Guard

### Structured output
- Instructor for typed extraction: https://github.com/instructor-ai/instructor

---

## 6. Deployment Stack

```
App ──▶ FastAPI (+ SSE streaming) ──▶ Model router ──▶ Cheap model / Frontier model
                                          │
                                          └──▶ Vector DB + Langfuse tracing
```

| Layer | Choice |
|---|---|
| API | FastAPI + Pydantic, stream responses (SSE) |
| Inference | Hosted API (start) → vLLM self-host (scale) → Ollama (local/prototype) |
| Orchestration | LangChain (chains/tools), LlamaIndex (RAG), LangGraph (stateful) |
| No-code fast path | Dify — visual RAG + agents + LLMOps |
| Deploy | Docker + Cloud Run / GKE / a VPS |

---

## 7. Your Data-Engineer Advantage (lean into this)

| Data skill | Applies to LLM engineering as |
|---|---|
| Pipelines | Ingest/embed pipelines (Dataflow, Airflow, dbt) |
| Warehouse | BigQuery/Postgres as structured context source |
| CDC | Keep vector store fresh when source changes |
| Data quality | Eval + guardrails = data quality for LLM apps |
| Cost optimization | Token/cost tracking, model routing |

**This is your differentiator.** Most LLM engineers don't understand data pipelines. You do.

---

## 8. Production Checklist (final review before shipping)

- [ ] Chunking experiments documented (chunk size, overlap, boundaries)
- [ ] Hybrid search (vector + BM25) enabled
- [ ] Reranker in place
- [ ] Eval harness (RAGAS) with golden set, before/after baselines
- [ ] Langfuse tracing on every request
- [ ] Streaming responses (SSE)
- [ ] Retries + exponential backoff on LLM calls
- [ ] Rate limiting + token budget per request
- [ ] Cost logging per request
- [ ] Guardrails on input/output (PII, injection)
- [ ] Citations in answers with metadata
- [ ] Fallback answer for low-confidence/no-context

---

## 9. Four-Week Study Plan

**Week 1 — RAG fundamentals:**
- Days 1-2: Build basic RAG with pgvector + LangChain/LlamaIndex
- Days 3-4: Chunking experiments + hybrid search
- Days 5-7: Reranking + eval (RAGAS)

**Week 2 — Production hardening:**
- Add Langfuse tracing, cost logging, retries, streaming
- Deploy as FastAPI with SSE
- Set up eval harness with a golden dataset

**Week 3 — Agentic RAG + scale:**
- Query rewriting, multi-hop decomposition
- Corrective RAG, self-reflection
- Scale: vector index tuning, caching

**Week 4 — Polish + ship:**
- Guardrails, fallback paths
- Package with Docker, deploy to Cloud Run
- Write the project post

---

## Go Deeper

- Bootcamp: DataTalksClub llm-zoomcamp - https://github.com/DataTalksClub/llm-zoomcamp
- Book + code: Hands-On Large Language Models - https://github.com/HandsOnLLM/Hands-On-Large-Language-Models
- LangChain: https://github.com/langchain-ai/langchain
- LlamaIndex: https://github.com/run-llama/llama_index
- Langfuse: https://github.com/langfuse/langfuse
- RAGAS: https://github.com/explodinggradients/ragas
- Runnable examples: https://github.com/Shubhamsaboo/awesome-llm-apps
- Dify: https://github.com/langgenius/dify
- pgvector: https://github.com/pgvector/pgvector