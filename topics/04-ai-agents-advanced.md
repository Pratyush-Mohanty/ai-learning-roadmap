# Advanced AI Agents - Comprehensive Study Guide

**Estimated time: 3-4 weeks.** The hardest and most in-demand area in 2026. Most agent projects fail on **coordination**, not model quality.

---

## 1. What Makes an Agent an Agent

```
User goal ──▶ Plan ──▶ [Reason ──▶ Call tool ──▶ Observe result] (repeat)
                                                          │
                                                  goal met? ──▶ Final answer
```

An agent = an LLM in a loop that **reasons → acts (tools) → observes → reasons again** until done.

**The key difference from a chatbot:** agency — the agent takes actions, and those actions have side effects in the real world (DB writes, emails, API calls).

---

## 2. The Core Components

### 2.1 Model
- Often **smaller specialized models per task** in 2026 (routing > one giant model)
- Cost routing: cheap model for classification/routing, frontier only for hard reasoning

### 2.2 Tools (THE critical component)
> **Tool design drives reliability more than model choice.** Poorly designed tools are the #1 source of agent failures.

Tool requirements:
- Clear, specific descriptions the model can understand
- Strict input/output schemas (validate at the boundary)
- Idempotent where possible (retries are safe)
- Human approval required for destructive operations

### 2.3 Memory (the difference between chatbot and agent)
| Type | What | Technology |
|---|---|---|
| Working | Context window | - |
| Short-term | Conversation + task state | Session store |
| Long-term | Persistent facts | Vector DB + knowledge graph (Mem0, Letta, Zep, pgvector) |

**Multi-scope memory:** tag each write with user_id / session_id / org_id; compose scopes at retrieval time.

**Memory benchmarks** (learn the names): LoCoMo, LongMemEval, BEAM — measure accuracy + token efficiency + latency, not just accuracy.

### 2.4 Planning
- **ReAct:** Reason → Act → Observe (interleaved)
- **Plan-and-Execute:** plan first, execute steps, re-plan on failure
- Re-planning is essential — the first plan is almost never right

### 2.5 Structured output
Every tool call must parse reliably. Use function calling / constrained decoding. Never free-text parse.

---

## 3. Orchestration Patterns (the 2026 taxonomy)

### The 8 canonical patterns

```
SEQUENTIAL:            A ──▶ B ──▶ C
PARALLEL:              A ──▶ B1, B2, B3 ──▶ merge
LOOP/CRITIC:           generate ──▶ critique ──▶ (loop until pass)
SUPERVISOR/WORKER:     supervisor routes to specialist workers ──▶ merge
HIERARCHICAL:          manager ──▶ leads ──▶ workers
PEER-TO-PEER:          agents collaborate directly (A2A)
MARKETPLACE:           dynamic discovery of agents by capability
HUMAN-IN/ON-LOOP:      approve each action (in) vs monitor+intervene (on)
```

### Deep dive on the most used:

**Supervisor/Worker** (production favorite)
```
User ──▶ Supervisor ──▶ Worker: research
                 ──▶ Worker: code
                 ──▶ Worker: write
                 ──▶ merges results, handles failures
```
- Supervisor owns the plan and quality bar
- Workers are focused, cheaper, replaceable

**Loop/Critic** (biggest quality jump)
- Generate → critique against a standard → regenerate until pass or max attempts
- Independent critique catches mistakes self-review misses
- Best for drafting, code generation, long-form writing

**Reflection** pattern: self-critique after completion; cheap, works surprisingly well.

---

## 4. Framework Selection (decide by need, not hype)

| Framework | Best for | When to use |
|---|---|---|
| LangGraph | Stateful, checkpointed, complex workflows | Production standard; most cases |
| CrewAI | Simple role teams | Fastest prototyping |
| AutoGen | Multi-agent deliberation, human-in-the-loop | Agent-to-agent conversation |
| OpenAI Agents SDK | Handoffs + guardrails as primitives | OpenAI-centric, minimal |
| Pydantic AI | Type-safe Python agents | Python-native teams |
| MetaGPT | SOP-driven software company | Research/advanced demos |

**Decision rule:** performance-critical + cost-sensitive → LangGraph. Need deliberation → AutoGen. Minimal + OpenAI → Agents SDK. Prototyping role teams → CrewAI.

---

## 5. Interoperability Protocols

- **MCP (Model Context Protocol)** — agent ↔ tools standard. Servers directory: https://github.com/modelcontextprotocol/servers
- **A2A (Agent2Agent)** — agent ↔ agent standard. https://github.com/google/A2A

Why it matters: agents need to call enterprise tools and other agents predictably. Standard protocols beat custom glue.

---

## 6. State, Durability, Reliability

### Checkpointing (non-negotiable)
Persist graph state at every step (LangGraph does this natively). A crash resumes, not restarts.

### Durable execution
Temporal-style: retries, sagas, compensation for long workflows. Mission-critical agent pipelines use two layers: orchestration (LangGraph) + durable execution (Temporal).

### Failure handling
- Circuit breakers on tool calls
- Retries with backoff
- Validate EVERY tool result before proceeding
- Human fallback for uncertain outcomes

### Non-determinism compounds
Each LLM call adds variance. Multi-agent multiplies it. Validate types and contracts at every handoff.

---

## 7. Guardrails & Governance (the #1 enterprise gap)

> 81% of AI agents are already in operation, only ~14% have full security approval.

The four-element guardrail framework:

```
PERMISSIONS (least privilege - what can the agent do?)
        │
APPROVAL (human gates - where must a human decide?)
        │
AUDIT TRAIL (full trace - what did it do and why?)
        │
KILL SWITCH (safe shutdown + rollback on anomaly)
```

### Design principles
- **Least privilege** for every tool/action (blast-radius control)
- **Progressive autonomy:** escalate decisions by risk level
- **Human-on-the-loop** for mature systems (monitor + intervene on anomalies)
- Guardrails designed IN, not bolted on after an incident

---

## 8. Evaluation & Cost Engineering

### Evaluating agents (not just the model)
- Trace-based evals of the whole loop: tool calls correct? state transitions valid? final answer grounded?
- Benchmarks: check agent benchmarks for your domain
- Multi-turn interactions (a single-turn test misses most agent failures)

### Cost engineering
```
per_task_cost = Σ (token cost of each LLM call + tool execution)
```
- Model tiering: routing/classification → cheap; reasoning → frontier
- Cache, shorten prompts, cap iterations
- **Full 3-year cost** includes staffing for prompt engineering + eval + maintenance — plan for it

### Monitoring
- Traces at agent boundaries (Langfuse, Arize)
- Error rates, decision paths, tool usage, ROI

---

## 9. Common Failure Modes (learn these)

| Failure | Symptom | Fix |
|---|---|---|
| Loop stuck | Agent repeats actions | Step limits + timeouts |
| Tool misuse | Wrong params, destructive ops | Strict schemas + validation + approval gates |
| Hallucination across hops | Errors compound | Grounding, citations, eval at each hop |
| Cost explosion | Token bill spikes | Budgets per agent, routing, caching |
| Prompt injection | Malicious tool/web content steers agent | Input sanitization, least-privilege tools, sandboxing |
| Non-determinism | Same input, different results | Contract/type validation at handoffs |

---

## 10. Three-Week Study Plan

**Week 1 — Single agents:**
- Days 1-2: Agent loop + tool design (LangGraph quickstart)
- Days 3-4: Tool calling, structured output, reflection
- Days 5-7: Memory integration (Mem0/pgvector) + checkpointing

**Week 2 — Multi-agent:**
- Days 8-9: Supervisor/worker in LangGraph
- Days 10-11: Loop/critic + hierarchical patterns
- Days 12-14: MCP tools + A2A, failure handling, circuit breakers

**Week 3 — Production:**
- Guardrails (permissions, approvals, audit, kill switch)
- Eval harness for the whole loop
- Cost tracking + model routing
- Build the flagship project: supervisor/worker system

---

## Go Deeper

- Patterns with code: https://github.com/balakumardev/ai-agent-design-patterns
- Azure orchestration patterns: https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/ai-agent-design-patterns
- Production guide: https://baeseokjae.github.io/posts/multi-agent-system-design-guide-2026/
- Research tracker (biweekly): https://github.com/masamasa59/ai-agent-papers
- Survey: https://arxiv.org/abs/2510.25445
- Cloud-native agents: https://github.com/panaversity/learn-agentic-ai
- Memory: https://github.com/mem0ai/mem0
- LangGraph: https://github.com/langchain-ai/langgraph