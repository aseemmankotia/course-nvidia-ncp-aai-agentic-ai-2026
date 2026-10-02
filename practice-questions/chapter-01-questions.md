## Chapter 1: Anatomy of an LLM Agent — Practice Questions

**Q1.** A customer support agent needs to look up order status, check inventory, and issue a refund — each step's outcome determines what to do next (e.g., if inventory is out of stock, escalate instead of refunding). Which control pattern BEST fits?

A) Plan-and-execute: generate a full plan upfront, then execute all steps in sequence
B) Tree-of-thought: explore multiple reasoning branches in parallel and vote on the best
C) ReAct: interleave reasoning and tool calls, observing each result before deciding the next action
D) A single chain-of-thought prompt with no tool calls

<details>
<summary>Answer</summary>

**Correct: C**

The workflow needs dynamic branching based on intermediate observations — exactly what ReAct provides by pairing each thought with a tool call and result.

**Why the others are wrong:**
- Plan-and-execute commits to a full plan before seeing results, a poor fit when the next step depends on an observation
- Tree-of-thought explores parallel reasoning branches for hard problems, not sequential tool-dependent workflows
- No tool calls means the agent can't check real order/inventory data at all

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A team is building an agent that answers policy questions using one well-documented knowledge base and two tools (search, calculator). They propose a 5-agent architecture (planner, retriever, calculator agent, critic, summarizer). What is the BEST recommendation?

A) Approve the 5-agent design because more agents increase accuracy
B) Add a 6th orchestrator agent to manage the other 5
C) Recommend a single agent with both tools bound directly, since scope is narrow and coordination need is low
D) Split retrieval into two separate agents for redundancy

<details>
<summary>Answer</summary>

**Correct: C**

A single well-tooled agent outperforms a multi-agent team when the task scope is narrow, tools are few, and there is no need for parallel specialized reasoning.

**Why the others are wrong:**
- More agents add latency, cost, and coordination failure points without added benefit here
- Adding an orchestrator compounds unnecessary complexity for a simple task
- Redundant retrieval agents don't solve a coordination problem that doesn't exist

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** An agent is authorized to autonomously draft and send internal Slack messages but must get human approval before sending external email or making a payment. This is an example of:

A) A fully autonomous agent
B) A deterministic control boundary applied selectively to high-risk actions
C) Tree-of-thought reasoning
D) A self-reflection loop

<details>
<summary>Answer</summary>

**Correct: B**

This scopes autonomy by gating specific high-risk actions (external comms, payments) behind human approval while leaving lower-risk actions autonomous — a deterministic control boundary.

**Why the others are wrong:**
- Fully autonomous would require no approval gates anywhere
- Tree-of-thought is a reasoning-exploration pattern, unrelated to approval gating
- Self-reflection is an internal critique mechanism, not an external approval gate

</details>

---

**Q4.** Which reasoning approach explores multiple candidate reasoning paths simultaneously and selects or aggregates the best, rather than committing to one linear chain?

A) Tree-of-thought
B) Chain-of-thought
C) ReAct
D) Self-critique loop

<details>
<summary>Answer</summary>

**Correct: A**

Tree-of-thought explicitly branches into multiple candidate reasoning paths and evaluates/backtracks to select the best.

**Why the others are wrong:**
- Chain-of-thought is a single linear reasoning path with no branching
- ReAct interleaves thought and action but does not explore parallel branches
- Self-critique reviews and revises a single output, it doesn't branch reasoning paths

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** You need an agent to complete a long, well-understood workflow (e.g., generate a quarterly report) where steps are known in advance and don't depend on unpredictable environment feedback. Which control pattern minimizes token usage and latency?

A) ReAct with a reasoning step before every tool call
B) Tree-of-thought with self-consistency voting
C) Plan-and-execute: generate the full plan once, then execute steps
D) Continuous self-reflection after every action

<details>
<summary>Answer</summary>

**Correct: C**

Since the workflow is predictable and doesn't need per-step correction from live feedback, planning once and executing is cheaper and faster than repeated reasoning.

**Why the others are wrong:**
- ReAct's per-step reasoning overhead is unnecessary when the steps are already known and stable
- Tree-of-thought exploration overhead is wasted on a deterministic, already-understood task
- Reflecting after every action adds latency without addressing any actual uncertainty

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** In system-prompt design, which practice BEST reduces the risk of a persona overriding safety and tool-use constraints?

A) Writing the persona description before the tool-use rules and safety constraints
B) Stating safety and tool-use boundaries explicitly and giving them priority over persona/tone instructions
C) Placing hard constraints after a lengthy persona backstory
D) Omitting the persona entirely to save tokens

<details>
<summary>Answer</summary>

**Correct: B**

Explicitly prioritizing hard constraints over persona/tone instructions keeps safety rules from being 'roleplayed away' by the persona.

**Why the others are wrong:**
- Ordering persona first without explicit priority still risks the model favoring tone over the constraint
- Burying constraints under a long persona backstory increases the chance the model deprioritizes them
- Removing the persona doesn't fix the underlying issue of constraint priority in the prompt

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A self-reflection loop lets the agent critique its own draft answer and revise before returning it to the user. What is the PRIMARY risk of not bounding the number of reflection iterations?

A) The agent will always produce a more accurate answer over time
B) Reflection loops always cost less compute than a single pass
C) The agent's persona will change with each iteration
D) Unbounded iterations can loop indefinitely or add latency/cost without guaranteed quality improvement

<details>
<summary>Answer</summary>

**Correct: D**

Reflection is not guaranteed to converge; without a max-iteration cap or convergence check it risks infinite loops or wasted compute/latency with no assured quality gain.

**Why the others are wrong:**
- More reflection iterations can help but are not guaranteed to always improve the answer
- This is false — each additional reflection pass adds compute cost
- Persona is defined in the system prompt and doesn't change across reflection iterations

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** In the perception-reasoning-action-reflection loop, what happens during the 'reflection' stage?

A) The agent receives raw input/observations from the environment
B) The agent decides the next action to take based on current state
C) The agent executes a tool call or produces output
D) The agent evaluates the outcome of its action against the goal and updates its approach if needed

<details>
<summary>Answer</summary>

**Correct: D**

Reflection is the evaluative/self-correction stage that closes the loop by checking the action's outcome against the goal.

**Why the others are wrong:**
- This describes the perception stage
- This describes the reasoning stage
- This describes the action stage

</details>

---

