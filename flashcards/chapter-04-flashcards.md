## Chapter 4: Orchestration Frameworks and State Management — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | LangGraph default recursion_limit value? | 25. Exceeding it raises GraphRecursionError. Exam tip: this is a safety net, not a substitute for a real termination condition — always pair it with a conditional edge to END. |
| 2 | AutoGen GroupChat parameter that caps total conversation rounds? | max_round (default 10). Exam tip: distinguish from max_consecutive_auto_reply, which limits per-agent auto-replies, not total rounds. |
| 3 | CrewAI's two process types? | Sequential (tasks execute in fixed order) and Hierarchical (a manager agent delegates and coordinates). Exam tip: Hierarchical needs a manager LLM configured. |
| 4 | Framework for explicit cyclic state machine with conditional branching and checkpoint/resume? | LangGraph — its StateGraph with nodes/edges and a checkpointer supports pause, resume, and human-in-the-loop steps. |
| 5 | Framework for free-form multi-agent conversation until consensus (e.g., coder + reviewer debate)? | AutoGen — GroupChat/GroupChatManager is purpose-built for flexible conversational multi-agent dialogue. |
| 6 | Framework for a simple role-based crew (Researcher/Writer/Editor) with minimal custom orchestration code? | CrewAI — role-based agents run via Sequential or Hierarchical process with little boilerplate. |
| 7 | What is the NVIDIA NeMo Agent toolkit (now called AIQ toolkit)? | A framework-agnostic layer that connects and orchestrates agents built in LangChain, LlamaIndex, CrewAI, etc., adding profiling, observability, and evaluation for enterprise deployment via NVIDIA AI Blueprints. It wraps existing frameworks — it doesn't replace LangGraph/CrewAI/AutoGen. |
| 8 | Control-granularity ranking (least to most) of CrewAI, AutoGen, LangGraph? | CrewAI (least granular, opinionated roles) < AutoGen (conversational, moderate control) < LangGraph (most granular, explicit graph/state control). |
| 9 | Top exam trap in loop/termination design for multi-agent systems? | Runaway loops or deadlocks caused by missing/incorrect termination conditions. Mitigation: explicit termination checks (conditional edges, max_round, max_iter) PLUS a hard iteration ceiling as a safety net. |
| 10 | LangGraph term for the object passed between nodes holding shared data? | State — typically a TypedDict or Pydantic schema. Each node reads and updates this shared state as the graph executes. |
| 11 | What triggers a GraphRecursionError in LangGraph? | The graph executes more super-steps than the configured recursion_limit — almost always a sign of a missing or broken conditional edge routing to END. |
| 12 | What does CrewAI's Hierarchical process require that Sequential does not? | A manager agent/LLM that delegates tasks and coordinates worker agents — an added coordination layer absent from the Sequential process. |
