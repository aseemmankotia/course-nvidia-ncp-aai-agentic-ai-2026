## Chapter 3: Multi-Agent Coordination Patterns — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Supervisor/worker pattern: what owns routing logic? | A single central agent decides which worker handles each task and aggregates results. Exam tip: one flat layer, no worker-to-worker communication. |
| 2 | Hierarchical multi-agent tree vs supervisor/worker — key difference? | Hierarchical has multiple nested delegation layers (manager-of-managers); supervisor/worker is a single flat routing layer. |
| 3 | When is a sequential pipeline the correct pattern? | When task order is fixed and unconditional (A→B→C, no branching, no feedback). Lowest coordination overhead of all patterns. |
| 4 | When is collaborative debate/critique the correct pattern? | When agents must iteratively challenge/refine each other's output (e.g., writer + critic loop) before a final result is accepted. |
| 5 | What must a handoff payload minimally contain? | Task context/summary, relevant conversation history, and completion criteria — enough for the receiving agent to act without re-querying the user. |
| 6 | Standard agent-to-agent message fields (typical schema) | sender, recipient, intent, payload, correlation_id. correlation_id maps async replies back to the original delegated task. |
| 7 | Shared-state / blackboard pattern — when to use? | When multiple agents need concurrent read/write access to common evolving data (facts, scores, open questions) rather than passing full context pairwise. |
| 8 | Why avoid point-to-point queues between every agent pair? | Pairwise links scale O(n²) as agents grow and don't guarantee a consistent shared view — prefer shared state or a central router instead. |
| 9 | LangGraph default recursion_limit (used to cap runaway multi-agent loops) | 25 by default. Exam tip: coordination graphs need an explicit loop/step cap to prevent infinite supervisor↔worker cycles. |
| 10 | Task routing rule design — what should routing logic key off? | Classified intent/task type plus required capability/tool access of the candidate agent — not just agent availability. |
| 11 | Common exam trap: 'multiple agents talking directly with no supervisor' — what pattern is this NOT? | It is NOT supervisor/worker (no central router) and NOT sequential pipeline (no fixed linear order) — likely collaborative/peer pattern. |
| 12 | Business workflow: strict compliance sign-off must happen in one fixed order after drafting — which pattern? | Sequential pipeline — fixed order, no branching, matches strict compliance gating requirements exactly. |
