## Chapter 7 Cheat Sheet — Tool Integration, APIs, and MCP

### Function-Calling Schema Essentials
| Element | Purpose | Exam Trap |
|---|---|---|
| JSON Schema | Defines param name/type/required fields | Assuming freeform text descriptions are validated the same way — they aren't |
| Validation layer | Checks model args against schema BEFORE execution | Skipping validation and letting the tool executor handle malformed input directly |
| Tool executor | Runs the actual call once args are validated | Confusing this with the guardrail/content filter layer |

### MCP Connector Pattern
| Component | Role |
|---|---|
| MCP Server | Exposes tools, resources, prompts |
| MCP Client | Embedded in host app; connects to server(s) |
| Transport | JSON-RPC 2.0 |
| Value | Turns N tools × M models integration into ~N+M via standardization |

Exam Trap: MCP server ≠ host application. The host embeds a client; the server is the thing exposing capabilities.

### Safe Execution Checklist
| Concern | Mitigation |
|---|---|
| Untrusted code execution | Sandbox/container with CPU, memory, network, filesystem limits |
| Over-privileged tool access | Least-privilege scoped service accounts (read-only where possible) |
| Downstream overload | Rate limiting per tool/user + quotas |
| Duplicate side effects on retry | Idempotency keys on non-idempotent ops (POST) |
| Transient failures | Exponential backoff + jitter + capped max retries |
| Unknown tool errors | Surface structured error back into reasoning loop, don't fail silently |

### Idempotency Quick Rules
- GET, PUT → naturally idempotent, safe to retry as-is.
- POST/create-type ops → NOT idempotent by default; require an idempotency key.
- Always cap retries; never retry indefinitely.

### Retry Strategy Decision
1. Transient error (timeout, 5xx) → exponential backoff + jitter, capped attempts, check idempotency.
2. Permanent error (4xx auth/validation) → do NOT blindly retry; surface to model/user to correct input.
3. Rate-limited (429) → respect Retry-After header if present, back off accordingly.

### Least-Privilege Quick Rules
- Scope credentials to the narrowest role needed (read-only vs read-write vs admin).
- Never reuse personal/end-user credentials for automated service accounts.
- Avoid provisioning permissions 'just in case' for future features.

### Common Wrong-Answer Patterns to Recognize
- 'Retry immediately/infinitely' — ignores backoff and duplicate-effect risk.
- 'Grant admin/root for convenience' — violates least-privilege.
- 'Run untrusted code in-process' — no sandbox isolation.
- 'Custom wrapper per tool' when question implies scale/standardization need — MCP is the better fit.
