## Multi-Agent Coordination Patterns — Cheat Sheet

### Pattern Comparison
| Pattern | Structure | Best For | Key Trait |
|---|---|---|---|
| Supervisor/Worker | 1 router, flat layer of specialists | Intent-based task dispatch (support tickets, classification) | Central routing decision, workers never talk to each other |
| Hierarchical Tree | Multiple nested management layers | Complex decomposed projects (research→sub-research) | Manager-of-managers, deep delegation |
| Sequential Pipeline | Fixed linear A→B→C | ETL-style, strict ordered/compliance workflows | No branching, no feedback loop, lowest overhead |
| Collaborative Debate/Critique | Peer agents iterate on shared artifact | Quality refinement (writer+critic, code review) | Feedback loops, multiple revision passes |

### Task Routing & Delegation Rules
- Route on: **classified intent** + **required capability/tool access**, not just agent availability.
- Supervisor should **never bypass** its own routing logic for edge cases — define a fallback/default agent.
- Exam trap: choosing hierarchical when the described workflow is actually flat (single routing decision) — count the *layers* of delegation, not just the number of agents.

### Handoff Protocol — Must-Have Payload Fields
| Field | Purpose |
|---|---|
| task/context summary | Lets receiver act without re-deriving history |
| conversation history (relevant slice) | Avoids repeated user queries |
| completion criteria | Defines "done" so receiver knows when to hand back |
| correlation_id | Matches async response to original request |

### Agent-to-Agent Message Schema (typical)
```
{
  "sender": "router-agent",
  "recipient": "billing-agent",
  "intent": "resolve_invoice_dispute",
  "payload": { ... task data ... },
  "correlation_id": "task-8842"
}
```
- `correlation_id` is the field tested most for "how does supervisor match async replies to tasks."

### Shared-State (Blackboard) Contract
- Use when ≥2 agents need concurrent read/write of common evolving data (facts, scores, unresolved questions).
- Avoid pairwise point-to-point messaging between every agent — it scales O(n²) and creates inconsistent views.

### Loop Safety
- Debate/critique and any cyclic graph MUST cap iterations (e.g., LangGraph default `recursion_limit` = 25) to prevent infinite handoff loops.

### Pattern-Matching Decision Rule (for exam scenarios)
1. Fixed unconditional order, no feedback → **Sequential Pipeline**
2. One central decision-maker, flat specialists → **Supervisor/Worker**
3. Multiple nested delegation layers → **Hierarchical Tree**
4. Agents iterate/critique the same artifact → **Collaborative Debate/Critique**

### Common Exam Traps
- Confusing "many agents" with "hierarchical" — hierarchical requires **layers**, not just agent count.
- Assuming shared state is only for hierarchical patterns — it's independent of topology.
- Missing `correlation_id` in handoff design when multiple concurrent tasks exist.
