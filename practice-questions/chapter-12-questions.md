## Chapter 12: Full Exam Simulation and Time-Management Strategy — Practice Questions

**Q1.** A support platform must resolve tickets spanning billing disputes, account security, and technical troubleshooting, each requiring different tools and escalation logic. Which design is MOST scalable for production?

A) A single LLM instance with an extremely large context window containing all tool documentation
B) A fine-tuned model trained once on all three domains' historical tickets, deployed as one agent
C) A RAG pipeline retrieving domain FAQs into a single agent's prompt on every turn
D) An orchestrator agent that routes to specialized sub-agents, each scoped to one domain with its own tools

<details>
<summary>Answer</summary>

**Correct: D**

Multi-agent orchestration isolates tool access, failure domains, and context per specialty, which scales horizontally as new domains are added without bloating a single agent's prompt or tool list.

**Why the others are wrong:**
- A single giant context window hits context-length and cost ceilings as domains grow, and doesn't isolate tool permissions per domain.
- One fine-tuned model conflates domains, making updates to one domain risk regressions in the others, and doesn't handle real-time tool calls.
- RAG alone solves knowledge retrieval, not the multi-tool, multi-escalation orchestration problem — it's a plausible but incomplete distractor.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An enterprise has 50-page contracts that change daily and must answer employee questions with citations to the exact clause. Which approach is MOST reliable at scale?

A) Stuff the full corpus into a long-context model prompt on every query
B) A RAG pipeline with a retriever indexing updated chunks and a generator that cites retrieved passages
C) Fine-tune the LLM nightly on the full contract text
D) Cache yesterday's answers and reuse them for similar questions

<details>
<summary>Answer</summary>

**Correct: B**

RAG keeps the index fresh via incremental re-embedding and grounds answers in retrieved chunks, enabling accurate citations without reprocessing the entire corpus per query.

**Why the others are wrong:**
- Long-context stuffing is costly, slower, and returns stale content once new contracts are added mid-session — a classic plausible-but-incomplete distractor.
- Nightly fine-tuning doesn't guarantee citation accuracy and can't reflect same-day contract changes.
- Cached answers go stale immediately given daily contract changes and provide no citation guarantee.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A lending agent can approve loans up to $10,000 autonomously but must pause for review above that threshold due to regulatory requirements. Which design BEST satisfies this constraint?

A) A human-in-the-loop checkpoint that blocks execution above the threshold pending explicit approval
B) Fully autonomous execution with all approvals logged for audit after the fact
C) Disable the approval action entirely and require a human to type in every approval manually
D) Use output guardrails only to flag risky language in the agent's response

<details>
<summary>Answer</summary>

**Correct: A**

A conditional HITL gate tied to a business threshold enforces compliance before the irreversible action occurs, which is what 'pause for review' requires.

**Why the others are wrong:**
- Post-hoc audit logging doesn't prevent the non-compliant action from executing — it only records it after the fact.
- Removing all autonomy defeats the stated requirement that sub-threshold loans remain autonomous.
- Output guardrails inspect text content, not transactional state like a dollar threshold, so they don't gate the action itself.

</details>

---

**Q4.** You need to prevent an agent from discussing competitor products and from leaking system prompts in its responses, without modifying the underlying model. Which NVIDIA component is designed for this?

A) NVIDIA Triton Inference Server
B) NVIDIA TensorRT-LLM
C) NVIDIA NeMo Guardrails
D) NVIDIA NIM Agent Blueprints

<details>
<summary>Answer</summary>

**Correct: C**

NeMo Guardrails applies programmable rails (topical, safety, and jailbreak-detection rails) around a model's inputs and outputs without retraining.

**Why the others are wrong:**
- Triton serves and batches inference requests; it has no built-in policy/rail enforcement.
- TensorRT-LLM is an inference optimization/compilation library, not a content policy layer.
- NIM Agent Blueprints are reference architectures for building agent workflows, not a standalone rail-enforcement engine.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** A team wants to deploy a production-ready LLM with a standardized API endpoint, minimal custom serving configuration, and built-in optimization, rather than assembling inference serving from scratch. Which component fits BEST?

A) Raw Triton Inference Server with a hand-written model repository
B) NeMo Curator
C) RAPIDS
D) NVIDIA NIM microservice

<details>
<summary>Answer</summary>

**Correct: D**

NIM packages an optimized model behind a standard API (often OpenAI-compatible) with Triton and TensorRT-LLM under the hood already configured — exactly the 'minimal custom serving' requirement.

**Why the others are wrong:**
- Raw Triton requires you to author the model repository, batching, and config yourself — more setup than the scenario wants, a classic 'too much manual work' distractor.
- NeMo Curator is a data curation/preprocessing tool, not an inference serving layer.
- RAPIDS accelerates data science/ETL workloads on GPUs; it does not serve LLM inference endpoints.

</details>

---

**Q6.** You are 90 minutes into a 120-minute, 65-question exam with 15 questions flagged. You have roughly 30 minutes left. What is the BEST use of remaining time?

A) Re-read every question from the start to double-check answers not flagged
B) Skip flagged questions and submit early to avoid running out of time
C) Return to flagged questions first, prioritizing ones where you already eliminated 2+ distractors
D) Spend the remaining time evenly across all 65 questions regardless of flag status

<details>
<summary>Answer</summary>

**Correct: C**

Flagged questions with partial elimination already done have the highest expected point return per minute spent — finishing those first maximizes score within limited remaining time.

**Why the others are wrong:**
- Re-reading unflagged (already-confident) answers wastes time on items unlikely to change and risks running out of time on flagged ones.
- Submitting early forfeits points on flagged questions you had a real chance of solving.
- Even time allocation ignores that flagged questions are the ones actually worth the remaining budget.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** In a NeMo Guardrails configuration, which rail type is responsible for blocking a generated response BEFORE it is returned to the user, e.g., to catch leaked PII in the model's answer?

A) Input rails
B) Dialog rails
C) Output rails
D) Execution rails

<details>
<summary>Answer</summary>

**Correct: C**

Output rails run checks on the generated response before it is sent to the user, which is where PII-leak filtering belongs.

**Why the others are wrong:**
- Input rails filter/validate the incoming user message, not the model's generated reply.
- Dialog rails manage conversation flow and canonical forms, not final-response content filtering.
- Execution rails govern tool/action calls the agent makes, not the text of the final response.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** A scenario asks which architecture is 'MOST scalable' for handling unpredictable spikes in agent traffic. Which answer pattern does this framing typically reward?

A) A single large fixed-capacity instance sized for peak load at all times
B) An autoscaling group of NIM instances behind a load balancer that scales with demand
C) A larger context window on the same fixed instance
D) Manual capacity increases requested by an on-call engineer during spikes

<details>
<summary>Answer</summary>

**Correct: B**

'MOST scalable' framing rewards horizontal, demand-driven elasticity — autoscaled NIM instances match load without idle over-provisioning or manual delay.

**Why the others are wrong:**
- Fixed peak-sized capacity is 'reliable' for spikes but wastes cost at idle — it answers a reliability question, not a scalability one, which is the classic trap.
- A larger context window addresses per-request capability, not concurrent request volume/traffic scaling.
- Manual scaling introduces human latency, defeating the 'unpredictable spikes' requirement.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

