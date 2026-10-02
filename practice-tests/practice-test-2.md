# Practice Test 2: NVIDIA NCP-AAI: Agentic AI Certification Prep

- **Questions:** 45
- **Time limit:** 90 minutes
- **Passing score:** 70%%

> Answers are in `answer-key-2.md`. Track results in `score-tracker.md`.

---

**Q1.** An agent must solve a puzzle where an early move can be wrong and only becomes apparent several steps later, requiring exploration of multiple candidate move sequences and the ability to abandon bad branches mid-solution. Which reasoning paradigm BEST fits this requirement?

A) ReAct (interleaved reasoning and acting)
B) Chain-of-Thought (single linear reasoning trace)
C) Tree-of-Thought (branching exploration with backtracking)
D) Plan-and-Execute (upfront plan then sequential execution)

---

**Q2.** A finance team needs an agent to generate a quarterly report by pulling data from four known APIs in a fixed order, aggregating results, and producing a PDF. The steps and dependencies are fully known in advance and do not change based on intermediate results, and minimizing per-step LLM token cost is a priority. Which control pattern is BEST?

A) Plan-and-execute: generate the full plan once, then execute the fixed steps deterministically
B) A ReAct loop that reasons before every single action
C) Tree-of-Thought exploration of multiple possible report structures
D) A fully autonomous reactive agent with no upfront plan

---

**Q3.** You are designing a tool-calling schema for an agent that invokes a 'create_refund' function. To minimize the chance the model invokes the function with an invalid or missing amount field, which schema design choice is MOST effective?

A) Describe the amount format only in the system prompt's narrative text
B) Allow amount to be an optional free-text string parsed later
C) Rely on the model's few-shot examples alone without schema constraints
D) Make amount a required field with a numeric type constraint enforced via the function's JSON schema

---

**Q4.** A company wants an assistant that independently reasons about legal compliance, technical troubleshooting, and billing disputes, each requiring distinct specialized prompting, tool access, and safety guardrails. Users submit tickets covering all three domains simultaneously. Which architecture is BEST?

A) A single agent with one large system prompt covering all three domains
B) Three independent agents running in parallel with no coordination, merging outputs by concatenation
C) A single agent using only chain-of-thought to reason through all domains sequentially in one context
D) A multi-agent system with a router/orchestrator agent delegating to domain-specialist sub-agents

---

**Q5.** An agent architecture adds a self-critique step where the model reviews and revises its own draft answer before returning it. Which statement about this pattern is MOST accurate for exam purposes?

A) Self-critique increases latency and token cost and improves output quality on reasoning/format errors, but does not guarantee elimination of factual hallucinations without external grounding
B) Self-critique guarantees factual correctness because the model verifies its own output against ground truth
C) Self-critique replaces the need for external grounding or retrieval since the model checks itself
D) Self-critique should always be run at temperature 0 with no additional passes to avoid extra cost

---

**Q6.** An agent is being designed to execute stock trades exceeding $50,000 based on market signal analysis. Regulatory requirements mandate human review before any irreversible high-value action. Which control design is BEST?

A) Full agentic autonomy: let the agent execute trades directly whenever its confidence score exceeds a threshold
B) A deterministic guardrail that routes any trade above the threshold to a human-approval step before execution, with the agent only proposing the action
C) A ReAct loop that reasons and acts without any checkpoint, relying on the LLM's judgment on when to seek approval
D) Remove the threshold and let a self-critique pass decide whether human review is needed

---

**Q7.** A team is building an agent to process refund requests. The workflow requires: verify order details, check refund eligibility against policy, call a payment reversal API, and log the outcome. The payment API is known to fail intermittently, and any failed step must trigger a targeted retry or replan of just that portion of the workflow without restarting the entire process. Which agent architecture BEST fits these requirements?

A) A reactive ReAct agent that decides the next single action after each observation with no persistent plan
B) A single chain-of-thought prompt that reasons through the entire refund logic in one LLM call with no tool invocations
C) A plan-and-execute agent that generates a multi-step plan upfront, executes each step, and dynamically replans only the failing subtree when a step errors
D) A tree-of-thought agent that explores multiple parallel reasoning branches but does not execute any tools

---

**Q8.** You are defining a function-calling/tool schema for a tool named get_refund_status. The tool takes a required string parameter order_id and an optional string parameter reason. Which JSON Schema configuration correctly enforces that the LLM's tool call MUST include order_id, while reason may be omitted?

A) { "required": ["order_id", "reason"] }
B) { "required": ["order_id"] }
C) { "required": [] , "properties": { "order_id": {"type": "string", "nullable": true}, "reason": {"type": "string", "nullable": true} } }
D) { "additionalProperties": true }, with no required array specified

---

**Q9.** An agent generates a first draft of code to solve a task. Before returning a final answer, the SAME agent runs a separate internal pass that compares its own draft output against test cases, identifies flaws, and revises the draft using notes on its own prior mistakes. Which reasoning pattern does this describe?

A) Multi-agent debate between two independently instantiated LLM personas arguing opposing positions
B) Plan-and-execute with a static plan generated once and never revisited
C) Self-reflection (generator-critic loop) in which a single agent iteratively critiques and revises its own output
D) A purely reactive single-pass ReAct loop with no revision step

---

**Q10.** An agent operates in a regulated financial environment where every decision path must be auditable and the agent is prohibited from deviating from company-approved procedures for compliance reasons. Which control strategy BEST satisfies these constraints while still leveraging an LLM for judgment calls?

A) A fully autonomous agent with unrestricted tool access and free-form multi-step reasoning at every turn
B) A deterministic workflow graph with fixed, approved decision points and guardrails, where the LLM is invoked only to resolve constrained sub-decisions within each node
C) A ReAct agent with an unbounded iteration budget and no step limit
D) A tree-of-thought agent that autonomously selects among many self-generated candidate action branches at each step

---

**Q11.** A team implements a debate/critique pattern where a Generator agent drafts a financial summary and a Critic agent reviews it, sending feedback back to the Generator for revision. To prevent runaway loops while still allowing early convergence, which termination design is BEST?

A) Let the Generator agent decide unilaterally when to stop revising
B) Set a fixed maximum number of critique rounds and terminate early if the Critic approves the output
C) Run the debate indefinitely until both agents stop producing new tokens
D) Use a single critique pass with no configurable round limit

---

**Q12.** A workflow requires cyclical branching based on intermediate results, long-running human-in-the-loop pauses, and the ability to resume execution from a saved point after a system restart. Which orchestration approach BEST fits these requirements?

A) CrewAI with Process.sequential
B) LangGraph modeled as a StateGraph with a persistent checkpointer
C) AutoGen GroupChat configured with a fixed max_round
D) NeMo Agent toolkit (AgentIQ) used purely as an observability wrapper with no state graph

---

**Q13.** Which LangGraph configuration parameter bounds the number of super-step iterations a graph can execute before raising a GraphRecursionError, and what is its default value?

A) max_iterations, default 10
B) step_timeout, default 60 seconds
C) max_rounds, default 25
D) recursion_limit, default 25

---

**Q14.** An enterprise has a research agent built in LangGraph and a report-writing crew built in CrewAI. They need a supervisor layer that can invoke both as callable tools, trace calls across both frameworks, and evaluate end-to-end performance — without rewriting either agent. Which approach is designed for this?

A) Use the NeMo Agent toolkit (AgentIQ) as a framework-agnostic layer that wraps each agent as a callable function for orchestration and observability
B) Rewrite both agents inside AutoGen to get a unified conversation protocol
C) Deploy each agent as a standalone NIM with no orchestration layer connecting them
D) Use CrewAI's Process.hierarchical to directly manage the LangGraph agent as a raw LLM call

---

**Q15.** Compared to a sequential agent pipeline, why does a collaborative debate pattern between two peer agents carry a higher inherent risk of runaway or deadlocked interactions?

A) Debate patterns always execute on a single GPU, which causes timeouts
B) Sequential pipelines cannot pass shared state between agents, so the risk doesn't apply to them
C) Debate turns have no natural stopping point since each agent's next turn depends on the other's response, requiring an explicit termination condition
D) Sequential pipelines use larger context windows, which structurally prevents infinite loops

---

**Q16.** A team needs a workflow where agents have clearly defined roles (researcher, writer, editor) executing in a strict fixed order, with the option to later switch to manager-delegated task assignment without redefining the agents themselves. Which framework's built-in abstraction directly supports this via distinct process modes?

A) CrewAI's Process (sequential vs. hierarchical)
B) AutoGen's GroupChat manager
C) LangGraph's conditional edge routing
D) NeMo Agent toolkit workflow YAML configuration

---

**Q17.** A financial services firm is designing a multi-agent claims-processing system. An intake agent classifies each case, then routes it to one of several specialist agents (loan review, fraud detection, compliance check). All agents must read and write to a shared audit trail, and if a specialist agent's confidence falls below a threshold, the case must be escalated to a human reviewer with full context preserved. Which architecture BEST meets these requirements?

A) A fully collaborative debate pattern where every agent discusses every case together before any decision is made
B) A strict sequential pipeline that always processes cases through every agent in a fixed order regardless of classification
C) A decentralized swarm where specialist agents negotiate directly peer-to-peer with no central coordinator or shared state
D) A hierarchical supervisor/worker pattern where a supervisor agent owns the shared state, routes cases to specialist workers, and triggers human-in-the-loop escalation on low-confidence outputs

---

**Q18.** A team is building a research-analyst agent system that requires explicit conditional branching, cycles between a research node and a critique node, and the ability to persist and resume execution state after a mid-run process failure. Which orchestration approach is BEST suited?

A) AutoGen's conversable agent group chat with default round-robin turn-taking
B) LangGraph, defining a StateGraph with nodes, conditional edges, and a checkpointer for state persistence
C) CrewAI's role-based crew running a fixed sequential process
D) The NeMo Agent toolkit's default single-agent ReAct workflow with no graph structure

---

**Q19.** An engineer builds a LangGraph workflow where a critique agent repeatedly reviews a writer agent's output in a loop. To prevent the loop from running away or deadlocking, what should be explicitly configured?

A) Lower the LLM temperature to 0 across both agents to force deterministic output
B) Set the graph's recursion_limit and add a conditional edge that checks an iteration counter or satisfaction flag in state to route to END after a max iteration count or when a stop condition is met
C) Remove the critique agent's system prompt so it stops generating feedback
D) Point both agents at a single shared vector store index for their memory

---

**Q20.** In a collaborative debate/critique multi-agent pattern — for example, a proposer agent and a critic agent iterating on a solution — what is the primary architectural risk that must be explicitly engineered against, and how is it typically mitigated?

A) Excessive GPU memory consumption, mitigated by shrinking each agent's context window
B) Hallucinated tool calls, mitigated by giving each agent access to more tools
C) Infinite non-convergence or deadlock where agents loop without reaching agreement, mitigated by enforcing a maximum turn count and an explicit consensus-check or termination condition
D) Data leakage between agents, mitigated by encrypting the shared conversation state

---

**Q21.** An agent executes a 40-step autonomous research task spanning multiple tool calls and sub-agent handoffs. After step 25, the agent begins pursuing a tangential sub-goal that technically relates to an earlier tool output but no longer serves the original user objective. Which architectural pattern BEST prevents this goal drift in long-horizon planning?

A) Increase the temperature of the planner LLM so it generates more diverse sub-tasks at each step
B) Store the full 40-step transcript verbatim in the context window so nothing is ever lost
C) Use a hierarchical planner that periodically re-grounds each sub-task against the original goal statement stored in a dedicated goal-tracking memory slot
D) Replace the planner with a single-shot prompt that generates all 40 steps upfront before execution begins

---

**Q22.** A retrieval pipeline combines BM25 keyword search results with dense vector similarity search results to answer agent queries over a mixed technical/legal corpus. The two systems produce scores on incompatible scales. Which fusion method should be used to merge the two ranked lists without requiring score normalization?

A) Weighted linear combination of raw BM25 and cosine similarity scores
B) Take the union of both result sets and sort alphabetically by document title
C) Discard the BM25 results entirely and rely only on the dense vector scores since they are already normalized
D) Reciprocal Rank Fusion (RRF), which combines results based on each document's rank position in each list

---

**Q23.** You are building the retrieval component for an agent that answers questions over long legal contracts where clauses frequently reference terms defined many paragraphs earlier. Naive fixed-size chunking with no overlap is producing poor grounding because definitions get separated from the clauses that use them. What is the BEST chunking configuration to fix this?

A) Use larger, section-aware chunks with overlapping windows so referenced definitions remain adjacent to the clauses citing them
B) Reduce chunk size further to 50 tokens so retrieval is more precise
C) Switch entirely to a single embedding for the whole document instead of chunking
D) Keep fixed-size chunking but increase the number of retrieved chunks (top-k) from 3 to 5

---

**Q24.** An agent's long-term memory store contains two types of entries: (1) records of specific past user interactions with timestamps and situational context, and (2) generalized distilled facts learned across many interactions with no reference to when or how they were learned. Which memory types do these two categories represent, respectively?

A) Semantic memory, then episodic memory
B) Working memory, then long-term memory
C) Episodic memory, then semantic memory
D) Short-term memory, then episodic memory

---

**Q25.** An agent performing an extended multi-turn tool-use loop is approaching its context window limit, driving up both latency and per-call cost as the conversation grows. The agent still needs to recall key facts and decisions from early in the session to complete the task correctly. Which strategy BEST balances context-window constraints with retention of critical information?

A) Progressively summarize older turns into a compact rolling summary while retaining recent turns verbatim, updating the summary as the session continues
B) Truncate the oldest turns entirely once the context window is full, keeping only the most recent N turns
C) Always include the full unmodified conversation history regardless of context window size, relying on the model's maximum context length
D) Generate a single fixed summary at the very start of the session and never update it as new information arrives

---

**Q26.** A retrieval pipeline uses a bi-encoder to fetch the top 50 candidate passages from a vector store for each agent query, but downstream answer accuracy is still poor because the top few results are often not the most relevant ones. Which addition to the pipeline BEST improves precision at low added cost, given that cross-encoders are too slow to run over the full corpus?

A) Apply a cross-encoder re-ranker to score and reorder only the top 50 bi-encoder candidates before passing the top few to the agent
B) Replace the bi-encoder entirely with a cross-encoder for the initial full-corpus retrieval step
C) Increase the bi-encoder's embedding dimensionality to improve initial retrieval accuracy
D) Skip retrieval entirely and let the agent generate answers from parametric knowledge only

---

**Q27.** An agent orchestration system executes a long-horizon research task involving 300+ sequential tool calls across an 18-hour run. The underlying LLM has a 128K token context window. The team notices that after ~40 tool calls, the agent starts losing track of the original goal and repeats earlier sub-tasks. Which memory architecture BEST addresses this while controlling cost and latency?

A) Maintain a rolling working-memory summary of the plan and recent steps, paired with an episodic memory store (vector database) that indexes past tool outputs and decisions for on-demand retrieval when relevant
B) Feed the entire raw transcript of all prior tool calls and outputs into the context window on every turn, relying on the 128K window to hold full history
C) Truncate the context window to only the last 5 tool call exchanges and discard everything older to keep latency low
D) Store all tool outputs only in long-term semantic memory as unstructured text blobs and re-embed the entire corpus after each tool call before every LLM turn

---

**Q28.** You are building the retrieval layer for an agent that must ground its tool-invocation decisions in a frequently-updated internal knowledge base of 50,000 technical documents. Requirements: minimize hallucinated citations, handle both exact keyword matches (error codes, API names) and conceptual/semantic queries, and keep the final context passed to the LLM small and highly relevant. Which retrieval configuration BEST satisfies these requirements?

A) Hybrid retrieval combining BM25 keyword search and dense embedding search, followed by a cross-encoder re-ranker that narrows candidates down to the top 5 most relevant chunks before passing them to the agent
B) Dense vector similarity search only, retrieving the top 50 chunks by cosine similarity and passing all 50 directly into the agent's context
C) Pure keyword (BM25) search only, since technical documents contain exact error codes and API names that embeddings may not match precisely
D) A single large embedding of each entire document (no chunking), matched against the query embedding, with the top 3 full documents passed to the agent

---

**Q29.** A company builds an agent that calls a payment processing tool. The agent occasionally retries tool calls due to transient network timeouts, risking duplicate charges. Which design BEST prevents duplicate side effects when retries occur?

A) Generate a unique idempotency key per logical operation and pass it with every retry attempt so the downstream service can deduplicate
B) Increase the tool call timeout so retries happen less frequently
C) Wrap the tool call in a try/except block and log the retry count for later manual reconciliation
D) Disable automatic retries entirely so the agent only ever attempts each tool call once

---

**Q30.** An agent has a 'code execution' tool that runs untrusted, LLM-generated Python code to answer data analysis questions. Which combination of controls BEST mitigates the risk of this tool being used to exfiltrate data or damage the host system?

A) Rely on the LLM's system prompt instructing it not to write malicious code
B) Execute the code in an isolated sandbox (e.g., a microVM) with no outbound network access, a read-only filesystem except a scratch directory, and enforced CPU/memory/time limits
C) Run the code in an unrestricted container with full network access so all analysis libraries function correctly
D) Log all code executions to an audit trail after execution completes, without runtime restriction

---

**Q31.** A team needs to connect their agent to a dozen enterprise data sources (a ticketing system, a CRM, and an internal wiki), and wants new tools to be discoverable and pluggable by any MCP-compatible agent framework without writing framework-specific glue code for each integration. Which approach BEST fits this requirement?

A) Write a single monolithic Python function per data source and register it directly as a native function-call inside one specific agent framework
B) Build a shared internal SDK that each team imports directly into their agent's codebase to call the data sources
C) Store data source credentials in the agent framework's config and let the LLM generate raw HTTP requests to each API directly
D) Implement each integration as a standalone MCP server exposing standardized resources, tools, and prompts, and connect the agent framework as an MCP client

---

**Q32.** When defining a function-calling tool schema for an agent, a developer wants to guarantee that the LLM's output arguments strictly conform to the declared JSON schema (correct types, no missing or extra required fields) before the tool ever executes. Which configuration achieves this most reliably?

A) Add a natural-language instruction in the system prompt asking the model to 'always return valid JSON matching the schema'
B) Increase the sampling temperature so the model has more flexibility to self-correct JSON errors
C) Enable constrained/structured decoding (e.g., grammar- or JSON-schema-constrained generation) so output tokens are restricted to only schema-conforming sequences
D) Post-process the model's free-text output with a regex to extract the closest JSON-like substring

---

**Q33.** In the context of agentic tool integration, 'least-privilege tool scoping' refers to which practice?

A) Granting every registered tool the same broad set of permissions so the agent never hits permission errors mid-task
B) Issuing the agent a single long-lived credential with admin-level access to all downstream systems for simplicity
C) Allowing the LLM to decide at runtime which permission level it needs and self-elevate access as tasks require
D) Granting each tool only the minimum set of scopes and data access required for its specific function, using short-lived, narrowly scoped credentials per tool or session

---

**Q34.** An agent's search tool calls a third-party API that enforces a strict rate limit and returns HTTP 429 with a Retry-After header when exceeded. During a burst of concurrent multi-agent tasks, the tool integration begins failing frequently. Which retry strategy BEST handles this scenario while remaining a good API citizen?

A) Immediately retry the failed call in a tight loop until it succeeds
B) Respect the Retry-After header when present, otherwise apply exponential backoff with jitter, and cap total retries with a circuit breaker to fail fast under sustained throttling
C) Treat every 429 as a permanent failure and surface it directly to the end user with no retry attempt
D) Poll the API every second regardless of the Retry-After value to check if the limit has reset

---

**Q35.** A production multi-agent system runs on Triton Inference Server with a TensorRT-LLM backend. During load testing, CUDA out-of-memory errors appear once concurrency crosses roughly 40 simultaneous agent sessions, even though aggregate GPU memory usage reported by nvidia-smi is well below capacity. In-flight batching is enabled and requests have highly variable output lengths. What is the MOST likely root cause and fix?

A) Switch from in-flight batching to static batching so memory allocation per batch becomes predictable and fixed
B) Raise max_batch_size further so the scheduler can pack more requests into each batch and reduce per-request overhead
C) Memory is fragmented because KV cache blocks are allocated as static contiguous reservations per sequence; enable paged KV cache management and tune kv_cache_free_gpu_mem_fraction to allow dynamic block allocation across sequences
D) Disable KV cache reuse and disable in-flight batching entirely to force one sequence to fully complete before the next begins

---

**Q36.** An agentic pipeline uses a router LLM, a retrieval reranker model, and a code-generation LLM, each with different latency/throughput profiles and independent release cadences. The team needs each model to scale, version, and roll back independently with minimal shared operational surface area. Which deployment approach best satisfies these requirements?

A) Combine all three models into a single custom TensorRT-LLM engine file so one process serves all agent needs
B) Load all three as manually configured Triton Python backends sharing one GPU memory pool for simplicity
C) Deploy all three models inside one Triton ensemble so the full pipeline executes as a single scheduled unit
D) Package each model as its own NIM microservice with its own TensorRT-LLM engine and container, called independently by the agent orchestration layer

---

**Q37.** An agent pipeline invokes NeMo Guardrails with three sequential rail checks (topical, jailbreak, moderation) on the final LLM output before returning a response to the user. This adds 300ms of latency and causes the pipeline to miss an 800ms end-to-end SLA. Safety coverage cannot be reduced. What is the BEST way to reduce added latency?

A) Remove the output rails and depend only on input rails, since that halves the guardrails latency
B) Move all guardrail checks to an asynchronous nightly batch audit job run against logged conversations
C) Increase the LLM's inference batch size so the guardrail model calls execute faster as part of the same batch
D) Run the independent rail checks concurrently as parallel async calls, gating the final response only on the checks that must block output, rather than executing them strictly in sequence

---

**Q38.** A team must deploy a 70B-parameter model on a single 80GB Hopper-class GPU using TensorRT-LLM. The agent frequently emits structured JSON tool-call arguments where small numeric precision errors break downstream parsing, so accuracy must be preserved as much as possible while still fitting weights and KV cache in memory. Which quantization configuration BEST balances memory footprint and output accuracy for this use case?

A) INT8 SmoothQuant with per-channel weight-only quantization and activations left in FP16
B) Full FP32 precision combined with tensor parallelism across GPU compute streams
C) INT4 AWQ weight-only quantization
D) FP8 weight and activation quantization using Hopper's native FP8 tensor cores

---

**Q39.** A multi-agent system spans a planner agent, several tool-calling sub-agents, and a NeMo Retriever call, each deployed as a separate NIM/Triton endpoint. During an incident, engineers need to reconstruct the full call graph, per-hop latency, and per-step cost attribution across all agent hops. Which approach aligns with recommended production observability practice for this scenario?

A) Instrument each agent step and NIM/Triton call with OpenTelemetry spans that propagate a shared trace/correlation ID across services, exported to a tracing backend that reconstructs the full step-by-step agent trace
B) Rely solely on Triton's built-in Prometheus GPU utilization metrics endpoint to diagnose the incident
C) Enable verbose stdout logging in each container and manually correlate log lines by timestamp across pods
D) Poll each NIM microservice's health check endpoint every 5 seconds to detect which service is failing

---

**Q40.** An agent occasionally produces different tool-call sequences for identical repeated input prompts even though temperature is set to 0 (greedy decoding) on a TensorRT-LLM-served model with in-flight batching enabled. What is the MOST likely root cause of this residual non-determinism?

A) The retrieval step returns different documents each run due to embedding drift in the vector index
B) The random sampling seed was never explicitly fixed in the client request
C) Differing batch composition under in-flight batching changes floating-point accumulation order in batched matrix operations, occasionally flipping a near-tied greedy argmax decision
D) The agent framework's conversation memory buffer randomly reorders prior turns between runs

---

**Q41.** A multi-agent system has a 'Researcher' agent with read-only web search access and an 'Executor' agent with access to a payment API and file-system write tool. During testing, a malicious prompt injected via a scraped webpage causes the Researcher agent to pass instructions that trick the Executor agent into calling the payment API to transfer funds to an unauthorized account. Which control would MOST effectively prevent this class of attack?

A) Enforce per-agent tool permission boundaries so the Researcher agent cannot invoke the payment API directly or indirectly, and require human approval for any payment execution above a defined threshold
B) Increase the temperature setting on the Researcher agent's LLM to reduce prompt injection susceptibility
C) Add a longer system prompt instructing the Executor agent to ignore instructions from external content
D) Increase the context window size so the Executor agent can better distinguish user instructions from injected content

---

**Q42.** A company deploys a customer support agent that must never discuss company financial performance, ongoing litigation, or political topics, even if directly asked by a user. Which guardrail category should be configured to enforce this restriction on the conversation subject matter?

A) Dialog rail
B) Topical rail
C) Execution rail
D) Retrieval rail

---

**Q43.** In a NeMo Guardrails Colang configuration, which flow structure correctly implements a human-in-the-loop approval gate before a high-risk 'execute_refund' action runs?

A) define flow refund
  user request refund
  bot execute refund
B) define flow refund
  user request refund
  execute execute_refund
  bot confirm refund
C) define flow refund
  user request refund
  bot ask human approval
  when human approves
    bot execute refund
  else
    bot decline refund
D) define flow refund
  user request refund
  bot execute refund
  bot ask human approval

---

**Q44.** For evaluating a multi-agent workflow that must book a flight, reserve a hotel, and send a confirmation email in the correct sequence, why is trajectory-based evaluation preferred over pure task-completion (outcome-only) evaluation?

A) Trajectory evaluation is cheaper to compute because it doesn't require an LLM judge
B) Trajectory evaluation eliminates the need for regression testing across model versions
C) Task-completion evaluation cannot be automated and requires manual grading for every run
D) Task-completion metrics alone cannot detect cases where the agent reached the final outcome via an unsafe, inefficient, or policy-violating sequence of intermediate steps

---

**Q45.** A team runs regression tests on an agentic pipeline after upgrading the underlying LLM. The same input now produces slightly different but semantically equivalent natural-language responses and tool-call orderings across runs due to sampling. The old regression suite uses exact-string-match assertions and now fails on roughly 40% of test cases despite no functional degradation. What is the BEST fix for the regression suite?

A) Set temperature to 0 for the production agent to guarantee deterministic outputs and keep exact-match assertions
B) Replace exact-match assertions with semantic-similarity or LLM-as-judge scoring plus tolerance for equivalent valid tool-call sequences, and run each test multiple times to establish a pass-rate threshold
C) Disable regression testing for this pipeline since non-determinism makes it inherently untestable
D) Downgrade back to the previous LLM version until deterministic outputs can be guaranteed

---

