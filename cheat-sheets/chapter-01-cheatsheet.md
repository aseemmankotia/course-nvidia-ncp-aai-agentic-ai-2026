## Chapter 1 Cheat Sheet: Anatomy of an LLM Agent

### The Four-Stage Agent Loop
| Stage | What Happens | Common Confusion |
|---|---|---|
| Perception | Ingest input/observations (user query, tool output, environment state) | Confused with Action |
| Reasoning | Decide next step/action given current state and goal | Confused with Reflection |
| Action | Execute a tool call or produce output | — |
| Reflection | Evaluate the action's outcome vs. goal; revise approach if needed | Confused with Reasoning |

### Control Pattern Selection
| Pattern | Best For | Cost/Latency | Exam Trigger Phrase |
|---|---|---|---|
| ReAct | Sequential tasks where next step depends on live tool/observation results | Per-step reasoning overhead | "outcome determines next action", "check X, then decide" |
| Plan-and-execute | Long, predictable workflows with known steps | Lower — one planning pass | "well-understood workflow", "steps known in advance" |
| Chain-of-thought (CoT) | Single linear reasoning problems | Low | Default reasoning, no branching needed |
| Tree-of-thought (ToT) | Problems needing exploration of multiple candidate solutions | High — branching + evaluation | "explore multiple approaches", "evaluate alternatives before choosing" |

### Single-Agent vs Multi-Agent Decision
| Signal | Choose |
|---|---|
| Narrow scope, few tools, one knowledge base | Single agent |
| Distinct specialized roles needed in parallel | Multi-agent |
| Conflicting sub-goals requiring negotiation | Multi-agent |
| Context/tool set too large for one agent's window | Multi-agent |
| Low coordination need | Single agent (default) |

### Deterministic vs Autonomous Boundaries
- Deterministic boundary = hard-coded gate (human approval, allow-list, rule) applied to **specific high-risk actions** (payments, external comms, destructive ops).
- Autonomous zone = actions the agent can take without gating (internal messages, read-only lookups).
- Exam trigger: "must get approval before X" = deterministic boundary scoped to X only, not full autonomy loss.

### System-Prompt / Persona Design Rules
1. State safety and tool-use constraints explicitly.
2. Give constraints **priority** over persona/tone instructions — don't bury them after a long backstory.
3. Persona shapes tone/voice; it must never be allowed to override hard rules.

### Self-Reflection / Self-Critique Loops
- Purpose: agent critiques and revises its own output before returning it.
- Risk: unbounded iterations → infinite loop or wasted latency/cost with no guaranteed quality gain.
- Fix: cap iterations (small fixed number) or use an explicit convergence/quality check.

### Quick Decision Checklist
- Next step depends on live result? → **ReAct**
- Steps fully known upfront? → **Plan-and-execute**
- Need to explore/compare multiple solution paths? → **Tree-of-thought**
- Task narrow + few tools? → **Single agent**
- High-risk action involved? → **Add deterministic approval gate**
