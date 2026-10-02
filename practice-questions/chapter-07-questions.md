## Chapter 7: Tool Integration, APIs, and the Model Context Protocol — Practice Questions

**Q1.** An agent calls a payment API to create an order. The call times out, and the agent's retry logic automatically resends the same request. What is the BEST way to prevent the order from being created twice?

A) Retry immediately without any modification to the request
B) Increase the API rate limit so retries succeed faster
C) Retry synchronously in a tight loop until a 200 response is received
D) Attach an idempotency key to the request so retries with the same key return the original result

<details>
<summary>Answer</summary>

**Correct: D**

Idempotency keys let the downstream service recognize a retried request as a duplicate of a prior attempt and return the original result instead of executing the side effect again, which is the standard pattern for safe retries on non-idempotent operations like POST.

**Why the others are wrong:**
- Retrying without changes does nothing to prevent duplicate execution if the first request actually succeeded server-side but the response was lost
- Rate limits control call frequency, not duplicate side effects
- A tight retry loop with no backoff or dedup mechanism increases the chance of duplicate orders and can trigger the rate limiter

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A team is adding a 'run Python snippet' tool to their agent so users can execute arbitrary code for data analysis. Which design BEST satisfies safe execution requirements?

A) Run the code inside an isolated, resource-limited sandbox (e.g., container) with restricted filesystem and network access
B) Execute the code directly in the same process as the orchestrator for lowest latency
C) Disable the feature and only allow pre-written code templates
D) Run the code with root/admin privileges so it never fails due to permission errors

<details>
<summary>Answer</summary>

**Correct: A**

Sandboxed execution with CPU/memory/network limits isolates untrusted code from the host system while still fulfilling the requirement to execute arbitrary code.

**Why the others are wrong:**
- Running in the same process as the orchestrator exposes the host to arbitrary code execution risk
- Disabling the feature fails the stated business requirement
- Elevated privileges violate least-privilege and dramatically increase blast radius of any exploit

</details>

---

**Q3.** An enterprise wants its agent to connect to a database, a search index, and a ticketing system, and wants new tools to be added by different teams without custom integration code for each model/tool pair. Which approach BEST fits this requirement?

A) Write a custom REST wrapper and prompt instructions for each tool individually
B) Hardcode API keys and endpoint URLs directly into the system prompt
C) Fine-tune a separate model checkpoint for each backend system
D) Adopt the Model Context Protocol (MCP) so each system is exposed as a standardized MCP server the agent can connect to

<details>
<summary>Answer</summary>

**Correct: D**

MCP standardizes how tools, resources, and prompts are exposed to any compliant client, turning an N-tools-times-M-models integration problem into a single reusable connector pattern.

**Why the others are wrong:**
- Custom wrappers work but require rebuilding integration logic per tool and don't scale across teams/models
- Hardcoding credentials in prompts is an insecure anti-pattern and not an integration architecture
- Fine-tuning per backend is costly, doesn't generalize, and isn't how tool access is implemented

</details>

---

**Q4.** In a function-calling pipeline, which component is responsible for confirming that the model's generated arguments match the tool's expected parameter names, types, and required fields before the tool actually runs?

A) The vector database retriever
B) The schema/argument validation layer sitting between the model output and the tool executor
C) The content moderation/guardrail filter
D) The embedding model used for semantic search

<details>
<summary>Answer</summary>

**Correct: B**

Argument validation against the tool's JSON Schema is a distinct step that catches malformed or missing parameters before invocation, preventing runtime errors or unsafe calls.

**Why the others are wrong:**
- The retriever handles document/context lookup, not tool argument correctness
- Guardrails filter content for safety/policy, not structural argument correctness
- The embedding model produces vectors for similarity search and has no role in tool invocation

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** In the Model Context Protocol architecture, which component exposes a set of tools, resources, and prompts that a connected application can invoke?

A) The MCP client
B) The MCP server
C) The host LLM application only
D) The orchestrator's planning module

<details>
<summary>Answer</summary>

**Correct: B**

An MCP server exposes the actual tools, resources, and prompt templates; the MCP client (embedded in the host application) connects to one or more servers to consume them.

**Why the others are wrong:**
- The client consumes capabilities, it does not expose them
- The host application embeds the client but is not itself the thing exposing tools/resources per the spec
- The planning/orchestrator module decides which tool to call but doesn't define the MCP exposure layer

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** A tool only needs to read records from a customer database for a support agent. Which configuration BEST reflects least-privilege access?

A) Configure the tool's service account with full database administrator credentials for flexibility
B) Scope the tool's service account to a read-only role limited to the required tables
C) Have the tool reuse the logged-in support agent's personal database password
D) Grant read-write access to all tables in case future features need writes

<details>
<summary>Answer</summary>

**Correct: B**

Least-privilege requires granting only the minimum permissions needed for the current task — a scoped read-only role on the relevant tables.

**Why the others are wrong:**
- Admin credentials grossly exceed what's needed and expand the blast radius of any compromise
- Reusing personal user credentials in a service account is an anti-pattern and complicates auditing/permissioning
- Granting write access 'just in case' violates least-privilege by provisioning for unneeded future capability

</details>

---

**Q7.** A tool call fails with a transient timeout error inside the agent's reasoning loop. Which retry strategy is considered safest and BEST practice?

A) Retry immediately and indefinitely until it succeeds
B) Abandon the task permanently with no retry
C) Retry once immediately with modified, randomized arguments
D) Apply exponential backoff with jitter, a capped maximum number of attempts, and an idempotency check before retrying

<details>
<summary>Answer</summary>

**Correct: D**

Exponential backoff with jitter avoids overwhelming a struggling downstream service, a retry cap prevents infinite loops, and idempotency checks prevent duplicate side effects — together this is the standard safe-retry pattern.

**Why the others are wrong:**
- Immediate infinite retries can worsen an outage and never terminate on persistent failures
- Abandoning immediately ignores that many timeouts are transient and recoverable
- Randomizing arguments changes the semantics of the call and doesn't address the actual failure cause

</details>

---

**Q8.** What primarily distinguishes a formal function-calling schema from a freeform natural-language tool description embedded in a prompt?

A) The schema is only used for UI display purposes and has no effect on execution
B) Freeform descriptions are always more accurate than schemas
C) Schemas are an optional pricing-tier feature offered only by some vendors
D) The schema defines structured parameter names, types, and required fields that can be programmatically validated, while freeform text cannot be reliably validated

<details>
<summary>Answer</summary>

**Correct: D**

A function-calling schema (typically JSON Schema) specifies structured, machine-checkable parameter definitions, enabling automated validation of model-generated arguments before execution — something freeform text cannot support reliably.

**Why the others are wrong:**
- Schemas directly gate execution correctness, not just UI presentation
- Freeform descriptions are more prone to ambiguity and hallucinated parameters, not more accurate
- Structured schemas are a core function-calling capability, not a paid add-on tier

</details>

---

