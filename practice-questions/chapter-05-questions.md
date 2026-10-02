## Chapter 5: Task Decomposition and Memory Systems — Practice Questions

**Q1.** An agent is executing a 40-step research task that spans multiple tool calls. After step 25, the agent starts producing outputs that no longer reference the original user objective. Which technique BEST addresses this failure mode?

A) Re-inject the original goal statement into the prompt/context at each decomposition step
B) Increase the temperature setting on the model to encourage more creative recovery
C) Switch the agent to use episodic memory instead of semantic memory
D) Reduce the number of available tools to simplify the agent's decision space

<details>
<summary>Answer</summary>

**Correct: A**

Long-horizon tasks drift from the original objective as intermediate context accumulates; explicitly re-anchoring the goal at each step (goal restatement / scratchpad pattern) keeps the plan aligned with the user's intent.

**Why the others are wrong:**
- Temperature affects randomness of token generation, not goal retention across steps.
- Episodic memory helps recall specific past interactions but does not by itself keep an active goal in scope during ongoing execution.
- Reducing tools may simplify choices but does not solve the underlying context/goal drift problem.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A customer support agent must remember a specific user's stated preference from a conversation held three weeks ago ('always ship to my office address') and apply it in today's session. Which memory type is required to satisfy this requirement?

A) Short-term/working memory only
B) A larger context window with no persistence layer
C) Long-term memory with cross-session persistence
D) Semantic memory containing only generalized domain facts

<details>
<summary>Answer</summary>

**Correct: C**

Cross-session recall of a specific fact tied to a specific user requires long-term memory that persists beyond the current session and can be retrieved later.

**Why the others are wrong:**
- Short-term/working memory is scoped to the current session and is discarded afterward.
- A larger context window only extends in-session capacity; it does not persist data across separate sessions.
- Semantic memory alone stores generalized facts, not a specific user's personal preference tied to an episode.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A user asks an agent, 'What did I tell you about my allergy last Tuesday?' The agent correctly retrieves the exact prior exchange, including the date and phrasing used. This retrieval demonstrates which memory category?

A) Semantic memory
B) Working memory
C) Episodic memory
D) Procedural memory

<details>
<summary>Answer</summary>

**Correct: C**

Episodic memory stores specific past events/interactions (what happened, when, in what context), which is exactly what is being recalled here.

**Why the others are wrong:**
- Semantic memory stores generalized, de-contextualized facts (e.g., 'peanuts are a common allergen'), not the specific conversation instance.
- Working memory only holds information for the duration of the current session and would not persist a memory from last Tuesday.
- Procedural memory (skills/how-to knowledge) is not a standard category tested for this exam's memory taxonomy and does not apply to recalling a conversation.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** An agent needs to track intermediate variables, partial tool outputs, and the current sub-step while completing a single multi-turn conversation. Which memory type should hold this information?

A) Long-term memory
B) Semantic memory
C) Episodic memory
D) Short-term/working memory

<details>
<summary>Answer</summary>

**Correct: D**

Short-term/working memory is designed to hold in-session, transient context such as intermediate reasoning state and partial results needed only for the current task execution.

**Why the others are wrong:**
- Long-term memory is for information that must persist beyond the current session, which is unnecessary overhead for transient intermediate state.
- Semantic memory holds generalized facts, not step-by-step execution state.
- Episodic memory records discrete past events for later recall, not the live scratchpad of an in-progress task.

</details>

---

**Q5.** An agent's conversation history is approaching the model's context limit during a long troubleshooting session. Which approach BEST manages this while preserving task continuity?

A) Summarize and compact older turns into a condensed running summary, retaining only key facts and the active goal
B) Discard the oldest messages with no replacement
C) Terminate the session and require the user to restart from scratch
D) Switch the model to a smaller parameter variant mid-session

<details>
<summary>Answer</summary>

**Correct: A**

Summarization/compaction condenses older context into a compact representation that preserves essential facts and the active goal, freeing space while maintaining continuity.

**Why the others are wrong:**
- Blind truncation risks losing critical facts or the original goal, causing task drift.
- Restarting the session destroys accumulated context and harms user experience unnecessarily.
- Swapping model size does not address context length management and can introduce inconsistent behavior.

</details>

---

**Q6.** A user is planning a multi-day trip and interacts with a travel-planning agent across many separate sessions over two weeks. The agent must recall the user's finalized itinerary from session to session without re-asking the same questions. Which combination of design choices BEST satisfies this requirement?

A) Persist itinerary details to long-term memory AND retrieve relevant entries at the start of each new session
B) Use only working memory and increase the context window size
C) Store the itinerary in semantic memory as a generalized travel fact
D) Re-summarize the entire conversation history at the start of every session without persistence

<details>
<summary>Answer</summary>

**Correct: A**

Cross-session continuity requires writing specific, user-tied facts to long-term memory and retrieving them at session start — this is the correct architecture pattern for this constraint.

**Why the others are wrong:**
- Working memory alone, regardless of window size, does not persist across separate sessions.
- Semantic memory is for generalized facts, not a specific user's personal itinerary, which is episodic/personal in nature.
- Re-summarizing without persistence has nothing to summarize once the session ends, since no data survives between sessions.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** Which statement BEST distinguishes episodic memory from semantic memory in an agentic AI system?

A) Episodic memory stores generalized rules; semantic memory stores specific timestamped events
B) Episodic memory is only used for short-term context; semantic memory is only used for long-term context
C) Episodic memory stores specific past interactions with contextual details; semantic memory stores generalized, de-contextualized facts
D) Episodic and semantic memory are interchangeable terms for the same storage mechanism

<details>
<summary>Answer</summary>

**Correct: C**

This is the standard distinction: episodic memory captures specific events/instances (who, what, when), while semantic memory captures generalized knowledge abstracted away from any single event.

**Why the others are wrong:**
- This reverses the correct definitions.
- Both episodic and semantic memory can be implemented as long-term persistence mechanisms; the distinction is about content type, not duration.
- They are distinct memory categories with different content and retrieval purposes, not interchangeable.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** Which task decomposition strategy is MOST appropriate for an agent handling a long-horizon goal composed of dozens of dependent steps where later steps depend on outputs of earlier ones?

A) Generate all steps upfront with no re-evaluation and execute them blindly in sequence
B) Execute steps in random order to explore the solution space
C) Treat the entire task as a single atomic step handled in one model call
D) Decompose into a hierarchical plan with checkpoints, re-evaluating and adjusting sub-goals as new information from prior steps arrives

<details>
<summary>Answer</summary>

**Correct: D**

Long-horizon, dependent tasks require hierarchical decomposition with checkpoints so the agent can incorporate results from completed sub-steps and adjust the remaining plan — this handles dependency and drift.

**Why the others are wrong:**
- Static upfront planning without re-evaluation fails when later steps depend on results unknown at planning time.
- Random ordering ignores dependencies and would produce invalid or inefficient execution.
- Treating a complex, multi-dependency task as a single atomic call typically exceeds reasoning capacity and loses traceability of intermediate state.

</details>

---

