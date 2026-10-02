## Chapter 5 Cheat Sheet — Task Decomposition & Memory Systems

### Memory Type Selection

| Memory Type | Scope | Use When | Discarded? |
|---|---|---|---|
| Short-term / Working | Current session only | Intermediate reasoning, tool outputs, active sub-step tracking | Yes, at session end |
| Long-term | Cross-session | User preferences, finalized decisions, facts needed in future sessions | No, persists |

**Exam trigger words:** "today's session" / "right now" → working memory. "Last week" / "next time" / "across sessions" → long-term memory.

### Episodic vs Semantic Memory

| Type | Stores | Example | Trigger Words |
|---|---|---|---|
| Episodic | Specific past events, with context (who/what/when) | "What I said last Tuesday" | "specific conversation", "that time when", "last Tuesday" |
| Semantic | Generalized, de-contextualized facts | "Peanuts are a common allergen" | "in general", "as a rule", "generalized fact" |

**Gotcha:** Episodic and semantic memory can BOTH be persisted long-term — the distinction is about content type (specific event vs. generalized fact), not duration.

### Task Decomposition Strategies

- **Static upfront plan:** Generate all steps first, execute in order. Good for short, independent tasks. Fails when steps depend on prior outputs.
- **Hierarchical with checkpoints:** Decompose into sub-goals, execute, re-evaluate with new info, adjust remaining plan. Required for long-horizon, dependency-heavy goals.
- **Goal anchoring:** Re-inject the original objective at each checkpoint/step to prevent drift over many turns.

### Managing Growing Context

| Technique | What it does | Risk if skipped |
|---|---|---|
| Summarization/compaction | Condenses older turns into a running summary, retaining key facts + active goal | Context limit exceeded, forced truncation |
| Blind truncation | Drops oldest messages outright | Loss of critical facts or goal drift |

**Rule of thumb:** Prefer summarization over truncation whenever the original goal or key facts must survive.

### Common Exam Traps

1. Confusing "long context window" with "long-term memory" — a large context window does NOT provide cross-session persistence.
2. Assuming episodic memory = short-term and semantic = long-term — duration and content-type are independent dimensions.
3. Choosing to restart a session or drop context abruptly instead of summarizing/compacting when nearing context limits.
4. Failing to re-anchor the goal during long multi-step decomposition, leading to task drift after many turns.

### Quick Decision Flow
1. Does the requirement span multiple sessions? → Long-term memory.
2. Is it a specific past event/conversation? → Episodic. Is it a generalized fact? → Semantic.
3. Is the task long-horizon with dependent steps? → Hierarchical decomposition + goal re-anchoring.
4. Is context nearing the limit? → Summarize/compact, don't blind-truncate.
