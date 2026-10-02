## Chapter 10: Evaluating Agentic Systems — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Task-completion metric — what does it measure? | Whether the agent achieved the end goal (success/fail or graded outcome), regardless of the path taken. Exam tip: use when the customer cares about outcomes, not process. |
| 2 | Trajectory metric — what does it measure? | The quality/efficiency/correctness of the sequence of actions (tool calls, steps, order) an agent takes to reach the outcome. Exam tip: use when process quality, safety, or efficiency matters, not just the final result. |
| 3 | When would you choose a trajectory metric over a task-completion metric? | When two agents reach the same correct outcome but you need to catch wasted tool calls, unsafe intermediate actions, redundant loops, or wrong-order steps. |
| 4 | Why must non-deterministic agents be regression-tested with multiple runs per test case, not one? | A single run can't separate a real regression from normal sampling variance. Aggregate scores across N runs (e.g., 5-10) and use statistical comparison, not single-sample diffing. |
| 5 | What causes LLM-as-judge score drift over time even when agent quality is stable? | Unpinned judge model versions or unfrozen rubric text changing behind the scenes. Fix: pin judge model version, freeze rubric, periodically re-validate against human labels. |
| 6 | What three elements improve LLM-as-judge rubric consistency? | 1) Explicit criteria per score level, 2) few-shot calibration examples per level, 3) requiring rationale/reasoning before the numeric score. |
| 7 | What must be held constant in a reusable evaluation harness when comparing agent v1 vs v2 vs v3? | Test set, sampling parameters/seeds, and scoring/judge logic — only the agent version under test should change. |
| 8 | Benchmarking a multi-step workflow end-to-end — what should the score reflect? | The final outcome across the entire chain of steps (not per-step scores in isolation), often combined with trajectory sub-scores to diagnose where failures occur. |
| 9 | Scenario: agent resolves the user's request but takes an unsafe or policy-violating intermediate action. Which metric catches this if task-completion looks fine? | Trajectory metric — it evaluates the path/steps taken, which is where the safety violation occurred, even though the final outcome succeeded. |
| 10 | Why compare judge scores against a human-labeled gold set periodically? | To detect judge drift or miscalibration early — human labels serve as ground truth to validate that the LLM-as-judge remains aligned with real quality judgments. |
| 11 | Common mistake when regression testing non-deterministic agents? | Comparing a single run of the new version to a single run of the old version and concluding a regression/improvement from that one sample — ignoring run-to-run variance. |
| 12 | Choosing metric type per evaluation goal — rule of thumb? | Ask 'does the customer/eval care about the outcome, the process, or both?' Outcome-only → task-completion. Process/efficiency/safety → trajectory. Most production evals combine both. |
