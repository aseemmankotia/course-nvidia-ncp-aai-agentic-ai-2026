## Chapter 8 Cheat Sheet: Serving Agentic Workloads on the NVIDIA Stack

### Serving Stack Roles
| Component | Role | Best For |
|---|---|---|
| **NVIDIA NIM** | Packaged, containerized model microservice with standard API (often wraps Triton + TensorRT-LLM/vLLM) | Fast deployment of supported models, minimal custom engineering |
| **Triton Inference Server** | General multi-framework, multi-model serving runtime; supports ensembles/pipelines | Serving heterogeneous models (PyTorch, ONNX, TensorRT) together |
| **TensorRT-LLM** | Compiler/runtime that builds optimized, quantized LLM inference engines | Deep, LLM-specific performance tuning (quantization, batching, KV-cache) |

### Quantization Trade-offs
| Precision | Speed/Memory | Accuracy Risk | GPU Support |
|---|---|---|---|
| FP32 | Slowest, most memory | Lowest risk | All GPUs |
| FP16/BF16 | Baseline for LLM serving | Low risk | All modern GPUs |
| FP8 | ~2x FP16 throughput on H100 | Low-moderate risk | Native on Hopper/Blackwell (Transformer Engine) |
| INT8 | Faster, less memory | Moderate risk (needs calibration) | Broad GPU support |
| INT4 (AWQ/GPTQ) | Fastest, least memory | Highest risk | Broad GPU support, weight-only |

**Rule of thumb:** accuracy-sensitive + Hopper/Blackwell → FP8. Memory-constrained, tolerant of some accuracy loss → INT4. General default → INT8/FP16.

### Batching Strategies
| Strategy | Behavior | Use Case |
|---|---|---|
| Static batching | Waits for full fixed batch before running | Fixed-shape, single-pass models |
| Dynamic batching (Triton) | Groups requests within a time window | Non-generative models (vision, embeddings) |
| Continuous / in-flight batching (TensorRT-LLM) | Requests join/leave batch at each decode step | LLM token generation with variable-length outputs |

### KV-Cache Management
- Memory scales with: `2 × layers × heads × head_dim × seq_len × batch_size × bytes/elem`
- **Paged KV-cache**: allocates in fixed-size blocks, reduces fragmentation, supports more concurrent long-context sessions.
- Long context (e.g., 128K tokens) + many concurrent users = primary cause of OOM; fix via paging, not just batch size cuts.

### Matching Config to SLA
| SLA Priority | Batch Size/Window | Precision | Notes |
|---|---|---|---|
| Low latency (e.g., <100ms/token) | Small | FP8/FP16 | Avoid large batching windows |
| High throughput (tokens/sec, many users) | Large, continuous batching on | INT8/FP8 | Maximize GPU utilization |

### Common Exam Traps
- Assuming NIM = general multi-framework server (it's not; that's Triton).
- Assuming 'more batching' always helps — it helps throughput but hurts latency.
- Choosing INT4 for accuracy-sensitive domains without acknowledging risk.
- Forgetting KV-cache memory scales with both batch size AND sequence length simultaneously.
- Confusing dynamic batching (Triton, fixed window) with continuous/in-flight batching (TensorRT-LLM, per-step).
