## Chapter 12: Full Exam Simulation and Time-Management Strategy — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Exam time budget: 65-70 questions in 120 minutes — target pace per question? | ~1.7-2 minutes/question. Exam tip: if a question takes >3 min, flag and move on — don't let one scenario eat your budget. |
| 2 | How much time should you reserve for final review pass? | Last ~15 minutes. Priority: flagged questions with 2+ distractors already eliminated, then unanswered questions, then low-confidence single-elimination flags. |
| 3 | Single-agent vs multi-agent — when does the exam expect multi-agent? | When the scenario has distinct domains/tools/escalation paths that would bloat one agent's context or tool permissions. Single agent is correct only for narrow, single-domain tasks. |
| 4 | RAG vs long-context — decision rule tested on exam? | RAG wins when data changes frequently or exceeds practical context limits and citations are required. Long-context wins only for small, static, one-shot documents. |
| 5 | HITL vs fully autonomous — what triggers HITL in exam scenarios? | Regulatory thresholds, irreversible actions, high financial/legal risk. Autonomous is correct only for low-risk, reversible, high-volume actions. |
| 6 | NVIDIA NIM vs raw Triton Inference Server — which is the 'less setup' answer? | NIM = pre-packaged, optimized microservice with standard API, minimal config. Triton alone = you build/configure the model repository and serving yourself. |
| 7 | NeMo Guardrails vs TensorRT-LLM — which enforces content policy? | NeMo Guardrails enforces topical/safety/jailbreak policy on inputs/outputs. TensorRT-LLM is an inference optimization/compiler library — no policy enforcement. |
| 8 | NeMo Guardrails rail type that blocks generated text before it reaches the user? | Output rails. (Input rails = filter incoming prompts; dialog rails = manage conversation flow; execution rails = govern tool/action calls.) |
| 9 | Exam trap: 'MOST scalable' framing — what wrong answer do students pick? | A fixed, oversized instance (that's the 'reliable for peak' answer, not scalable). Correct pattern: autoscaling/elastic instances that grow with demand. |
| 10 | Exam trap: 'MOST reliable' framing — what does this usually reward? | Redundancy, retries, and fixed guaranteed capacity/checkpoints — not the cheapest or most elastic option. Distinguish from 'scalable' which rewards elasticity. |
| 11 | Flagging strategy: which flagged questions to revisit FIRST in final review? | Those where you already eliminated 2+ distractors — highest expected point return per minute of remaining time. |
| 12 | Certiverse proctored remote exam — key pacing anxiety mitigation? | Do a full timed practice simulation beforehand so the on-screen timer and question flow feel familiar; treat unfamiliar UI friction, not just content gaps, as a risk to manage. |
