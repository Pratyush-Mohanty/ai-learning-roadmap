# Agent Architectures (Advanced)

How to design reliable multi-agent systems — the hardest and most in-demand area in 2026.

## Primary Framework
- **LangGraph** — https://github.com/langchain-ai/langgraph
  Stateful, checkpointed agent graphs. The production standard. Learn this first.

## Reference Patterns (the 2026 taxonomy)
**Google's 8 patterns:** sequential, parallel, loop/critic, hierarchical, supervisor/worker, peer-to-peer, marketplace, human-in-the-loop (in/on the loop)

- **Azure Architecture Center: AI Agent Orchestration Patterns** — https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/ai-agent-design-patterns
  Sequential, concurrent, group chat, handoff, magentic patterns.
- **Multi-Agent System Design Guide** — https://baeseokjae.github.io/posts/multi-agent-system-design-guide-2026/
  State management, fault tolerance, cost engineering, observability.

## GitHub (learn by example)
- **AutoGen** — https://github.com/microsoft/autogen
  Multi-agent conversations with human-in-the-loop.
- **CrewAI** — https://github.com/crewAIInc/crewAI
  Role-based agent teams (simple entry point).
- **MetaGPT** — https://github.com/FoundationAgents/MetaGPT
  SOP-driven software company simulation (advanced).
- **AI Agent Design Patterns** — https://github.com/balakumardev/ai-agent-design-patterns
  8 production patterns with runnable LangGraph code.
- **Learn Agentic AI** — https://github.com/panaversity/learn-agentic-ai
  Cloud-native agentic AI on Kubernetes + Dapr.
- **Agent2Agent (A2A) Protocol** — https://github.com/google/A2A
  Agent-to-agent communication standard.
- **MCP Servers** — https://github.com/modelcontextprotocol/servers
  Model Context Protocol: agent-to-tools standard.
- **AI Agent Papers** — https://github.com/masamasa59/ai-agent-papers
  Biweekly curated agent research papers. Stay current.

## Research (depth)
- **Agentic AI: Comprehensive Survey of Architectures** — https://arxiv.org/abs/2510.25445
- **AI Agents: Review of Evolution, Architectures, Applications** — https://ieeexplore.ieee.org/document/11241897

## Key Production Concepts
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