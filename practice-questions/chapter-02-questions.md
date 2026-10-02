## Chapter 2: Structured Reasoning and Tool-Calling Schemas — Practice Questions

**Q1.** An agent must solve a multi-step logic puzzle where several candidate solution paths look promising early but many turn out to be dead ends requiring backtracking. Which reasoning pattern BEST fits this problem?

A) ReAct-style reactive loop (observe-act-observe)
B) Single-pass chain-of-thought
C) Tree-of-thought with branch evaluation and backtracking
D) Plan-and-execute with a fixed, non-revisable plan

<details>
<summary>Answer</summary>

**Correct: C**

Tree-of-thought explicitly generates multiple candidate reasoning branches, scores them, and allows backtracking to abandoned branches — exactly what's needed when many paths look viable but most are dead ends.

**Why the others are wrong:**
- ReAct reacts step-by-step to observations but has no mechanism to explore/compare multiple branches or backtrack once committed
- Chain-of-thought commits to a single linear reasoning path with no exploration or recovery from dead ends
- A fixed plan-and-execute agent commits upfront and doesn't revisit or explore alternate paths mid-execution

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A team wants an agent to call an external payments API reliably, with the model producing well-formed, type-correct arguments every time. Which approach BEST ensures reliable invocation?

A) Use JSON-schema-constrained function/tool calling with explicit types and required fields
B) Ask the model to describe the API call in a sentence, then regex-parse the arguments
C) Have the model generate a shell script that performs the call
D) Let the model choose its own argument format each time for flexibility

<details>
<summary>Answer</summary>

**Correct: A**

Structured output enforcement via JSON schema / function-calling constrains the model's output to a validated, typed structure, minimizing malformed or missing arguments before the tool is invoked.

**Why the others are wrong:**
- Regex-parsing free text is brittle and fails on any deviation in phrasing
- Generating a shell script adds an unnecessary execution/security surface and doesn't guarantee argument validity
- Letting format vary defeats the purpose of a reliable, machine-parseable contract with the tool

</details>

---

**Q3.** A fleet of 500 lightweight agents must react to streaming sensor events with sub-second latency, and each event only requires a simple, well-defined response. Upfront multi-step planning would add unnecessary latency and cost. Which agent architecture BEST fits at this scale?

A) Plan-and-execute, generating a full multi-step plan for every incoming event
B) Reactive agent architecture mapping observations directly to actions
C) Tree-of-thought evaluating multiple branches per event
D) Self-reflection loop re-evaluating every action before committing

<details>
<summary>Answer</summary>

**Correct: B**

Reactive agents skip upfront plan generation and map perception directly to action, giving the lowest latency and lowest per-event compute cost — ideal for high-volume, simple, time-sensitive decisions at scale.

**Why the others are wrong:**
- Plan-and-execute adds planning latency and token cost per event that doesn't scale well for simple, high-frequency decisions
- Tree-of-thought is overkill for simple decisions and multiplies compute cost across 500 agents
- A self-reflection gate before every action adds latency that violates the sub-second requirement

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** An agent occasionally returns confidently incorrect final answers to users. The team wants a mechanism that checks the agent's own draft output against quality criteria before it's returned, and retries generation if it fails. Which technique addresses this?

A) Self-reflection loop with a quality-gating check
B) Tree-of-thought branch exploration
C) Reactive stimulus-response loop
D) Increasing the sampling temperature

<details>
<summary>Answer</summary>

**Correct: A**

A self-reflection loop has the agent (or a critic pass) evaluate its own draft output against defined criteria and regenerate/correct before finalizing — directly gating output quality.

**Why the others are wrong:**
- Tree-of-thought addresses exploring multiple reasoning paths, not post-hoc quality gating of a final answer
- A reactive loop has no self-evaluation step at all
- Raising temperature increases output variance and would make hallucination risk worse, not better

</details>

---

**Q5.** When designing the argument schema for a tool-calling function so the model reliably produces valid calls, which practice reduces invocation errors the MOST?

A) Explicitly define required fields, strict types, and enums for constrained values
B) Use loosely typed 'any'/free-form fields for maximum flexibility
C) Allow all parameters as optional free text to avoid rejecting valid calls
D) Omit field descriptions to keep the schema short and reduce token usage

<details>
<summary>Answer</summary>

**Correct: A**

Explicit required fields, strict typing, and enums constrain the model's generation space, drastically reducing malformed or ambiguous tool calls — this is the core of reliable argument-schema design.

**Why the others are wrong:**
- 'Any' typing removes validation and reintroduces the ambiguity the schema is meant to prevent
- Making everything optional free text reintroduces parsing ambiguity and defeats structured enforcement
- Removing descriptions may shrink the prompt but increases the model's error rate on parameter selection, which outweighs the token savings

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** A team is writing a system-prompt persona for an agent that must stay strictly within a customer-support scope and never attempt unrelated actions. Which persona design practice BEST constrains behavior?

A) A vague persona such as 'You are a helpful assistant'
B) No persona at all — rely on the model's default alignment
C) A persona defined only in the first user message, not the system prompt
D) An explicit persona defining allowed actions, forbidden actions, and behavioral boundaries

<details>
<summary>Answer</summary>

**Correct: D**

Explicit boundaries (allowed/forbidden actions, scope limits) in the system prompt give the model concrete constraints to follow, reducing scope creep and off-task behavior.

**Why the others are wrong:**
- A vague persona provides no concrete constraint and leaves scope ambiguous
- Relying on default alignment gives no task-specific boundary and risks off-scope actions
- Placing persona rules in a user message (not the system prompt) makes them lower priority and easier to override in multi-turn conversations

</details>

---

**Q7.** Which statement BEST characterizes the fundamental trade-off between plan-and-execute and reactive agent architectures?

A) Plan-and-execute produces coherent multi-step action sequences at the cost of upfront planning latency, while reactive agents respond faster but risk local, short-sighted decisions
B) Plan-and-execute always has lower latency but lower reliability than reactive agents
C) Reactive agents replan on every step, incurring higher token cost than plan-and-execute agents
D) The two architectures are functionally interchangeable and differ only in naming

<details>
<summary>Answer</summary>

**Correct: A**

Plan-and-execute invests upfront compute/latency to produce a globally coherent multi-step plan; reactive agents trade that coherence for speed, risking suboptimal or short-sighted per-step decisions.

**Why the others are wrong:**
- This reverses the trade-off — plan-and-execute typically has higher upfront latency, not lower
- Reactive agents specifically avoid full replanning per step; that's the point of the pattern
- The two patterns solve different problems and have materially different latency, cost, and coherence profiles

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** An autonomous travel-booking agent can complete bookings under $5,000 without supervision, but company policy requires review of high-value transactions. Which autonomy design is BEST for this agent role?

A) Grant full autonomy for all transaction sizes to maximize efficiency
B) Require a human-in-the-loop approval gate for transactions above the threshold while allowing autonomous handling below it
C) Require human approval for every transaction regardless of amount
D) Randomly sample a percentage of transactions for review, independent of amount

<details>
<summary>Answer</summary>

**Correct: B**

Autonomy should be calibrated to risk: low-risk, low-value actions can run fully autonomously, while high-risk/high-value actions require a human checkpoint — a graduated autonomy design.

**Why the others are wrong:**
- Full autonomy on high-value transactions ignores the stated risk-based policy and exposes the business to unchecked financial risk
- Requiring approval on every transaction eliminates the efficiency benefit of autonomy for low-risk cases and doesn't match the stated policy
- Random sampling independent of value ignores risk magnitude and could let a large unsupervised transaction through while blocking a trivial one

</details>

---

