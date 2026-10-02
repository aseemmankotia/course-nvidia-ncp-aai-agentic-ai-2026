## Chapter 2 Cheat Sheet — Structured Reasoning & Tool-Calling Schemas

### Reasoning Pattern Selection
| Problem Type | Pattern | Why |
|---|---|---|
| Linear, single clear path | Chain-of-thought | No branching needed |
| Exploration + backtracking (puzzles, planning with dead ends) | Tree-of-thought | Evaluates/compares branches, can backtrack |
| Needs output quality check before finalizing | Self-reflection loop | Critic pass gates final answer |
| Step-by-step tool/env interaction | ReAct (reactive) | Observe → act → observe, no upfront plan |
| Multi-step task needing global coherence | Plan-and-execute | Generates full plan upfront, then executes |

### Reactive vs Plan-and-Execute Trade-offs
| Factor | Reactive | Plan-and-Execute |
|---|---|---|
| Latency per action | Low | Higher (planning overhead) |
| Coherence across steps | Lower (local decisions) | Higher (global plan) |
| Scale (many concurrent agents) | Scales well | Costlier at scale |
| Best for | High-frequency, simple decisions | Complex, multi-step tasks needing consistency |

### Argument-Schema Design Checklist
- Define `required` fields explicitly
- Use strict types (string/int/enum), avoid 'any'
- Use enums for constrained value sets
- Keep descriptions concise but present (omitting them raises error rate)
- Validate before execution — never trust raw model text as final args

### Persona / Autonomy Design
| Design Element | Rule of Thumb |
|---|---|
| Persona placement | System prompt (highest priority), not user message |
| Persona content | State allowed actions, forbidden actions, scope boundary explicitly |
| Autonomy grant | Calibrate to risk: low-impact → full autonomy; high-impact → human-in-the-loop gate |
| Self-reflection | Add a critic/quality-gate pass before returning final output on high-stakes tasks |

### Exam Notes
- Exam scenarios describe the *problem shape* (branching? quality risk? scale? risk of action) — match to pattern, don't default to the 'trendiest' technique.
- Common wrong answer: picking Tree-of-thought or self-reflection for simple, high-volume reactive tasks — adds unnecessary latency/cost.
- Common wrong answer: loosely-typed/free-text tool arguments described as 'more flexible' — exam favors strict schema enforcement for reliability.
- Autonomy questions test graduated/risk-based delegation, not all-or-nothing autonomy.
