# Agent Architectures (Advanced)

How to design reliable multi-agent systems — the hardest and most in-demand area in 2026.

## Framework: one primary
- `langchain-ai/langgraph` — stateful, checkpointed agent graphs. The production standard. Learn this first.

## Reference patterns (the 2026 taxonomy)
**Google's 8 patterns:** sequential, parallel, loop/critic, hierarchical, supervisor/worker, peer-to-peer, marketplace, human-in-the-loop (in / on the loop)
- Azure Architecture Center: **AI Agent Orchestration Patterns** — sequential, concurrent, group chat, handoff, magentic
- `baeseokjae.github.io` multi-agent design guide — state management, fault tolerance, cost engineering, observability

## GitHub (learn by example)
- `microsoft/autogen` — multi-agent conversations with human-in-the-loop
- `crewAIInc/crewAI` — role-based agent teams (simple)
- `FoundationAgents/MetaGPT` — SOP-driven software company simulation (advanced)
- `balakumardev/ai-agent-design-patterns` — 8 production patterns with runnable LangGraph code
- `panaversity/learn-agentic-ai` — cloud-native agentic AI on Kubernetes + Dapr
- `google/A2A` — agent-to-agent protocol (interop)
- `modelcontextprotocol/servers` — MCP: agent-to-tools standard
- `masamasa59/ai-agent-papers` — biweekly curated agent research papers (stay current)

## Research (depth)
- Survey: *Agentic AI: A Comprehensive Survey of Architectures* (arXiv 2510.25445)
- IEEE: *AI Agents: Review of Evolution, Architectures, Applications*

## Key production concepts
- **Tool design** drives reliability more than model choice — bad tools = agent failures
- Memory systems (short-term, long-term, vector + knowledge graphs)
- Cost-aware model routing (cheap model for simple tasks, frontier for hard)
- Observability (Langfuse, Arize) — you can't debug what you can't see
- Checkpointing / failure recovery / circuit breakers / human-in-the-loop
- Token economics — model tiering at scale

## Rules
- Start with a single reflection agent before a complex multi-agent system
- Choose patterns based on failure modes, not capability demos
- Design tools first, agents second
- Plan for failure from day one

## Milestone
Build a supervisor/worker system in LangGraph with state checkpointing, then a tool-calling loop, then add observability + eval.