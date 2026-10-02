## Chapter 11: Guardrails, Security, and Responsible Deployment — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What are the 5 rail types in NVIDIA NeMo Guardrails architecture? | Input, Dialog, Retrieval, Execution, Output rails. Exam tip: Execution rails are where tool/function-call permission boundaries are enforced — not input or output rails. |
| 2 | OWASP Top 10 for LLM Applications — what is LLM01? | Prompt Injection. Exam tip: it's ranked first because it breaks the trust boundary between instructions and untrusted data/content. |
| 3 | Direct vs. indirect prompt injection — key difference? | Direct: attacker types malicious instructions straight into the prompt. Indirect: malicious instructions are hidden in external content (web page, doc, email, tool output) the agent later ingests. Exam tip: indirect injection bypasses input rails entirely. |
| 4 | Where should tool-call permission checks be enforced in a guardrails pipeline? | In the execution rail, after the dialog/decision step but before the tool actually runs. Exam tip: input/output rails alone cannot stop tool misuse. |
| 5 | Colang: what defines a topical refusal flow? | `define user ask about competitors` + `define flow` + `bot refuse to answer`. Exam tip: Colang expresses dialog rails as canonical form + flow definitions. |
| 6 | Risk tiering rule for oversight controls? | Reversibility + blast radius (not just data sensitivity) determine tier: low-risk/reversible → autonomous; high-risk/irreversible → mandatory human-in-the-loop approval gate. |
| 7 | Primary data-exfiltration vector in agentic tool use? | Tool OUTPUT (retrieved doc/web/email content) carrying embedded instructions, or the agent copying sensitive data forward into a later tool call. Exam tip: always treat tool output as untrusted data, never as instructions. |
| 8 | Agent long-term memory — what's the exfiltration risk? | Sensitive data persisted in memory can resurface and leak in a later, unrelated session or tool call. Exam tip: apply memory scoping/redaction before persisting anything sensitive. |
| 9 | Least-privilege tool design principle? | Give each tool the narrowest, task-scoped permission set (allowlisted actions/scopes) — never broad/admin access 'with monitoring.' Exam favors narrowest-scope answers. |
| 10 | Minimum required fields for agentic AI audit logs? | Timestamp, input prompt, tool calls + parameters, tool outputs, final action/decision, and a trace/session ID — stored tamper-evident/immutable for compliance. |
| 11 | Input rail vs. Output rail — timing and purpose? | Input rail: filters the user message BEFORE it reaches the LLM (jailbreak/topic checks). Output rail: filters the LLM response BEFORE it's returned (PII leak, toxicity). Exam tip: injection via tool output bypasses both — needs execution rail. |
| 12 | What should trigger a human-in-the-loop (HITL) approval gate? | The action's RISK TIER — high-risk/irreversible actions (financial transactions, deletions, external communications, permission changes), not the model's confidence score. |
