## Chapter 9: Guardrails, Observability, and Scaling Multi-Agent Workloads — Practice Questions

**Q1.** A company deploys a multi-agent customer support pipeline. They must block prompt injection attempts before the LLM processes user input, and also filter PII before returning agent responses to users. Which NeMo Guardrails configuration correctly implements this?

A) Configure only output rails with PII detection
B) Configure a single rail type that handles both checks
C) Configure output rails for prompt injection and input rails for PII
D) Configure input rails for prompt injection detection and output rails for PII filtering

<details>
<summary>Answer</summary>

**Correct: D**

Input rails run before the user message reaches the LLM/agent logic, making them the correct place to stop injection attempts. Output rails run on the generated response, the correct place to redact PII before it reaches the user.

**Why the others are wrong:**
- Output-rail-only PII detection does nothing to stop injection at the input stage
- A single rail type does not separate pre- vs post-processing concerns and misses the injection check
- Reversing input/output placement means injected prompts reach the LLM unchecked, and PII would leak before any output filtering occurs

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** During production, a multi-agent system produces inconsistent tool-calling decisions for the same input across runs. Which approach best helps diagnose this?

A) Increase the model's temperature to make behavior more diverse for testing
B) Restart the agent orchestrator service
C) Enable step-level tracing capturing each agent's decision, prompt, and tool call to compare divergent execution paths
D) Remove all guardrails to observe raw model behavior

<details>
<summary>Answer</summary>

**Correct: C**

Step-level tracing exposes exactly where two runs diverge (which agent, which prompt, which tool call), which is required to root-cause non-deterministic behavior.

**Why the others are wrong:**
- Increasing temperature would make the system more, not less, non-deterministic and doesn't aid debugging
- Restarting the orchestrator does not surface the root cause of divergence
- Removing guardrails changes behavior and destroys the very safety layer under investigation, without adding diagnostic visibility

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A team needs to scale a multi-agent workflow to handle thousands of concurrent user sessions while controlling GPU cost. Which combination of levers is most effective?

A) Increase the context window size for every agent call
B) Disable observability tracing to reduce overhead
C) Run every agent on the largest available model to guarantee consistent output
D) Route simple sub-tasks to smaller/cheaper models, batch requests where possible, and cap max concurrent agent sessions per node

<details>
<summary>Answer</summary>

**Correct: D**

Model routing by task complexity, request batching, and concurrency caps directly reduce per-request compute cost while protecting throughput and stability at scale.

**Why the others are wrong:**
- Larger context windows increase token cost and latency per call without addressing concurrency scaling
- Disabling tracing may save marginal overhead but removes the visibility needed to manage cost and debug at scale, and doesn't itself control concurrency cost
- Running every agent on the largest model maximizes cost and latency rather than controlling it

</details>

---

**Q4.** Which NVIDIA component is designed to serve as the retrieval layer that feeds grounded, up-to-date context into agent actions?

A) NeMo Guardrails
B) NeMo Retriever
C) Triton Inference Server
D) A NIM microservice for LLM inference

<details>
<summary>Answer</summary>

**Correct: B**

NeMo Retriever provides the embedding/retrieval microservices used to fetch relevant context from enterprise data, grounding agent actions in accurate, current information.

**Why the others are wrong:**
- NeMo Guardrails is the safety/control layer for input and output, not a retrieval component
- Triton Inference Server is a general model-serving runtime, not specifically a retrieval layer
- A NIM microservice for LLM inference serves the generative model itself, not the retrieval step

</details>

---

**Q5.** Which telemetry dimensions should be captured per agent step to support both performance tuning and budget management in a multi-agent deployment?

A) Only latency
B) Only the final output text
C) Token count, latency, and cost per step
D) Only the number of agent hops

<details>
<summary>Answer</summary>

**Correct: C**

Token count, latency, and cost per step together let engineers identify which step is slow, expensive, or token-heavy — the three metrics the exam associates with per-step observability.

**Why the others are wrong:**
- Latency alone hides token/cost drivers behind that latency
- Final output text tells you nothing about which internal step caused a cost or latency spike
- Hop count alone doesn't reveal where token or cost inefficiency actually occurred

</details>

---

**Q6.** In a NeMo Guardrails pipeline, where should a rail be placed to reject a user's request before it ever reaches the LLM or agent logic?

A) Input rail
B) Output rail
C) Retrieval rail
D) Post-execution rail

<details>
<summary>Answer</summary>

**Correct: A**

Input rails execute on the incoming user message before it is passed to the LLM/agent, making them the correct enforcement point to reject disallowed requests up front.

**Why the others are wrong:**
- Output rails run only after generation, too late to prevent the request from being processed
- Retrieval rails govern what context is fetched, not whether the request itself is accepted
- There is no standard 'post-execution rail' concept distinct from output rails in this pipeline

</details>

---

**Q7.** Which cost-control lever is LEAST likely to reduce spend in a large multi-agent deployment?

A) Increasing max_tokens for every agent to avoid truncation
B) Caching repeated retrieval/tool results across sessions
C) Setting per-agent token budgets and early-exit conditions
D) Routing routine sub-tasks to smaller NIM-hosted models

<details>
<summary>Answer</summary>

**Correct: A**

Raising max_tokens uniformly increases the ceiling for generated tokens per call, which tends to raise cost and latency rather than reduce it.

**Why the others are wrong:**
- Caching avoids redundant retrieval/tool calls, directly cutting repeated compute cost
- Token budgets and early-exit stop agents from over-generating, directly capping spend
- Routing to smaller models for simple sub-tasks lowers per-call inference cost

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the primary purpose of output rails in NeMo Guardrails, as distinct from input rails?

A) Authenticating the end user prior to any LLM call
B) Selecting which retrieval index to query
C) Validating and filtering the model's/agent's generated response before it reaches the user, such as PII redaction or topic restriction
D) Load balancing across GPU nodes

<details>
<summary>Answer</summary>

**Correct: C**

Output rails act as a post-processing check on the generated response, enforcing policies like PII redaction, toxicity filtering, or topical restrictions before content reaches the end user.

**Why the others are wrong:**
- User authentication is a security/session concern, not a guardrails rail function
- Selecting a retrieval index is a retrieval-layer/orchestration concern, not an output rail's job
- GPU load balancing is an infrastructure/serving concern unrelated to guardrails rail logic

</details>

---

