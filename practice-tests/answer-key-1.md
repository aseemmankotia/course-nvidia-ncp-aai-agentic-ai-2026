# Answer Key — Practice Test 1

**Q1. Correct: D**

ReAct explicitly interleaves reasoning traces (thoughts) with actions (tool calls) and observations in a loop, which matches the described think-act-observe cycle.

---

**Q2. Correct: C**

Plan-and-execute agents separate planning from execution, generate a multi-step plan upfront, and support re-planning when a step's outcome diverges from the plan — exactly the failure-and-recompute behavior described.

---

**Q3. Correct: C**

ToT frames reasoning as a search over a tree of intermediate thought states, generating multiple branches, scoring them, and backtracking when a branch appears unpromising — a structural generalization of CoT's single linear path.

---

**Q4. Correct: B**

Structured tool-calling reliability depends on strict typed JSON schemas with required fields explicitly marked, plus clear, unambiguous names/descriptions — this minimizes malformed calls and parameter hallucination.

---

**Q5. Correct: B**

Independent specialized analysis with explicit cross-domain disagreement detection requires distinct expert personas each reasoning separately, coordinated by an orchestrator that reconciles and surfaces conflicts — a canonical multi-agent decomposition pattern.

---

**Q6. Correct: A**

High-stakes actions should reduce agent autonomy via deterministic guardrails (allow-listed tools, validation, human-in-the-loop approval), while lower-risk actions can retain more autonomous reasoning — this is the standard deterministic-vs-autonomous control tradeoff tested for risk-scaled agent design.

---

**Q7. Correct: C**

Tree-of-Thought explicitly structures reasoning as a branching search tree, allowing the agent to generate multiple candidate thought paths, score them, and backtrack from dead ends — exactly what's needed when exploration and comparison of alternatives is required.

---

**Q8. Correct: A**

This is a compliance-critical decision point that must be enforced deterministically rather than left to model judgment. A deterministic branch that checks the amount and routes high-value transactions to a human approval step (while allowing low-value ones to proceed autonomously) directly satisfies the policy and is the core pattern the exam tests for constraining agent freedom on regulated actions.

---

**Q9. Correct: D**

A strict JSON schema with explicit types, required fields, and enum constraints for categorical values forces the model's structured output generation to conform to a validated contract, dramatically reducing malformed calls and enabling automatic validation/rejection before execution — the standard approach tested for reliable function/tool calling.

---

**Q10. Correct: A**

Separating the writer role and critic role into distinct agents with different personas/system prompts implements the self-reflection pattern properly: the critic agent evaluates the writer's output against accuracy and tone criteria and feeds corrections back, producing a genuinely independent review — the exam-favored multi-agent pattern for draft-then-critique workflows.

---

**Q11. Correct: D**

A single coordinating agent that inspects the task, routes work to specialized workers, and aggregates their outputs into a final decision is the defining characteristic of the supervisor/worker pattern.

---

**Q12. Correct: B**

LangGraph models workflows as an explicit graph of nodes and conditional edges, supports cycles for retries, and provides a checkpointer to persist and resume state across runs, matching all stated requirements.

---

**Q13. Correct: A**

Two agents iteratively producing and critiquing an artifact until approval or a round cap is the debate/critique pattern, and capping rounds is the standard safeguard against runaway loops.

---

**Q14. Correct: D**

LangGraph exposes a recursion_limit configuration value that caps the total number of super-steps executed for a run, which is the standard mechanism to stop a cyclic graph from looping forever.

---

**Q15. Correct: B**

NeMo Agent toolkit is explicitly designed as a framework-agnostic layer that connects to agents built in different frameworks to provide unified profiling, tracing, and evaluation without requiring a rewrite.

---

**Q16. Correct: C**

Hierarchical designs add multiple tiers of decision-making authority that route work dynamically, while sequential designs simply chain agents in a fixed order with no runtime routing decisions.

---

**Q17. Correct: A**

The workflow explicitly requires iterative refinement between a generator and a critic with a stopping condition (quality score or max rounds) — this is the textbook debate/critique pattern, which requires an explicit loop and termination condition to avoid running forever.

---

**Q18. Correct: A**

LangGraph models workflows as an explicit state graph where nodes are agents/tools and edges (including conditional edges and cycles) define routing logic based on typed shared state — this is purpose-built for complex, cyclic, conditionally-routed multi-agent control flow.

---

**Q19. Correct: C**

AutoGen's GroupChatManager exposes max_round to cap the number of conversational turns, and is_termination_msg lets you define an explicit condition (e.g., a keyword or consensus signal) to end the chat early — together these directly prevent unbounded, runaway agent loops.

---

**Q20. Correct: C**

Message passing is inherently point-to-point (or broadcast to specific recipients), requiring the sender to address a target agent, whereas a shared blackboard/state store decouples agents — any agent can read or write the common state without knowing which other agent will consume it, enabling looser coupling.

---

**Q21. Correct: D**

Long-horizon agents need an explicit, externally tracked plan/goal-state (not just implicit context) that is checked at each step so sub-goal execution is validated against the original objective, preventing goal drift over many steps.

---

**Q22. Correct: D**

Episodic memory captures specific, time-bound events (e.g., 'user reported issue X on date Y'), whereas semantic memory holds generalized facts and relationships abstracted away from any single event -- a core distinction tested for agent memory design.

---

**Q23. Correct: B**

For procedural technical content, chunking should respect semantic/structural boundaries (sections, numbered steps) with slight overlap so retrieved chunks preserve context and complete procedures, improving retrieval precision and grounding quality.

---

**Q24. Correct: A**

Hybrid retrieval combines dense semantic matching with sparse keyword/lexical matching to catch exact rare-term hits that embeddings may miss, and a reranker then reorders combined candidates by true relevance -- the standard pattern for improving precision on domain-specific/rare-term corpora.

---

**Q25. Correct: B**

RAG retrieves only the most relevant chunks per query, dramatically reducing token consumption and inference latency/cost compared to stuffing the full corpus, while still grounding responses in the needed knowledge.

---

**Q26. Correct: B**

Cross-session recall requires persistent long-term memory (e.g., summarized episodes stored in a vector database keyed by customer/session) that can be retrieved on demand, rather than depending on ephemeral context windows.

---

**Q27. Correct: C**

Summarization/compaction of stale turns into condensed long-term memory, combined with a sliding window of recent verbatim turns, preserves the original goal and key facts while keeping token usage bounded — this is the standard pattern for long-horizon agent memory management tested on the NCP-AAI exam.

---

**Q28. Correct: B**

Hybrid retrieval captures both semantic similarity (dense embeddings) and exact lexical matches (sparse/BM25, critical for part numbers and error codes), and re-ranking with a cross-encoder reorders the merged candidates for precision — this hybrid+re-rank pattern is the recommended approach for grounding agent actions on the NVIDIA stack.

---

**Q29. Correct: C**

Idempotency keys let the downstream service (or an idempotency-aware wrapper) recognize that a retried request is a repeat of an in-flight or completed operation, so it can safely return the original result instead of re-executing the side effect. This is the standard pattern for making non-idempotent APIs safe to retry in agentic tool execution.

---

**Q30. Correct: D**

Sandboxed, resource-limited, network-restricted ephemeral containers isolate arbitrary code execution from the host environment, agent process, and sensitive credentials, containing the blast radius of malicious or buggy generated code — the core principle of safe tool execution for code-running tools.

---

**Q31. Correct: C**

MCP defines a standardized client-host-server architecture where each external system (database, API, filesystem) is exposed through an MCP server offering discoverable resources, tools, and prompts, and the agent host/client connects uniformly to any compliant server — exactly the standardized-connector pattern needed here.

---

**Q32. Correct: A**

Pre-invocation schema validation catches malformed arguments before they reach the function, and feeding a structured error back into the agent's context lets the model self-correct on the next turn — the standard pattern for robust structured argument validation and error recovery in function-calling loops.

---

**Q33. Correct: D**

A centralized, shared rate limiter enforces the provider's per-key quota across all concurrent consumers, smoothing bursts via queuing or backoff — the correct configuration pattern for rate limiting a shared external resource among multiple agent sessions.

---

**Q34. Correct: B**

Function calling is a specific, schema-driven protocol for structured invocation and result return, while 'tool use' is the umbrella term for the agent's overall capability to leverage external functions, retrieval, and other resources — function calling is one mechanism used to implement tool use.

---

**Q35. Correct: A**

NIM microservices package TensorRT-LLM-optimized engines behind a Triton-backed serving layer with in-flight (continuous) batching, which is the supported path to hit low-latency, high-concurrency LLM SLAs on NVIDIA infrastructure.

---

**Q36. Correct: D**

TensorRT-LLM is the library purpose-built to optimize transformer inference through kernel fusion, quantization (e.g., FP8/INT8), and in-flight batching to maximize GPU throughput.

---

**Q37. Correct: A**

Paged KV cache allocates cache memory in fixed-size blocks rather than contiguous per-sequence buffers, reducing fragmentation and memory pressure while preserving throughput — a core TensorRT-LLM optimization tested on the exam.

---

**Q38. Correct: A**

NeMo Guardrails lets teams define topical (and safety/security) rails in Colang that intercept and block disallowed conversation topics at runtime, exactly the exam-tested pattern for policy enforcement without retraining.

---

**Q39. Correct: D**

In-flight batching dynamically admits and evicts requests at the token-generation step level rather than waiting for a fixed batch to fully complete, which keeps GPUs busy and significantly improves throughput versus static batching.

---

**Q40. Correct: A**

Structured, step-level observability that captures per-agent decisions, tool invocations, token usage, and latency (e.g., via OpenTelemetry-integrated tracing) is the standard, exam-tested method for reconstructing and debugging non-deterministic multi-agent behavior in production.

---

**Q41. Correct: B**

High-risk, high-value autonomous actions require a human-in-the-loop approval checkpoint tied to the actual tool/action invocation (an execution-level control), not a text filter. This ensures the agent cannot complete the action until a human authorizes it, directly enforcing the policy threshold.

---

**Q42. Correct: D**

garak is NVIDIA's open-source LLM vulnerability scanner designed to automatically probe models and pipelines for weaknesses like prompt injection, jailbreaks, data leakage, and toxic output generation, producing a structured vulnerability report suitable for pre-release security review.

---

**Q43. Correct: C**

Execution rails in NeMo Guardrails govern which actions/tools the agent is permitted to invoke, enforcing an allowlist at the point of tool/function execution regardless of the surrounding conversation content.

---

**Q44. Correct: B**

Trajectory-based evaluation with an LLM-as-judge rubric can assess whether each intermediate step (retrieval quality, reasoning validity, correct tool invocation and order) was performed correctly, which is essential for multi-step, non-deterministic agentic workflows where final-output-only checks are insufficient.

---

**Q45. Correct: C**

Trajectory metrics specifically evaluate the path an agent takes — the order and correctness of intermediate reasoning steps, tool invocations, and decisions — as opposed to only judging the final result, which is the domain of task-completion metrics.

---

