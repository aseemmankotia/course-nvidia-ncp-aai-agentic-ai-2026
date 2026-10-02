## Chapter 7: Tool Integration, APIs, and the Model Context Protocol — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What message/transport format does MCP (Model Context Protocol) use? | JSON-RPC 2.0. Exam tip: this standardization is what lets any MCP client talk to any MCP server without custom glue code. |
| 2 | MCP client vs MCP server — which one exposes tools/resources/prompts? | The MCP server exposes tools, resources, and prompts. The MCP client (inside the host app) connects and invokes them. Exam tip: don't confuse the host application with the server role. |
| 3 | Purpose of an idempotency key in tool/API calls? | Ensures a retried request with the same key doesn't re-execute the side effect (e.g., no duplicate order). Exam tip: needed for non-idempotent ops like POST, not for GET. |
| 4 | What format typically defines a function-calling argument schema? | JSON Schema — specifying parameter names, types, required fields, enums. Exam tip: the model's output arguments must validate against this schema before the tool executes. |
| 5 | Define least-privilege as applied to tool/service accounts. | Grant only the minimum permissions needed for the tool's task (e.g., read-only instead of admin). Exam tip: avoid reusing personal or admin credentials for automated tool access. |
| 6 | Why sandbox code-execution tools? | To isolate untrusted code from the host system and limit CPU, memory, filesystem, and network access. Exam tip: containers/VMs with resource caps are the standard mechanism. |
| 7 | What does rate limiting protect against in tool integration? | Overwhelming downstream APIs and runaway agent loops making excessive calls. Exam tip: pair with backoff and per-tool/per-user quotas. |
| 8 | Exponential backoff vs fixed-interval retry — which is preferred for transient tool errors? | Exponential backoff with jitter is preferred; it reduces thundering-herd effects and adapts to sustained outages. Exam tip: always cap max retries/total wait time. |
| 9 | How should the reasoning loop handle an unrecognized structured tool error? | Surface it back to the model as an observation so it can reason about alternatives or retry, rather than failing silently. Exam tip: structured error passthrough keeps the agent informed and able to adapt. |
| 10 | MCP vs one-off custom REST integration — key exam differentiator? | MCP is a standardized, reusable connector protocol reducing an N-tools × M-models integration problem to roughly N+M; custom REST wrappers must be rebuilt per tool/vendor pair. |
| 11 | Where in the pipeline should argument validation occur? | Before tool execution — validate model-generated arguments against the tool's schema (types, required fields) prior to invoking the tool. Exam tip: prevents malformed or malicious calls from running. |
| 12 | Idempotent vs non-idempotent operations — give an example of each. | Idempotent: GET, PUT (repeating produces same result). Non-idempotent: POST (each call can create a new resource). Exam tip: only non-idempotent ops require idempotency keys for safe retries. |
