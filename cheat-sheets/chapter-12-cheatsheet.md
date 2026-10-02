## Chapter 12 Cheat Sheet: Full Exam Simulation & Pacing

### Time Budget
| Metric | Value |
|---|---|
| Total time | 120 minutes |
| Question count | 60-70 |
| Target pace | 1.7-2 min/question |
| Final review reserve | Last 15 minutes |

### Pacing Rules
- If a question exceeds ~3 minutes → flag, guess your best current answer, move on.
- Track flags mentally by confidence: (a) 2+ distractors eliminated, (b) 1 eliminated, (c) pure guess.
- Final 15 min order: (a) → (b) → (c) → unanswered.

### Cross-Domain Decision Pairs
| Trigger in scenario | Choose | Not |
|---|---|---|
| Distinct tools/domains/escalation paths | Multi-agent orchestration | Single large-context agent |
| Frequently changing data, needs citations | RAG | Long-context stuffing |
| Regulatory threshold / irreversible action | HITL checkpoint | Fully autonomous |
| Low-risk, high-volume, reversible action | Autonomous | HITL (adds needless latency) |

### NVIDIA-Stack vs Generic Terms
| Need | NVIDIA Answer | Generic Distractor (usually wrong) |
|---|---|---|
| Deploy optimized LLM w/ standard API, minimal config | NIM microservice | Raw Triton config from scratch |
| Content/topic/safety policy enforcement | NeMo Guardrails | "Prompt engineering only" |
| Inference compilation/optimization | TensorRT-LLM | Generic "model quantization tool" |
| Data curation/dedup for training | NeMo Curator | "ETL pipeline" |
| Reference agent workflow architecture | NIM Agent Blueprints | "Custom framework from scratch" |

### Framing-Trap Decoder
| Keyword | Rewards | Common Wrong Pick |
|---|---|---|
| "MOST scalable" | Elastic/autoscaling design | Fixed oversized instance |
| "MOST reliable" | Redundancy/guaranteed capacity/retries | Cheapest or most elastic option |
| "BEST" (no qualifier) | Fully meets ALL stated constraints | Option missing one constraint (cost, compliance, latency) |

### Guardrails Rail Quick Reference
| Rail | Function |
|---|---|
| Input | Validates/filters incoming user message |
| Dialog | Manages conversation flow/canonical forms |
| Output | Filters generated response before returning to user |
| Execution | Governs tool/action calls |

### Elimination Heuristic
1. Cross out options with deprecated/wrong-service names first.
2. Cross out options that solve only PART of the stated requirement (plausible-but-incomplete).
3. Between remaining two, apply the framing-trap decoder (scalable vs reliable vs best).

### Anxiety/Pacing on Certiverse
- Do one full 120-min timed mock before test day.
- Treat the on-screen timer as a checkpoint tool, not a stressor — check at 30/60/90/105 min marks only.
