## Chapter 10 Cheat Sheet: Evaluating Agentic Systems

### Metric Type Selection

| Question you're answering | Metric type | Example |
|---|---|---|
| Did the agent achieve the goal? | Task-completion | Ticket resolved without escalation? |
| Was the path efficient/correct/safe? | Trajectory | Tool-call count, redundant loops, unsafe steps |
| Both matter (most production evals) | Combine both | Score outcome AND path separately, report together |

**Rule of thumb:** Outcome-only concern → task-completion. Process/efficiency/safety concern → trajectory. When unsure, default to reporting both — they diagnose different failure modes.

### LLM-as-Judge Rubric Design Checklist

- [ ] Explicit criteria written for **each** point on the scale (not just endpoints)
- [ ] Few-shot calibration examples for each score level included in the judge prompt
- [ ] Judge outputs a **rationale before** the numeric score (reduces snap-judgment bias)
- [ ] Judge model **version pinned** — never left to "latest"
- [ ] Rubric text **frozen** across comparison runs
- [ ] Judge scores periodically re-validated against a **human-labeled gold set** (watch for drift)

### Regression Testing Non-Deterministic Agents

| Bad practice | Correct practice |
|---|---|
| Compare 1 run vs 1 run | Run N samples (e.g., 5-10) per test case per version |
| Diff raw outputs directly | Aggregate scores, compare distributions statistically |
| Increase temperature to "see differences" | Keep sampling params fixed/pinned across versions |
| Manual spot-check only | Automated harness across full suite, every version |

### Reusable Evaluation Harness — What to Freeze vs Vary

| Freeze (keep identical across versions) | Vary (the thing under test) |
|---|---|
| Test set / benchmark suite | Agent version (v1, v2, v3...) |
| Sampling seeds / temperature / params | — |
| Scoring logic + judge model version | — |
| Rubric wording | — |

If any "freeze" column item changes between comparisons, the result is confounded and not a valid version-to-version comparison.

### End-to-End Multi-Step Workflow Benchmarking

- Score the **final outcome** across the whole chain (task-completion).
- Add **per-step or trajectory sub-scores** to localize *where* a failure occurred (e.g., correct retrieval but wrong synthesis step).
- Report both together — end-to-end pass rate alone hides *why* failures happen; step-level trajectory data alone hides whether the user's goal was actually met.

### Exam Traps

- **Trap:** Assuming a high task-completion score means the agent is "good" — it can hide unsafe or inefficient intermediate steps only trajectory metrics catch.
- **Trap:** Comparing single non-deterministic runs and calling it a regression — always sample multiple runs first.
- **Trap:** Treating LLM-as-judge as a fixed oracle — unpinned judge versions or rubric edits silently shift scores over time; always re-validate against human labels.
