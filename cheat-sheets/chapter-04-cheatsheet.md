## Chapter 4 Cheat Sheet: Orchestration & State Management

### Framework Selection (Heavily Tested)

| Framework | Core Abstraction | Best For | Control Granularity |
|---|---|---|---|
| **LangGraph** | Explicit StateGraph (nodes, edges, shared state) | Cyclic workflows, conditional branching, human-in-the-loop, checkpoint/resume | Highest |
| **CrewAI** | Role-based agents + Sequential/Hierarchical process | Structured crews (Researcher/Writer/Editor) with minimal custom code | Lowest (opinionated) |
| **AutoGen** | Conversational GroupChat / GroupChatManager | Free-form multi-agent dialogue until consensus | Moderate |
| **NeMo Agent (AIQ) toolkit** | Framework-agnostic orchestration + observability layer | Unifying/profiling agents built across LangChain, LlamaIndex, CrewAI, etc. under NVIDIA AI Blueprints | N/A (wrapper, not an engine) |

**Selection rule of thumb:** Need explicit graph control + persistence → LangGraph. Need role structure fast → CrewAI. Need open conversation between agents → AutoGen. Need enterprise-wide observability across frameworks → NeMo Agent (AIQ) toolkit on top of the above.

### Loop & Termination Controls

| Framework | Parameter | Purpose | Default (if known) |
|---|---|---|---|
| LangGraph | `recursion_limit` | Caps graph super-steps; raises `GraphRecursionError` | 25 |
| AutoGen | `max_round` | Caps total GroupChat rounds | 10 |
| AutoGen | `max_consecutive_auto_reply` | Caps per-agent auto-replies | Configurable |
| CrewAI | `max_iter` | Caps per-agent task iterations | Configurable |

**Exam trap:** Relying only on a hard limit (recursion_limit/max_round) as your "termination logic." The exam expects you to design an **explicit** condition (conditional edge to END, consensus check, or task-completion flag) with the numeric limit as a backstop only — never the primary mechanism.

### Common Failure Modes
- **Runaway loop:** no branch ever satisfies the exit condition → agents cycle until hitting the safety limit.
- **Deadlock:** two or more agents wait on each other's output with no forcing function (e.g., a moderator or manager agent) to break the tie.
- **Silent state corruption:** shared state object mutated inconsistently across nodes/agents, causing downstream logic to branch incorrectly.

### State Management Notes
- LangGraph state is a typed schema (TypedDict/Pydantic) passed and updated node-to-node; checkpointers persist this state for resume/human-in-the-loop.
- CrewAI and AutoGen manage state more implicitly through task outputs/conversation history rather than an explicit shared schema.
- NeMo Agent (AIQ) toolkit does not define its own state model — it observes/profiles state and execution across whatever underlying framework is used.

### Quick Memory Trick
"**L**angGraph = **L**ogic graph (explicit, cyclic). **C**rewAI = **C**hain of command (roles, sequence). **A**utoGen = **A**ctual conversation (chat-driven)."
