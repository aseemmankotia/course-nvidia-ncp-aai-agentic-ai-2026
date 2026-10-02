## Chapter 2: Structured Reasoning and Tool-Calling Schemas — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | When should you use Tree-of-thought over Chain-of-thought? | When the problem requires exploring multiple candidate paths and backtracking from dead ends (e.g., puzzles, planning with branching options). Exam tip: ToT = explore + backtrack; CoT = single linear path, no recovery. |
| 2 | Self-reflection loop — what does it gate? | Output quality before returning a final answer: the agent (or critic pass) evaluates its own draft against criteria and regenerates if it fails. Exam tip: this is a quality gate, not a reasoning-exploration technique. |
| 3 | Reactive agent vs Plan-and-execute — core trade-off? | Reactive: low latency, maps observation directly to action, risks short-sighted/local decisions. Plan-and-execute: upfront planning latency/cost, produces globally coherent multi-step action sequences. |
| 4 | At large agent-fleet scale (100s of agents, real-time events), which architecture is usually preferred and why? | Reactive — avoids per-event planning cost/latency that doesn't scale when decisions are simple and time-sensitive. |
| 5 | Best practice for tool-calling argument schema design? | Explicit required fields, strict types, and enums for constrained values. Reduces malformed/ambiguous tool calls versus free-text or 'any'-typed fields. |
| 6 | Why prefer JSON-schema function-calling over free-text argument parsing? | Structured output enforcement constrains the model to a validated, typed format before execution, eliminating regex-based parsing brittleness. |
| 7 | Function-calling schema — minimum elements to define reliably? | Name, description, parameters object with typed properties, `required` array, and enums where values are constrained. Missing `required` is a common cause of malformed calls. |
| 8 | System-prompt persona design for constrained agents — what must it explicitly include? | Allowed actions, forbidden actions, and behavioral/scope boundaries — not just a role label like 'helpful assistant'. Vague personas allow scope creep. |
| 9 | Where should persona/boundary rules be placed for highest priority adherence? | In the system prompt, not a user message — system-level instructions are weighted higher and persist better across multi-turn conversations. |
| 10 | How should autonomy level be decided for an agent role? | Calibrate to risk/impact of the action: low-risk actions → full autonomy; high-risk/high-value actions → human-in-the-loop approval gate (graduated autonomy). |
| 11 | Matching reasoning pattern to problem type — quick rule | Single clear linear task → Chain-of-thought. Needs exploration/backtracking → Tree-of-thought. Needs quality check before final answer → Self-reflection loop. Needs external tool interaction step-by-step → ReAct/reactive. |
| 12 | Common exam trap: reactive vs plan-and-execute latency | Students often assume plan-and-execute is always faster because it 'has a plan.' Wrong — plan-and-execute pays upfront planning latency; reactive is faster per-step but risks short-sighted decisions. |
