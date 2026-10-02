# Practice Test 1: NVIDIA NCP-AAI: Agentic AI Certification Prep

- **Questions:** 45
- **Time limit:** 90 minutes
- **Passing score:** 70%%

> Answers are in `answer-key-1.md`. Track results in `score-tracker.md`.

---

**Q1.** An agent must answer a question requiring current stock prices. It needs to alternate between generating a thought, invoking a price-lookup tool, observing the result, and refining its next thought before producing a final answer. Which reasoning paradigm is being implemented?

A) Tree-of-thought
B) Self-consistency decoding
C) Retrieval-augmented generation with no tool loop
D) ReAct (Reason+Act)

---

**Q2.** A team is automating a multi-step data migration workflow where all steps and their order are known in advance, but any individual step can fail and require the entire remaining plan to be recomputed with the new state. Which agent control pattern should they implement?

A) A purely reactive agent that decides the next single action based only on the latest observation
B) A single chain-of-thought prompt with no execution loop
C) A plan-and-execute agent that generates a full plan upfront and re-plans when execution deviates from expectations
D) A tree-of-thought agent that only explores plans without ever executing actions

---

**Q3.** How does tree-of-thought (ToT) reasoning fundamentally differ from standard chain-of-thought (CoT) reasoning?

A) ToT requires a fine-tuned model while CoT works only with prompting
B) ToT eliminates the need for intermediate reasoning steps entirely
C) ToT generates and evaluates multiple candidate reasoning paths in a branching search, allowing backtracking, whereas CoT follows a single linear reasoning chain
D) ToT is only applicable to code-generation agents, while CoT applies to all agent types

---

**Q4.** When designing a function/tool-calling schema for an agent so that a downstream NIM-hosted LLM reliably invokes the correct tool with valid parameters, which schema design practice is most important?

A) Provide the tool as unstructured free-text documentation in the system prompt instead of a schema
B) Define each tool's parameters using a strict JSON schema with explicit types and a marked list of required fields, and give the tool a precise name and description
C) Let the model infer parameter names and types at inference time without a predefined schema
D) Combine all available tools into a single generic function to reduce schema complexity

---

**Q5.** A company needs an assistant that reviews contracts by independently analyzing financial risk, legal compliance, and technical feasibility of clauses, then produces a consolidated recommendation with disagreement flagged between the three perspectives. Which architecture BEST fits this requirement?

A) A single agent with one generic system prompt covering all three domains
B) A multi-agent system with distinct finance, legal, and technical personas plus an orchestrator/aggregator agent that reconciles their outputs
C) A single chain-of-thought prompt asked to produce three separate paragraphs
D) A reactive single agent that randomly samples one domain perspective per request

---

**Q6.** An agent authorizes financial transactions above a certain dollar threshold. The design team wants to minimize the risk of the agent taking an unintended or unsafe action for these high-value transfers. Which approach best balances agent capability with safety for this specific action?

A) Constrain the agent to a deterministic, allow-listed transaction tool with hard validation rules and human approval gating for transfers above the threshold, while allowing more autonomous reasoning for low-value transfers
B) Give the agent full autonomous freedom to choose any available tool and parameters for every transaction size
C) Remove all reasoning and let the agent execute transactions purely on a fixed schedule with no LLM involvement
D) Increase the model's temperature for high-value transactions to encourage more creative decision-making

---

**Q7.** A research agent must solve a complex multi-step puzzle where several candidate solution paths need to be explored in parallel, evaluated, and abandoned (backtracked) when they lead to dead ends before a final answer is chosen. Which reasoning paradigm BEST fits this requirement?

A) Chain-of-Thought reasoning
B) ReAct (Reason+Act) loop
C) Tree-of-Thought reasoning
D) Plan-and-execute with a single upfront plan

---

**Q8.** A financial services agent processes wire transfers. Company policy requires that any transfer above $10,000 be paused for human approval, while transfers below that threshold should be completed automatically without delay. Which agent control design BEST satisfies this requirement?

A) A deterministic workflow with a conditional human-in-the-loop gate triggered by the transaction amount threshold
B) A fully autonomous agent with no constraints, relying on its own judgment for every transfer amount
C) A multi-agent debate system where several agents vote on whether to approve each transfer
D) A self-reflection loop that reviews and audits all transfers only after they have already been completed

---

**Q9.** When designing a tool/function-calling schema for an agent that must reliably invoke a get_weather(location, unit) function, which schema design choice minimizes malformed or invalid calls?

A) Describe the parameters only in a free-text natural language sentence within the system prompt
B) Pass all parameters concatenated into a single unstructured string argument
C) Let the agent invent whatever parameter names it thinks are appropriate on each call
D) Define a strict JSON schema with typed, required parameters and an enum constraint on the unit field (e.g., "celsius"/"fahrenheit")

---

**Q10.** A workflow requires drafting a technical document, then having an independent perspective check the draft for factual accuracy and tone before producing the final output. Which architecture is the recommended approach for this requirement?

A) A multi-agent pattern with a dedicated writer agent and a separate critic agent operating in a reflection loop
B) A single agent with one system prompt that drafts and reviews the document in the same pass
C) A Tree-of-Thought approach that only generates multiple draft branches without any review step
D) A plan-and-execute agent that generates a document outline and executes it with no reflection step

---

**Q11.** An insurance company is building a claims-processing agentic system. A central agent receives each claim, determines which specialized agents are needed (document verification, fraud detection, payout calculation), sends work to them, then combines their outputs into a final decision. Which orchestration pattern does this describe?

A) Sequential pipeline pattern, where each agent runs in a fixed order on every claim
B) Debate/critique pattern, where two agents iteratively challenge each other's conclusions
C) Fully decentralized peer-to-peer handoff, where agents pass the claim directly to each other with no central coordinator
D) Supervisor/worker pattern, where a central agent dynamically delegates and aggregates results from specialized worker agents

---

**Q12.** A workflow requires conditional branching, multiple retry loops around failing sub-tasks, and the ability to persist and resume execution state after a crash. Which orchestration framework is BEST suited to implement this as an explicit stateful graph?

A) CrewAI, using its default sequential process
B) LangGraph, using conditional edges and a built-in checkpointer for persisted state
C) AutoGen, using a group chat manager for agent conversations
D) NVIDIA NeMo Agent toolkit (AgentIQ) used as the primary graph engine

---

**Q13.** A marketing team wants a writer agent to draft content and a critic agent to review it, with the draft revised repeatedly until the critic approves it or a maximum of 4 rounds is reached. Which pattern, plus safeguard, correctly matches this design?

A) Debate/critique pattern with a bounded iteration count acting as the termination condition
B) Hierarchical pattern with three levels of supervisor agents
C) Sequential pattern with a single fixed pass through both agents and no iteration
D) Supervisor/worker pattern with the supervisor performing all writing and reviewing itself

---

**Q14.** You are invoking a LangGraph graph that contains a cycle for retrying a failing tool call. To prevent the graph from looping indefinitely if the tool keeps failing, which configuration should you set on the graph invocation?

A) Increase the LLM's max_tokens parameter so each node can produce longer output
B) Disable the graph's checkpointer so state is not persisted between steps
C) Raise the LLM's temperature to encourage the agent to find a different tool path
D) Set the recursion_limit in the invocation config to cap the number of graph super-steps

---

**Q15.** A team has agents built across LangGraph and CrewAI, deployed on NVIDIA infrastructure, and wants centralized tracing, profiling, and evaluation across all of them without rewriting the agents in a single framework. Which NVIDIA tool is designed for this?

A) AutoGen's group chat manager
B) NVIDIA NeMo Agent toolkit (formerly AgentIQ)
C) CrewAI's hierarchical process
D) LangGraph's checkpointer

---

**Q16.** Which statement correctly distinguishes the hierarchical multi-agent pattern from the sequential pattern?

A) Sequential patterns allow dynamic delegation across multiple management tiers, while hierarchical patterns strictly enforce a fixed order of execution
B) Hierarchical and sequential patterns are functionally identical, differing only in the naming of the agents involved
C) Hierarchical patterns involve multiple levels of supervisor agents that dynamically delegate tasks based on runtime decisions, while sequential patterns execute agents in a fixed, predetermined order regardless of intermediate results
D) Sequential patterns require a central supervisor agent, whereas hierarchical patterns eliminate any supervisory agent entirely

---

**Q17.** A legal-tech company is designing a multi-agent contract review workflow: one agent drafts a redline summary, a second agent critiques it against a compliance checklist, and the draft agent revises based on the critique. This should repeat until the critique agent scores the draft above a quality threshold or a maximum of 5 rounds is reached. Which orchestration pattern and control mechanism BEST fits this requirement?

A) A debate/critique pattern with a generator and critic agent iterating in a loop, gated by a quality-threshold or max-iteration termination condition
B) A sequential pipeline where each agent runs exactly once in a fixed order with no feedback loop
C) A supervisor/worker pattern that decomposes the task into independent, parallel sub-tasks assigned to separate workers
D) A single agent using a large context window to draft and self-check the document in one pass

---

**Q18.** You are architecting a multi-agent system that requires complex conditional branching between agents, cycles (an agent may need to be revisited multiple times based on evolving state), and fine-grained programmatic control over state transitions and routing logic. Which orchestration framework is BEST suited to this requirement?

A) LangGraph, using an explicit graph with typed state and conditional edges supporting cycles
B) CrewAI, using its role-based sequential or hierarchical process abstraction
C) AutoGen, using a GroupChat with a fixed speaker order
D) NeMo Agent toolkit (AgentIQ) used as a standalone control-flow graph engine

---

**Q19.** You built a multi-agent debate system using AutoGen's GroupChat with three agents. During testing, the agents keep responding to each other indefinitely without reaching consensus or ending the conversation, consuming excessive tokens. Which configuration change directly addresses this runaway loop?

A) Increase the temperature setting on each agent to encourage faster convergence
B) Add more agents to the GroupChat so consensus is reached through majority voting
C) Set the max_round parameter on the GroupChatManager and define an is_termination_msg function to detect a stopping condition
D) Switch from GroupChat to a single stateless chat-completion call per agent turn

---

**Q20.** In a multi-agent system, two coordination mechanisms are being compared: (1) direct message passing between named agents, and (2) a shared blackboard/state store that any agent can read from and write to. What is the key architectural difference between these two approaches?

A) Message passing can only be used with hierarchical topologies, while shared state only works with fully sequential pipelines
B) Shared blackboard state requires every agent to run on the same GPU, while message passing works across distributed nodes
C) Message passing requires point-to-point addressing between specific sender and receiver agents, while a shared blackboard allows any agent to read/write common state without direct addressing
D) Message passing is stateless by definition, so agents using it cannot retain any conversation history across turns

---

**Q21.** An agent orchestrated with NVIDIA NeMo Agent toolkit must complete a 40-step data migration workflow. After step 25, the agent begins optimizing for a sub-task that contradicts the original migration goal stated in the initial prompt. Which architectural change BEST prevents this drift?

A) Increase the LLM's temperature to encourage more creative problem-solving on later steps
B) Remove the system prompt after the first 10 steps to reduce token usage
C) Restart the entire workflow from step 1 whenever a sub-task is completed
D) Maintain an explicit, persistent plan/goal-state object that the planner re-validates each step against the original objective

---

**Q22.** In an agentic memory architecture, which statement correctly distinguishes episodic memory from semantic memory?

A) Episodic memory stores generalized domain facts, while semantic memory stores timestamped records of specific past interactions
B) Episodic and semantic memory are interchangeable terms for the same short-term context buffer
C) Semantic memory only exists in the LLM's context window, while episodic memory only exists in the vector database
D) Episodic memory stores specific past experiences/interactions with temporal context, while semantic memory stores generalized, decontextualized facts and knowledge

---

**Q23.** You are configuring a RAG pipeline using NeMo Retriever embedding models over a corpus of long, deeply nested technical manuals with numbered procedural steps. Which chunking configuration is MOST appropriate?

A) Fixed 4096-token chunks with no overlap to maximize embedding batch throughput
B) Semantic-boundary-aware chunking with modest overlap (e.g., 10-15%) that preserves procedural step groupings
C) Splitting strictly by whitespace every 50 tokens regardless of sentence or section boundaries
D) One embedding per entire document to minimize the number of vectors stored

---

**Q24.** A company's agent must answer highly specific domain queries against a corpus containing rare technical acronyms that generic embeddings handle poorly. Which retrieval configuration BEST improves answer precision?

A) Use a hybrid retriever combining dense embeddings with sparse keyword search (e.g., BM25), followed by a NeMo Retriever reranking model on the combined candidates
B) Use only a dense embedding retriever with a larger top-k to compensate for weak matches
C) Disable retrieval entirely and rely on the LLM's parametric knowledge for acronym resolution
D) Use dense embeddings only, but reduce top-k to 1 to force higher confidence

---

**Q25.** An agent must reference a 500,000-token internal knowledge base to answer each user query. Which approach BEST balances cost and latency while maintaining answer quality?

A) Concatenate the entire 500,000-token knowledge base into the context window on every request
B) Use RAG to retrieve only the top-k most relevant chunks per query and inject those into the context window
C) Fine-tune a new model on the knowledge base weekly and remove retrieval entirely
D) Truncate the knowledge base to the first 8,000 tokens and ignore the rest

---

**Q26.** A multi-day customer-support agent needs to recall the specific resolution steps used for a particular customer's ticket three weeks ago when that customer contacts support again. Which memory strategy is MOST appropriate?

A) Rely solely on the current session's context window, since it always contains the full conversation history
B) Store conversation summaries and key resolution details in a long-term vector-indexed memory store, retrieved by customer/session identifiers at query time
C) Increase the model's context window size so the entire support history for all customers fits in every prompt
D) Discard all prior interactions and treat every new contact as a first-time conversation

---

**Q27.** An agentic system executes a customer-support workflow that can run for several thousand turns. The underlying LLM has a 128K-token context window, and simply appending every turn causes the agent to exceed the window and lose track of the original goal after a few hundred turns. Which memory strategy BEST keeps the agent grounded in its original objective while staying within context limits?

A) Store the full verbatim transcript in the prompt every turn and truncate the oldest tokens once the window fills
B) Discard all prior turns at the start of each new turn so the prompt only contains the current user message
C) Periodically summarize older turns into a compact memory representation, retain only recent turns verbatim, and inject the summary alongside the working context
D) Rely exclusively on a vector database for all context and remove the working/short-term memory buffer entirely

---

**Q28.** An agent must ground its tool-calling decisions in a large, heterogeneous internal knowledge base containing both precise technical identifiers (part numbers, error codes) and free-form explanatory text. Precision of retrieved passages directly affects which tool the agent invokes. Which retrieval configuration BEST maximizes grounding accuracy in this scenario?

A) Dense embedding-based vector search only, using the top-k nearest neighbors as retrieved context
B) Combine dense and sparse (keyword/BM25) retrieval, then apply a cross-encoder re-ranker to the merged candidate set before passing results to the agent
C) Expand the context window enough to include the entire knowledge base in every prompt instead of retrieving
D) Sparse keyword (BM25) search only, returning the top-k lexical matches as retrieved context

---

**Q29.** An agent orchestrates payments through a third-party billing API tool. During a network blip, the agent times out waiting for a response and automatically retries the same tool call. The billing API has no built-in duplicate detection. Which design change BEST prevents a duplicate charge from being processed?

A) Increase the tool call timeout so retries happen less frequently
B) Disable retries entirely and surface every timeout as a failed turn to the user
C) Have the agent generate and pass a unique idempotency key with each logical request, and configure the tool wrapper to reuse the same key across retries of that request
D) Log every tool call to a database and have a human review logs nightly for duplicates

---

**Q30.** A tool exposed to an agent allows arbitrary Python code execution to help with data analysis tasks. Which implementation BEST satisfies safe tool execution requirements for this capability?

A) Run the code directly in the same process as the agent orchestrator for lowest latency
B) Require the user to manually approve each line of code before execution
C) Only allow the code execution tool to be called once per conversation to limit exposure
D) Execute the code in an ephemeral, network-restricted container with CPU/memory/time limits and no access to the host filesystem or credentials

---

**Q31.** An engineering team needs a single agent to discover and invoke tools across several heterogeneous systems — a SQL database, an internal ticketing REST API, and a local filesystem — using a consistent, standardized interface rather than writing bespoke integration code for each system. Which connector pattern BEST fits this requirement?

A) A single monolithic prompt that lists all three systems' credentials for the LLM to reason about directly
B) Direct SDK calls embedded in the system prompt with no abstraction layer
C) Model Context Protocol (MCP), with each system exposed behind an MCP server that the agent's MCP client/host connects to
D) A shared relational database that all three systems write their state into

---

**Q32.** During testing, an LLM occasionally emits a tool call with a string value in a field the schema declares as an integer, causing the downstream function to throw an unhandled exception. Which fix BEST addresses this class of failure in a production agentic system?

A) Validate the LLM-generated arguments against the tool's JSON Schema before invocation, and return a structured validation error back into the reasoning loop for the agent to correct
B) Increase the model's temperature so it produces more varied tool call formats
C) Remove type constraints from the tool schema so any value is accepted
D) Catch the exception at the top level and silently ignore the failed tool call

---

**Q33.** A tool wraps a third-party weather API that enforces a hard limit of 60 requests per minute per API key. Multiple agent sessions share the same key. Which configuration BEST prevents the shared key from being throttled or banned due to burst traffic from concurrent agent sessions?

A) Give each agent session its own retry loop with no coordination, relying on the API to queue excess requests
B) Cache all responses indefinitely regardless of data freshness requirements
C) Randomly drop tool calls from sessions that have already made a request that minute
D) Implement a centralized rate limiter (e.g., token bucket) shared across all sessions using the key, capping outbound calls below the provider's 60/minute limit with queuing or backoff for excess requests

---

**Q34.** In agent architecture terminology, what is the key distinction between 'function calling' and the broader concept of 'tool use'?

A) Function calling only works with NVIDIA NIM microservices, whereas tool use works with any inference backend
B) Function calling is the structured mechanism (schema-defined name, parameters, and return format) by which a model requests and receives a specific function's output within its context; tool use is the broader umbrella that includes function calling plus other agent capabilities like retrieval, memory access, and multi-step planning around tool invocation
C) Function calling requires human approval for every invocation, whereas tool use never does
D) Function calling refers to any interaction an agent has with external systems, while tool use is a narrower term reserved only for read-only actions

---

**Q35.** A team is deploying an open-weights LLM for a customer-support agent that must sustain sub-200ms per-token latency under high concurrent load. Which deployment approach BEST meets this requirement?

A) Deploy the model as a NIM microservice, which applies TensorRT-LLM optimizations and in-flight batching under Triton
B) Serve the model directly from a custom Flask app running PyTorch in eager mode
C) Rely on NeMo Guardrails alone to filter and speed up responses
D) Deploy the model on CPU nodes using ONNX Runtime for portability

---

**Q36.** Which NVIDIA component is specifically responsible for compiling and optimizing an LLM's transformer layers — fusing kernels and applying quantization — for maximum GPU inference throughput?

A) NeMo Guardrails
B) Triton Inference Server
C) NeMo Retriever
D) TensorRT-LLM

---

**Q37.** You are tuning a TensorRT-LLM deployment of a 70B-parameter model on a multi-GPU node and need to reduce GPU memory pressure from long-context KV caches without significantly hurting throughput. Which technique should you enable?

A) Paged KV cache (paged attention) with block-based memory management
B) Increase max_batch_size without any upper bound
C) Disable in-flight batching entirely
D) Switch all layers to FP32 precision

---

**Q38.** An agentic assistant occasionally discusses competitor pricing, violating company policy. The team wants to enforce a topical restriction at runtime without retraining the underlying LLM. Which approach BEST solves this?

A) Add a topical rail defined in Colang within NeMo Guardrails to detect and block off-topic requests
B) Fine-tune the LLM so it never mentions competitors
C) Increase the sampling temperature to reduce the chance of the topic coming up
D) Add more few-shot examples to the system prompt as the only safeguard

---

**Q39.** In the context of TensorRT-LLM serving, what is the key advantage of in-flight (continuous) batching over static batching for LLM inference?

A) It eliminates the need for a KV cache entirely
B) It requires every sequence in a batch to have an identical output length
C) It only functions correctly on single-GPU deployments
D) It allows new requests to join a batch and completed requests to leave without waiting for the entire batch to finish, improving GPU utilization

---

**Q40.** A multi-agent production system occasionally loops indefinitely or invokes the wrong tool, but the behavior is intermittent and hard to reproduce. Which approach BEST enables the team to diagnose the root cause?

A) Instrument the agent pipeline with step-level tracing capturing each agent's decisions, tool calls, tokens, and latency to reconstruct the full execution trace
B) Add ad-hoc print statements and redeploy, hoping to catch the issue again
C) Rely solely on the LLM's final output logs stored after each session
D) Increase the model's temperature setting to make behavior more consistent

---

**Q41.** A multi-agent system autonomously initiates financial trades based on market analysis. The risk team requires that no trade above $50,000 executes without explicit human authorization, while smaller trades can proceed autonomously. Which control BEST meets this requirement?

A) Add an output content filter that blocks messages mentioning trade amounts over $50,000
B) Insert a human-in-the-loop approval gate on the trade-execution tool call that pauses the agent and requires sign-off when the trade value exceeds the threshold
C) Use an LLM-as-judge to score the trade recommendation for quality before logging it
D) Add a topical rail restricting the agent's conversation to finance-related subjects only

---

**Q42.** Before releasing an agentic system to production, a security team wants to automatically probe the deployed LLM and its tool-use pipeline for susceptibility to prompt injection, jailbreaks, and data leakage, generating a vulnerability report. Which NVIDIA tool is purpose-built for this task?

A) NVIDIA Triton Inference Server
B) NeMo Guardrails runtime rails
C) NVIDIA NIM microservice
D) NVIDIA garak LLM vulnerability scanner

---

**Q43.** An engineering team is configuring NeMo Guardrails for an agent that can call external APIs (e.g., send_email, delete_file, transfer_funds). They want to ensure the agent can only invoke a pre-approved allowlist of actions and cannot call arbitrary or unapproved tools, regardless of how the conversation is phrased. Which rail type should they configure?

A) Input rails
B) Retrieval rails
C) Execution rails
D) Dialog rails

---

**Q44.** A team evaluates a customer-support multi-agent workflow (retrieve documents, reason over them, call a ticketing API, respond to the user). Outputs vary slightly across runs due to model non-determinism, and the team needs to assess whether the agent took the correct sequence of intermediate actions to resolve tickets, not just whether the final reply looked reasonable. Which evaluation approach BEST fits this need?

A) Exact string match between the final response and a single golden answer
B) LLM-as-judge scoring each step of the recorded trajectory against a rubric that checks correct retrieval, reasoning, and tool-call order
C) A single BLEU score computed only on the final user-facing message
D) Manual human review of 100% of production traffic before every response is sent

---

**Q45.** During exam-style review, a candidate must distinguish task-completion metrics from trajectory metrics in agent evaluation. Which statement BEST describes a trajectory metric?

A) It measures only whether the agent's final output achieved the user's stated goal, regardless of the steps taken
B) It measures the latency of the agent's end-to-end response time in production
C) It measures whether the sequence of intermediate reasoning steps, tool calls, and decisions the agent took matches the expected or valid path to the goal
D) It measures the dollar cost of GPU inference consumed across all agent turns

---

