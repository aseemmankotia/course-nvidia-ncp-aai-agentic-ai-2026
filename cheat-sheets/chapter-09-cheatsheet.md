## Chapter 9 Cheat Sheet: Guardrails, Observability, Scaling

### NeMo Guardrails Placement
| Rail Type | Runs When | Purpose |
|---|---|---|
| Input rail | Before LLM/agent sees message | Block injection, jailbreaks, off-topic input |
| Dialog rail | During conversation flow control | Enforce allowed conversation paths |
| Retrieval rail | When fetching context | Validate/filter retrieved documents |
| Execution rail | Around tool/action calls | Gate what actions an agent may take |
| Output rail | After generation, before user sees it | PII redaction, toxicity filter, fact-check |

**Memory trick:** Input = gatekeeper at the door. Output = inspector at the exit.

### NeMo Retriever
- Retrieval/embedding layer that grounds agent actions in enterprise data.
- NOT a safety component — do not confuse with Guardrails on exam questions asking 'which component grounds responses in current data.'

### Step-Level Tracing — What to Capture
| Dimension | Why It Matters |
|---|---|
| Step/agent identity | Know which agent made which decision |
| Prompt + tool call | Reproduce and compare divergent runs |
| Token count | Identify token-heavy steps |
| Latency | Identify slow steps/bottlenecks |
| Cost | Attribute spend to specific steps |

### Debugging Non-Deterministic Behavior
1. Enable step-level tracing.
2. Compare divergent runs at the agent/tool-call level.
3. Check for randomness sources (temperature, retrieval order, race conditions) — don't just restart services.
4. Never remove guardrails to 'test raw behavior' as a debugging step — it changes the system under test without adding diagnostics.

### Scaling Multi-Agent Concurrency — Levers
| Lever | Effect |
|---|---|
| Route simple tasks to smaller models | Lowers per-call inference cost |
| Batch requests | Improves GPU utilization |
| Cap concurrent sessions per node | Prevents resource contention/cost spikes |
| Cache retrieval/tool outputs | Avoids redundant compute |
| Per-agent token budgets + early exit | Bounds worst-case spend per session |

### Cost-Control Anti-Patterns (exam traps)
- Increasing `max_tokens` broadly → raises cost, doesn't fix truncation issues efficiently.
- Running every agent on the largest model → maximizes cost/latency.
- Disabling observability to save overhead → removes visibility needed for cost/debug control.

### Exam Domain Weight
Deployment, Optimization, and NVIDIA Infrastructure ≈ 14% of exam. Heavily tested: Guardrails rail placement, step-level observability metrics, concurrency/cost scaling levers.
