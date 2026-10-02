## Ch.11 Cheat Sheet — Guardrails, Security, Responsible Deployment

### NeMo Guardrails: 5 Rail Types
| Rail | Runs | Purpose |
|---|---|---|
| Input | Before LLM sees user msg | Jailbreak/topic detection on user input |
| Dialog | During conversation flow | Canonical intent, routing, refusal flows |
| Retrieval | On RAG/context fetch | Sanitize retrieved docs before use |
| Execution | Before tool/function call runs | **Permission boundary enforcement**, policy checks |
| Output | Before response returned | PII/toxicity/hallucination check on final answer |

### Risk Tiering → Oversight Control
| Risk Tier | Example Action | Control |
|---|---|---|
| Low (reversible, read-only) | Query KB, read balance | Fully autonomous + log |
| Medium | Update ticket, send internal msg | Autonomous + post-hoc review / soft approval |
| High (irreversible, external) | Wire transfer, delete data, external email | Mandatory human-in-the-loop approval gate |

### Prompt Injection Types
| Type | Vector | Defense |
|---|---|---|
| Direct | User types malicious instruction | Input rail + instruction hierarchy |
| Indirect | Embedded in doc/web/email/tool output | Treat tool output as untrusted DATA; execution rail gate |
| Jailbreak | Crafted prompt to bypass safety training | Output rail + refusal fine-tuning (different from injection) |

### Tool-Misuse Defense Checklist
- Least-privilege, task-scoped credentials per tool (never one broad key)
- Allowlist specific actions/endpoints, not entire APIs
- Sandbox execution environment for code/tool actions
- Execution rail validates action against policy BEFORE call runs
- Never let tool output alter the instruction hierarchy

### Data Exfiltration Risk Points
- Tool OUTPUT containing hidden instructions → triggers unwanted follow-up tool call
- Agent MEMORY persisting sensitive data → leaks in unrelated future session
- Fix: untrusted-output handling + memory scoping/redaction

### Auditability — Minimum Log Fields
- Timestamp, input prompt, tool call + params, tool output, final action, trace/session ID
- Store tamper-evident/immutable

### OWASP Top 10 for LLM Apps (relevant items)
- LLM01: Prompt Injection
- LLM06: Excessive Agency (over-broad permissions)
- LLM02: Insecure Output Handling

### Common Exam Traps
- Thinking a strong system prompt stops injection (it doesn't — instructions can be overridden by context).
- Confusing input rail (user text) with execution rail (tool-call gating) as the injection defense point.
- Assuming HITL should gate every action instead of only high-risk/irreversible ones (over-gating breaks automation).
