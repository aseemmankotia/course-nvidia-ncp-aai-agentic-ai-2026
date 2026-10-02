## Chapter 3: Multi-Agent Coordination Patterns — Practice Questions

**Q1.** A support platform receives an incoming ticket. A central agent classifies intent (billing, technical, refund), then forwards the ticket to the matching specialist agent, and receives the specialist's response before replying to the user. Every specialist reports back only to the central agent — specialists never talk to each other. Which coordination pattern is this?

A) Sequential pipeline
B) Collaborative debate/critique pattern
C) Peer-to-peer mesh pattern
D) Supervisor/worker pattern

<details>
<summary>Answer</summary>

**Correct: D**

A single central agent owns routing logic and delegates to specialist workers, aggregating their output — the defining trait of supervisor/worker.

**Why the others are wrong:**
- Sequential pipeline implies a fixed linear chain regardless of intent — there's no conditional routing decision here
- Debate/critique requires agents iteratively challenging each other's output, which doesn't happen — specialists never interact
- Peer-to-peer mesh has no central authority; here a supervisor clearly owns the decision

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A legal-document workflow decomposes a case into research, drafting, and compliance-review sub-tasks. Each of these three areas is itself managed by a mid-level agent that further delegates to 2-3 specialized worker agents (e.g., research splits into case-law search and precedent-check agents). Which pattern best matches this workflow?

A) Hierarchical multi-agent tree
B) Supervisor/worker pattern
C) Sequential pipeline
D) Collaborative debate/critique pattern

<details>
<summary>Answer</summary>

**Correct: A**

Multiple layers of delegation (top orchestrator → mid-level managers → leaf workers) form a tree of decomposed responsibility, which is the hierarchical pattern.

**Why the others are wrong:**
- Supervisor/worker is flat — one router and workers, not multiple management layers
- Sequential pipeline implies strict linear hand-off, not parallel branching sub-trees
- Debate/critique requires agents evaluating each other's output, not task decomposition

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A writer agent produces a draft blog post. A critic agent reviews it and returns specific objections. The writer revises based on the objections, and the cycle repeats up to 3 times before a final version is accepted. Which pattern describes this interaction?

A) Hierarchical multi-agent tree
B) Sequential pipeline
C) Collaborative debate/critique pattern
D) Supervisor/worker pattern

<details>
<summary>Answer</summary>

**Correct: C**

Iterative critique and revision between peer agents with feedback loops is the defining structure of the collaborative debate/critique pattern.

**Why the others are wrong:**
- Hierarchical implies decomposition into sub-agents, not iterative peer feedback
- Sequential pipeline is one-directional with no feedback loop back to an earlier agent
- Supervisor/worker requires a central router assigning discrete tasks, not mutual critique

</details>

---

**Q4.** Which coordination pattern is the correct choice for a workflow where Agent A always extracts data, Agent B always transforms it, and Agent C always loads it into a database, in that fixed order, with no branching or agent-to-agent negotiation?

A) Collaborative debate/critique pattern
B) Hierarchical multi-agent tree
C) Supervisor/worker pattern
D) Sequential pipeline

<details>
<summary>Answer</summary>

**Correct: D**

A fixed, unconditional linear chain of agent outputs feeding the next agent's input is a sequential pipeline — the simplest and lowest-overhead pattern.

**Why the others are wrong:**
- Debate/critique requires disagreement/feedback loops, not a straight-through ETL chain
- Hierarchical requires task decomposition across management layers, which isn't present
- Supervisor/worker implies conditional routing decisions; here the order never varies

</details>

---

**Q5.** Three agents collaborating on a research task all need to read and update a common object holding gathered facts, open questions, and confidence scores as the task progresses. Which mechanism should the design use?

A) Point-to-point message queues between every agent pair
B) Independent local memory per agent with no synchronization
C) A sequential pipeline with no shared object
D) A shared-state contract (blackboard-style shared object)

<details>
<summary>Answer</summary>

**Correct: D**

When multiple agents must read/write common evolving data, a shared-state (blackboard) contract avoids duplicated or stale local copies and is the standard pattern for this need.

**Why the others are wrong:**
- Pairwise queues scale poorly (O(n^2) links) and don't guarantee a consistent shared view of facts
- Independent local memory causes agents to work from stale or conflicting information
- Sequential pipeline doesn't fit — the scenario requires concurrent read/write access, not linear hand-off

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** When designing a handoff protocol between a routing agent and a specialist agent, which element is MOST important to include in the handoff payload to prevent the specialist from re-deriving context it doesn't need to?

A) The specialist agent's internal system prompt
B) Only the specialist agent's unique ID
C) A random correlation token with no task data
D) The full conversation history plus a structured summary of the task and completion criteria

<details>
<summary>Answer</summary>

**Correct: D**

A well-formed handoff includes enough context (history + task framing + success criteria) for the receiving agent to act without re-querying the user or supervisor.

**Why the others are wrong:**
- The specialist's own system prompt is internal configuration, not something the router needs to pass
- An ID alone gives no task context, forcing wasted round trips
- A token with no data provides traceability but no usable task context

</details>

---

**Q7.** An agent-to-agent message schema includes fields: sender, recipient, intent, payload, and correlation_id. Which field is specifically responsible for letting a supervisor match a worker's asynchronous response back to the original delegated task?

A) intent
B) correlation_id
C) payload
D) sender

<details>
<summary>Answer</summary>

**Correct: B**

correlation_id ties a response message to the specific outbound request/task it answers, which is essential when multiple tasks are in flight concurrently.

**Why the others are wrong:**
- intent describes what the message is asking for, not which prior task it maps to
- payload carries the data content, not the tracking reference
- sender only identifies who sent it, not which original task the reply belongs to

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the primary structural difference between the supervisor/worker pattern and the hierarchical multi-agent tree pattern?

A) Supervisor/worker never uses shared state; hierarchical always does
B) Supervisor/worker has one flat layer of delegation; hierarchical has multiple nested layers of delegation
C) Hierarchical trees cannot use tool-calling agents
D) There is no meaningful difference — they are interchangeable terms

<details>
<summary>Answer</summary>

**Correct: B**

Supervisor/worker is a single-level router-to-specialist relationship, while hierarchical trees stack multiple management layers, each delegating further down.

**Why the others are wrong:**
- Shared-state usage is independent of which topology is chosen — either can use it
- Tool-calling capability is unrelated to topology depth
- The patterns are distinct exam-tested concepts, not interchangeable

</details>

---

