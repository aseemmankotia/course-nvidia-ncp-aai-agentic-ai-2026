## Chapter 4: Orchestration Frameworks and State Management — Practice Questions

**Q1.** A team must build an agent workflow with explicit human-in-the-loop approval gates, conditional branching based on prior outputs, and the ability to checkpoint state and resume after a crash. Which framework BEST fits?

A) CrewAI with the Sequential process
B) AutoGen with a basic GroupChat
C) NeMo Agent (AIQ) toolkit used standalone
D) LangGraph

<details>
<summary>Answer</summary>

**Correct: D**

LangGraph models workflows as an explicit StateGraph of nodes/edges with a persistent shared state object and built-in checkpointing, which directly supports pause/resume and conditional human-in-the-loop steps.

**Why the others are wrong:**
- CrewAI Sequential runs tasks in a fixed order and doesn't expose fine-grained conditional state transitions or checkpointing.
- AutoGen's basic GroupChat is conversation-driven, not a graph with resumable checkpoints.
- The NeMo Agent (AIQ) toolkit is a framework-agnostic orchestration/observability wrapper, not itself a state-machine engine.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A team wants a Researcher, Coder, and Reviewer agent to freely converse and iterate on a coding problem until they reach consensus, with minimal upfront workflow structure. Which framework fits this pattern best?

A) AutoGen
B) LangGraph
C) CrewAI with the Hierarchical process
D) NeMo Agent (AIQ) toolkit

<details>
<summary>Answer</summary>

**Correct: A**

AutoGen's GroupChat/GroupChatManager pattern is purpose-built for flexible, conversational multi-agent interaction where agents talk until a stopping condition is met.

**Why the others are wrong:**
- LangGraph would require defining an explicit graph up front, adding unnecessary structure for open-ended dialogue.
- CrewAI Hierarchical enforces a manager-delegation structure, not free-form peer conversation.
- The NeMo Agent (AIQ) toolkit is an orchestration/observability layer, not a conversational engine itself.

</details>

---

**Q3.** An enterprise has agents built independently in LangChain, LlamaIndex, and CrewAI and needs unified profiling, evaluation, and observability before production rollout. What should they adopt?

A) Rewrite everything in LangGraph
B) AutoGen's GroupChatManager as the single entry point
C) NVIDIA NeMo Agent (AIQ) toolkit as a framework-agnostic wrapper
D) CrewAI's Hierarchical process to unify all agents

<details>
<summary>Answer</summary>

**Correct: C**

The NVIDIA NeMo Agent toolkit (now AIQ toolkit), part of NVIDIA AI Blueprints, is designed to sit above existing agent frameworks and add profiling, evaluation, and observability without requiring a rewrite.

**Why the others are wrong:**
- Rewriting in LangGraph discards existing working agents and adds unnecessary migration cost.
- AutoGen's GroupChatManager only manages AutoGen-native conversational agents, not agents from other frameworks.
- CrewAI's Hierarchical process only coordinates CrewAI agents, not cross-framework agents.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which framework is optimized for defining a role-based crew (e.g., Researcher, Writer, Editor) executing tasks in a defined order with minimal custom orchestration code?

A) LangGraph
B) CrewAI
C) AutoGen
D) Raw manual prompt chaining

<details>
<summary>Answer</summary>

**Correct: B**

CrewAI abstracts orchestration into role-based agents and Sequential/Hierarchical processes, letting teams define crews with minimal boilerplate.

**Why the others are wrong:**
- LangGraph requires explicitly defining the graph, nodes, and edges — more control but more code.
- AutoGen is oriented around conversational patterns, not fixed role-sequenced task execution.
- Raw prompt chaining lacks any structured orchestration abstraction, requiring the most custom code.

</details>

---

**Q5.** A workflow requires cyclic execution, conditional branching, and a shared mutable state object explicitly passed between processing steps. Which framework provides this level of control?

A) AutoGen
B) LangGraph
C) CrewAI
D) NeMo Agent (AIQ) toolkit

<details>
<summary>Answer</summary>

**Correct: B**

LangGraph's StateGraph explicitly models cycles, conditional edges, and a shared state schema passed between nodes, giving the highest control granularity of the listed options.

**Why the others are wrong:**
- AutoGen manages conversational turns, not an explicit graph of state transitions.
- CrewAI offers only Sequential/Hierarchical process templates, not arbitrary cyclic graphs.
- The NeMo Agent (AIQ) toolkit orchestrates and observes agents built in other frameworks; it isn't itself a graph state engine.

</details>

---

**Q6.** A LangGraph agent workflow never reaches an END node and keeps looping. What is the FIRST thing to check/fix?

A) Increase the AutoGen max_round parameter
B) Verify the recursion_limit and add/fix an explicit conditional edge that routes to END
C) Reduce the number of CrewAI agents in the crew
D) Disable the agent's memory entirely

<details>
<summary>Answer</summary>

**Correct: B**

LangGraph's recursion_limit (default 25) acts as a safety net that raises a GraphRecursionError, but the real fix for a runaway loop is ensuring a conditional edge actually routes execution to END under the intended condition.

**Why the others are wrong:**
- max_round is an AutoGen GroupChat parameter and doesn't apply to LangGraph.
- Reducing CrewAI agents is irrelevant since the scenario is a LangGraph workflow.
- Disabling memory doesn't address a missing or broken termination condition and may break the workflow further.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** In AutoGen, which parameter directly caps the total number of conversational turns in a GroupChat to prevent runaway agent-to-agent dialogue?

A) recursion_limit
B) max_iter
C) max_round
D) checkpoint_interval

<details>
<summary>Answer</summary>

**Correct: C**

max_round on AutoGen's GroupChat sets the ceiling on total conversation rounds among participating agents, directly bounding runaway conversations.

**Why the others are wrong:**
- recursion_limit is a LangGraph-specific parameter for graph super-steps, not AutoGen turns.
- max_iter is associated with CrewAI agent task iterations, not AutoGen group conversation rounds.
- checkpoint_interval is not a real termination-control parameter in any of these frameworks.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the primary architectural difference between LangGraph and CrewAI?

A) LangGraph models workflows as an explicit graph of nodes/edges with shared state; CrewAI abstracts orchestration into role-based agents with Sequential/Hierarchical processes
B) LangGraph only supports a single agent while CrewAI is the only framework supporting multiple agents
C) CrewAI requires a GPU cluster to run while LangGraph runs only on CPU
D) AutoGen and LangGraph share the same underlying execution engine, while CrewAI is unrelated

<details>
<summary>Answer</summary>

**Correct: A**

LangGraph's core abstraction is an explicit state graph (nodes, edges, shared state), giving fine-grained control, while CrewAI's core abstraction is role-based agents coordinated via Sequential or Hierarchical processes, trading control for simplicity.

**Why the others are wrong:**
- Both frameworks support multi-agent systems; this is not a distinguishing factor.
- Neither framework has a GPU/CPU-only requirement tied to its architecture — that depends on the underlying model, not the orchestration framework.
- AutoGen, LangGraph, and CrewAI are independent frameworks with different execution engines.

</details>

---

