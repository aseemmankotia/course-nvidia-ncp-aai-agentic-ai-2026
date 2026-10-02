## Chapter 9: Guardrails, Observability, and Scaling Multi-Agent Workloads — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Where do NeMo Guardrails input rails execute in an agent pipeline? | Before the user message reaches the LLM/agent logic. Used for prompt injection detection, jailbreak blocking, and topic gating. Exam tip: 'stop it before it starts' = input rail. |
| 2 | Where do NeMo Guardrails output rails execute? | After the model/agent generates a response, before it reaches the user. Used for PII redaction, toxicity filtering, fact-checking. Exam tip: 'clean it before it leaves' = output rail. |
| 3 | What role does NeMo Retriever play in an agentic pipeline? | Provides the retrieval/embedding layer that fetches grounded, current context (e.g., from enterprise docs) to feed agent reasoning and tool actions. It is not a guardrail or safety component. |
| 4 | NeMo Guardrails vs NeMo Retriever — core distinction? | Guardrails = safety/control layer (input/output/dialog/execution rails). Retriever = data-grounding layer (fetches relevant context). Exam tests confusing these two roles. |
| 5 | What are the main rail types in NeMo Guardrails? | Input rails, dialog rails, retrieval rails, execution rails, output rails — each governs a different stage of the conversation/agent flow. |
| 6 | Why is step-level tracing critical for multi-agent debugging? | It records per-agent decisions, prompts, and tool calls, letting you pinpoint exactly where two runs of the same input diverge — essential for debugging non-deterministic behavior. |
| 7 | What three metrics should be tracked per agent step? | Token count, latency, and cost — together they show which step is slow, expensive, or token-heavy. All three, not just one, are needed for full observability. |
| 8 | Best first move when debugging non-deterministic multi-agent output in production? | Enable/inspect step-level tracing to compare divergent execution paths across runs — not raising temperature or disabling guardrails. |
| 9 | Main levers to scale multi-agent concurrency cost-effectively? | Route simple sub-tasks to smaller models, batch requests, cap concurrent sessions per node, cache repeated retrieval/tool calls, and set per-agent token budgets. |
| 10 | Does increasing max_tokens per agent reduce cost? | No. It raises the ceiling on generated tokens per call, typically increasing cost and latency — a common wrong-answer trap on cost-control questions. |
| 11 | Cost-control lever: caching retrieval/tool results | Reusing prior retrieval or tool outputs across sessions avoids redundant compute, directly cutting cost in high-concurrency multi-agent deployments. |
| 12 | Model routing as a cost lever | Send routine/simple sub-tasks to smaller, cheaper NIM-hosted models; reserve larger models for complex reasoning steps. Reduces average per-call inference cost across the workflow. |
