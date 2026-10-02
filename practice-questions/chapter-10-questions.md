## Chapter 10: Evaluating Agentic Systems — Practice Questions

**Q1.** A team evaluates a multi-step research agent that searches the web, synthesizes findings, and writes a report. Two versions produce reports with equally high factual accuracy, but Version B calls the search tool 9 times per task versus Version A's 3 calls, and Version B occasionally loops back to already-visited pages. Which evaluation approach reveals this difference?

A) Task-completion metrics only, since both reports are equally accurate
B) A single end-to-end pass/fail check on the final report
C) Trajectory metrics, since they score the efficiency and correctness of the path taken, not just the final output
D) Human review of the final report's writing style

<details>
<summary>Answer</summary>

**Correct: C**

Trajectory metrics evaluate the sequence of actions (tool calls, revisits, redundant steps) an agent takes, independent of whether the final output is correct — exactly what distinguishes A and B here.

**Why the others are wrong:**
- Task-completion metrics measure whether the goal was achieved, and both versions achieve it equally, so this metric alone hides the inefficiency.
- A binary pass/fail on the final report says nothing about the intermediate steps or wasted tool calls.
- Writing-style review doesn't capture tool-call efficiency or looping behavior, which are trajectory concerns.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An agent's outputs vary run-to-run due to sampling temperature > 0. The team wants to detect a real quality regression after a prompt change, not noise from randomness. What is the BEST regression-testing approach?

A) Run the new version once and compare its single output directly to the old version's single output
B) Run each version N times (e.g., 5-10 trials) per test case, aggregate scores, and compare distributions using statistical significance rather than single samples
C) Set temperature to a very high value to make differences more visible
D) Skip automated testing and rely only on manual spot-checks of a few examples

<details>
<summary>Answer</summary>

**Correct: B**

Non-deterministic systems require multiple sampled runs per test case with aggregated/statistical comparison to separate genuine regressions from sampling noise.

**Why the others are wrong:**
- A single-run comparison cannot distinguish a real regression from normal run-to-run variance.
- Increasing temperature increases variance, making noise larger and regressions harder, not easier, to detect.
- Manual spot-checks don't scale, aren't reproducible, and miss statistically meaningful regressions across the full test suite.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A team uses an LLM-as-judge to score agent responses for helpfulness on a 1-5 scale. Over several months, the judge model's scores drift upward even though human raters see no real quality improvement. What is the MOST likely cause and correct fix?

A) The agent has genuinely improved; no fix is needed
B) The judge prompt/rubric or underlying judge model changed without version pinning, so the fix is to pin the judge model version and freeze the rubric, re-validating against human labels periodically
C) The test set is too small; the fix is to remove the test set entirely
D) LLM judges are inherently unreliable and should never be used for regression testing

<details>
<summary>Answer</summary>

**Correct: B**

Judge drift commonly comes from unpinned judge model updates or rubric changes; the correct mitigation is version-pinning the judge, freezing the rubric, and periodically re-validating judge scores against a human-labeled gold set.

**Why the others are wrong:**
- Assuming genuine improvement ignores the human-rater disagreement, which signals a measurement problem, not a real gain.
- A small test set could add noise, but it wouldn't produce a systematic upward drift over time — removing it entirely also removes the ability to detect regressions.
- LLM judges are widely used in practice when properly calibrated against human labels; discarding them entirely is not the correct engineering response.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** You need to evaluate whether a coding agent's chosen sequence of file edits, test runs, and commits follows best practices (e.g., writing a test before implementation), separate from whether the final code passes tests. Which metric type should you choose?

A) Task-completion metric
B) Trajectory metric
C) Latency metric
D) Cost-per-token metric

<details>
<summary>Answer</summary>

**Correct: B**

Evaluating the process/order of actions (not just the end result) is precisely what trajectory metrics measure.

**Why the others are wrong:**
- Task-completion metrics only check whether the final goal (passing tests) was reached, not the process followed.
- Latency measures speed, not correctness of action sequence.
- Cost-per-token measures resource usage, unrelated to whether best-practice steps were followed.

</details>

---

**Q5.** A customer wants to know, across 1,000 support tickets, what percentage the agent successfully resolved without human escalation. Which metric type directly answers this?

A) Task-completion metric
B) Trajectory metric
C) Rubric-based style metric
D) Tool-call count metric

<details>
<summary>Answer</summary>

**Correct: A**

Task-completion metrics measure whether the end goal (ticket resolved, no escalation) was achieved, which is exactly what's being asked.

**Why the others are wrong:**
- Trajectory metrics would assess how the agent resolved tickets (steps taken), not whether resolution occurred.
- A style rubric evaluates tone/formatting, not resolution success.
- Tool-call count is a trajectory-adjacent efficiency signal, not a measure of goal achievement.

</details>

---

**Q6.** When designing an LLM-as-judge scoring rubric for agent responses, which practice MOST improves consistency and reduces scoring variance across runs?

A) Give the judge only the final answer with no context about the task or reference criteria
B) Ask the judge for a single holistic score with no defined scale or examples
C) Randomize the rubric wording each run to avoid the judge memorizing patterns
D) Provide explicit scoring criteria per point on the scale, few-shot examples of each score level, and require the judge to output a rationale before the score

<details>
<summary>Answer</summary>

**Correct: D**

Explicit per-level criteria, calibration examples, and chain-of-thought-before-score (rationale first) are established techniques that reduce LLM-judge variance and improve agreement with human raters.

**Why the others are wrong:**
- Omitting task context and reference criteria removes the information the judge needs to score accurately, increasing variance.
- An undefined holistic scale with no anchors leads to inconsistent, subjective scoring across runs.
- Randomizing rubric wording removes the standardization needed for reproducible, comparable scores across evaluation runs.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** You are building a reusable evaluation harness to compare agent versions v1, v2, and v3 over time on the same benchmark suite. Which design choice is MOST important to ensure valid version-to-version comparisons?

A) Freeze the test set, fixed seeds/sampling parameters, and scoring logic (including judge version) across all versions being compared, changing only the agent under test
B) Use a different test set each time to avoid overfitting to old examples
C) Let each version use its own preferred evaluation prompt to play to its strengths
D) Only compare the newest version against the previous one, discarding historical results

<details>
<summary>Answer</summary>

**Correct: A**

A reusable harness requires holding everything constant (test set, sampling config, scoring/judge logic) except the agent version, so observed differences are attributable to the agent change alone.

**Why the others are wrong:**
- Changing the test set each run introduces a confound — you can no longer tell if score changes come from the agent or the new test cases.
- Letting each version use its own eval prompt biases results in its favor and breaks comparability.
- Discarding historical results prevents tracking regressions/improvements over the full version history, which is the core purpose of a reusable harness.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** Which statement BEST distinguishes task-completion metrics from trajectory metrics?

A) Task-completion metrics evaluate whether the end goal was achieved; trajectory metrics evaluate the quality, efficiency, or correctness of the steps taken to get there
B) Task-completion metrics measure cost, while trajectory metrics measure latency
C) Task-completion metrics are only used for single-turn systems; trajectory metrics are only used for chatbots
D) There is no meaningful difference — both terms describe the same evaluation approach

<details>
<summary>Answer</summary>

**Correct: A**

This is the core conceptual distinction: outcome (did it succeed) vs. process (how it got there).

**Why the others are wrong:**
- Cost and latency are separate operational metrics, not what defines task-completion vs trajectory.
- Both metric types apply to multi-step agentic workflows, not restricted to single-turn or chatbot-only contexts.
- The two metric types measure fundamentally different things and are often combined, not interchangeable.

</details>

---

