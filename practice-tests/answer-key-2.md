# Answer Key — Practice Test 2

**Q1. Correct: C**

Tree-of-Thought explicitly maintains multiple partial solution branches and allows evaluating and discarding weak branches, which matches the need to backtrack after a bad early move.

---

**Q2. Correct: A**

Since the steps and dependencies are fully known and unlikely to change, planning once and executing deterministically avoids repeated LLM reasoning calls, directly satisfying the cost-minimization requirement.

---

**Q3. Correct: D**

Enforcing a required, typed field at the JSON schema level allows the calling framework to structurally validate and reject malformed calls before execution, independent of prompt phrasing.

---

**Q4. Correct: D**

Distinct specialized personas, tools, and guardrails per domain, combined with the need for a coordinated final answer, argue for an orchestrator delegating to domain-specialist sub-agents.

---

**Q5. Correct: A**

Self-critique adds an extra generation pass, raising latency and cost, and is effective at catching reasoning/format errors, but it has no access to ground truth, so it cannot reliably fix factual hallucinations without external grounding.

---

**Q6. Correct: B**

A hard, rule-based deterministic checkpoint reliably enforces the regulatory mandate for human review before irreversible high-value actions, with the agent constrained to only proposing the trade.

---

**Q7. Correct: C**

Plan-and-execute architectures generate an explicit multi-step plan, execute steps against tools, and support localized replanning of just the failed segment on error — minimizing redundant LLM calls while still allowing recovery, which is exactly what a known, structured, multi-step workflow with intermittent tool failures requires.

---

**Q8. Correct: B**

Listing only order_id in the JSON Schema 'required' array forces the model/runtime to reject or retry a tool call missing order_id, while leaving reason absent from 'required' correctly marks it optional — this is the standard structured-output pattern tested for tool-calling schema design.

---

**Q9. Correct: C**

Self-reflection (also called self-critique or Reflexion-style looping) is defined by a single agent evaluating and revising its own prior output using feedback or stored memory of mistakes, without requiring a second distinct agent persona — exactly what's described.

---

**Q10. Correct: B**

Constraining agent freedom via a deterministic workflow graph — where the overall control flow and audit trail are fixed and the LLM is scoped to narrow, bounded sub-decisions — provides the traceability and compliance guarantees required in regulated settings, while still using LLM reasoning where appropriate.

---

**Q11. Correct: B**

A hard round cap combined with an explicit approval-based early-exit condition guarantees bounded execution while still allowing efficient termination when quality converges — the standard pattern for debate/critique loops in production agentic systems.

---

**Q12. Correct: B**

LangGraph's StateGraph natively supports conditional edges for cycles, and pairing it with a checkpointer (e.g., persistent SQLite/Postgres backend) enables durable state that can pause for human input and resume after a restart.

---

**Q13. Correct: D**

recursion_limit is the LangGraph run configuration parameter that caps the number of super-steps a graph executes; its default value is 25, and exceeding it raises a GraphRecursionError.

---

**Q14. Correct: A**

The NeMo Agent toolkit (AgentIQ) is purpose-built to be framework-agnostic, wrapping agents from different frameworks (LangGraph, CrewAI, custom code, etc.) as callable functions/tools while providing unified tracing and evaluation across the whole system.

---

**Q15. Correct: C**

In a sequential pipeline, execution naturally ends once the last stage completes. In a peer-to-peer debate loop, there is no inherent 'last stage' — each agent keeps responding to the other indefinitely unless an explicit round limit or convergence condition is added.

---

**Q16. Correct: A**

CrewAI explicitly exposes Process.sequential and Process.hierarchical as swappable execution strategies for the same set of role-based agents, allowing a switch from fixed-order execution to manager-delegated task assignment without redefining agent roles.

---

**Q17. Correct: D**

Hierarchical supervisor/worker is the standard pattern for classify-then-route workflows requiring centralized shared state, an auditable decision trail, and a clean escalation path — the supervisor tracks confidence and hands off to a human when thresholds aren't met.

---

**Q18. Correct: B**

LangGraph is purpose-built for explicit stateful graphs with conditional edges, cycles, and checkpointer-based persistence, allowing a run to be resumed exactly where it left off after failure — the core requirements in the scenario.

---

**Q19. Correct: B**

Preventing runaway loops requires an explicit termination mechanism: a bounded recursion_limit combined with a conditional edge that inspects state (iteration count or convergence signal) to deterministically route to END.

---

**Q20. Correct: C**

Debate/critique patterns are structurally prone to non-convergence, since two agents can keep disagreeing or refining indefinitely; the standard mitigation is a hard max-turn limit paired with a convergence or satisfaction check that forces termination.

---

**Q21. Correct: C**

Long-horizon agents drift because intermediate context dominates over the original objective. Hierarchical planning with periodic re-grounding checkpoints against a persistent, explicitly stored goal representation is the standard mitigation tested on the exam for maintaining goal fidelity across many steps.

---

**Q22. Correct: D**

RRF fuses ranked lists using only rank position (1/(k+rank)), sidestepping the need to normalize incompatible scoring scales between sparse and dense retrievers. This is the standard hybrid retrieval fusion technique tested for agentic RAG pipelines.

---

**Q23. Correct: A**

Section-aware chunking respects document structure (e.g., clause/definition boundaries), and overlap ensures cross-references aren't split apart mid-context. This directly addresses the failure mode of definitions being separated from usage, which is a core exam topic on chunking strategy selection.

---

**Q24. Correct: C**

Episodic memory stores specific, time-and-context-bound events (what happened, when), while semantic memory stores generalized, decontextualized facts and knowledge distilled over time — a core distinction tested for agentic memory architecture design.

---

**Q25. Correct: A**

Progressive/rolling summarization (compaction) preserves salient early information in condensed form while keeping recent context verbatim for fidelity, directly reducing token count and cost/latency without losing critical earlier facts — the standard exam-tested context management technique for long sessions.

---

**Q26. Correct: A**

Cross-encoders jointly encode query and passage for higher precision but are too expensive to run over an entire corpus. The standard pattern is bi-encoder retrieval for recall over the full corpus, followed by cross-encoder re-ranking over a small candidate set (e.g., top 50) to boost precision at low marginal cost — a core hybrid/re-ranked retrieval concept tested on the exam.

---

**Q27. Correct: A**

Combining a compacted rolling summary (working memory) with a retrievable episodic store lets the agent preserve the original goal and plan state cheaply while still being able to pull specific past details on demand — this is the standard pattern for keeping goal-coherence across long-horizon, high-tool-call-count tasks without blowing the context budget or losing critical history.

---

**Q28. Correct: A**

Hybrid retrieval captures both exact lexical matches (error codes, API names via BM25) and conceptual similarity (via dense embeddings), while a cross-encoder re-ranker re-scores the combined candidate set for precision, shrinking the final context to a small, high-relevance set — directly reducing hallucinated citations and keeping token cost/latency low, which is the pattern NVIDIA's agentic RAG guidance emphasizes for grounding.

---

**Q29. Correct: A**

An idempotency key ties every retry of the same logical operation to a single execution on the downstream service, so duplicate attempts are safely deduplicated regardless of how many times the client resends the request.

---

**Q30. Correct: B**

A hardened, network-isolated sandbox with resource limits establishes an actual security boundary that contains malicious or buggy generated code regardless of what the model produces.

---

**Q31. Correct: D**

Standalone MCP servers expose a standardized protocol contract, so any MCP-compatible client (agent framework) can discover and use them without custom, framework-specific integration code — exactly the pluggability the requirement calls for.

---

**Q32. Correct: C**

Constrained decoding enforces schema conformance at the token-generation level itself, making it structurally impossible for the model to emit output that violates the schema.

---

**Q33. Correct: D**

Least-privilege scoping means each tool receives narrowly bounded, time-limited access matched exactly to what it needs, minimizing blast radius if the tool or agent is compromised or misused.

---

**Q34. Correct: B**

Honoring the server's explicit Retry-After guidance, falling back to jittered exponential backoff otherwise, and bounding retries with a circuit breaker recovers from transient throttling without contributing to a thundering-herd overload of the API.

---

**Q35. Correct: C**

Variable-length outputs under naive contiguous KV cache allocation cause fragmentation long before aggregate memory is exhausted. TensorRT-LLM's paged KV cache (block-based, page-attention style) eliminates this fragmentation by allocating/releasing fixed-size pages on demand, and tuning the reserved GPU memory fraction lets the scheduler admit more concurrent sequences safely.

---

**Q36. Correct: D**

Packaging each model as its own NIM microservice gives independent containers, independent TensorRT-LLM engines, independent autoscaling, and independent versioning/rollback — exactly what's needed when models have different profiles and release cycles, with the agent orchestrator calling each endpoint as needed.

---

**Q37. Correct: D**

Topical, jailbreak, and moderation rails are typically independent classifiers that don't depend on each other's output; executing them concurrently instead of sequentially collapses their latency to roughly the slowest single check instead of the sum, while still gating the response on required checks before it's returned.

---

**Q38. Correct: D**

FP8 on Hopper is natively supported in hardware, giving roughly 2x memory reduction versus FP16 with substantially lower accuracy degradation than INT4 methods, making it the recommended tradeoff for use cases sensitive to precision (like structured tool-call generation) while still meeting the memory budget.

---

**Q39. Correct: A**

Distributed tracing with OpenTelemetry spans and a propagated correlation/trace ID across every agent hop and model-serving call is what reconstructs the full call graph with per-hop latency and cost, which is exactly what's needed to debug a multi-agent incident.

---

**Q40. Correct: C**

Under in-flight batching, which sequences are grouped together and in what order changes run to run, and floating-point arithmetic is not strictly associative; this can shift accumulated logit values by tiny amounts, occasionally flipping which token wins a near-tied greedy argmax — a well-documented source of residual non-determinism in high-throughput batched inference even at temperature 0.

---

**Q41. Correct: A**

Least-privilege, per-agent tool permission boundaries combined with a human-in-the-loop approval gate for high-risk financial actions is the defense-in-depth control the cert expects for cross-agent prompt injection leading to tool misuse. Prompt-only defenses are not sufficient.

---

**Q42. Correct: B**

Topical rails are specifically designed to keep the agent's conversation within an approved set of subjects and to block or redirect digressions into disallowed topics like litigation or politics.

---

**Q43. Correct: C**

The approval request must occur before the action executes, with explicit branching so the refund only runs if the human approves — this is the correct ordering for a genuine human-in-the-loop gate on a high-risk action.

---

**Q44. Correct: D**

Trajectory evaluation inspects the intermediate steps (tool calls, order, decisions), catching cases where the final outcome looks correct but the agent got there through an unsafe, wasteful, or non-compliant path — something outcome-only metrics miss entirely.

---

**Q45. Correct: B**

Regression testing non-deterministic agentic systems requires tolerance-based evaluation (semantic similarity or LLM-as-judge) plus multi-run sampling with statistical pass-rate thresholds, rather than brittle exact-match assertions that falsely flag valid variation as failure.

---

