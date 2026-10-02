## Chapter 8: Serving Agentic Workloads on the NVIDIA Stack — Practice Questions

**Q1.** A team needs to deploy a single fine-tuned Llama-family model as a production chatbot within days, exposing an OpenAI-compatible API, with minimal custom engineering. Which approach BEST meets this requirement?

A) Deploy the model as a custom Triton backend with hand-written pre/post-processing scripts
B) Deploy the corresponding NVIDIA NIM microservice for that model
C) Build a TensorRT-LLM engine from scratch and write a custom serving loop
D) Serve the model via a CPU-based Python Flask wrapper for simplicity

<details>
<summary>Answer</summary>

**Correct: B**

NIM microservices package a pre-optimized model (often built on Triton + TensorRT-LLM under the hood) into a container with a standardized, OpenAI-compatible API, minimizing time-to-deployment.

**Why the others are wrong:**
- Custom Triton backends require significant engineering effort and are meant for multi-framework flexibility, not fastest single-model deployment
- Building a TensorRT-LLM engine from scratch requires manual compilation, tuning, and API wrapping — far slower than using a packaged NIM
- CPU-based serving cannot meet production LLM latency/throughput needs and ignores available GPU acceleration

</details>

---

**Q2.** A company must simultaneously serve models built in PyTorch, ONNX, and TensorRT, including an ensemble pipeline that chains a preprocessing model into an LLM, all on shared GPU infrastructure. Which serving platform BEST fits this requirement?

A) NVIDIA Triton Inference Server, using its multi-backend and ensemble scheduling support
B) NVIDIA NIM, since it standardizes all models under one API
C) A single monolithic TensorRT-LLM engine covering all models
D) A hand-rolled Flask microservice per model with manual routing

<details>
<summary>Answer</summary>

**Correct: A**

Triton natively supports multiple backends (TensorRT, LibTorch, ONNX Runtime, Python, vLLM) and model ensembles/pipelines within a single server, which is exactly this multi-framework, multi-model requirement.

**Why the others are wrong:**
- NIM packages are generally single-model microservices optimized for LLM serving, not a general multi-framework ensemble platform
- TensorRT-LLM is specifically a compiler/runtime for LLMs, not a general framework-agnostic serving platform for ONNX/PyTorch models
- A hand-rolled Flask setup lacks built-in batching, ensemble scheduling, and GPU-aware concurrency that Triton provides out of the box

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** An LLM inference service must maximize total tokens/sec across many concurrent users with highly variable prompt and output lengths, on a fixed GPU budget. Which serving approach BEST achieves this?

A) TensorRT-LLM configured with static batching that waits for a full batch before executing
B) Serving each request independently with batch size fixed at 1
C) Running the model in FP32 without any batching to guarantee correctness
D) TensorRT-LLM configured with in-flight (continuous) batching

<details>
<summary>Answer</summary>

**Correct: D**

In-flight/continuous batching lets new requests enter and finished requests exit the batch at each generation step, keeping GPU utilization high despite variable-length sequences — maximizing aggregate throughput.

**Why the others are wrong:**
- Static batching must wait for the slowest sequence in the batch to finish, wasting GPU cycles on shorter completed sequences
- Batch size 1 processing dramatically underutilizes GPU compute and cannot reach high aggregate throughput
- FP32 with no batching sacrifices both compute speed and throughput, the opposite of the stated goal

</details>

---

**Q4.** Which combination of components typically underlies an NVIDIA NIM microservice for LLM inference?

A) An inference engine (e.g., TensorRT-LLM or vLLM) served through a runtime such as Triton, packaged in a container with a standard API
B) A plain PyTorch eager-mode model wrapped in a REST API with no compilation
C) Only ONNX Runtime with no GPU acceleration
D) A CPU-only quantized model served via gRPC without batching support

<details>
<summary>Answer</summary>

**Correct: A**

NIM containers package an optimized inference engine (commonly TensorRT-LLM, and increasingly other backends like vLLM) behind a serving runtime, exposed via a standardized API, so users get performance without manual engine-building.

**Why the others are wrong:**
- Eager-mode PyTorch without compilation forfeits the performance optimizations NIM is built to deliver
- ONNX Runtime CPU-only ignores the GPU-optimized engines NIM relies on for LLM throughput
- CPU-only, batching-free serving contradicts the GPU-accelerated, batched design of NIM microservices

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** A medical-diagnosis assistant LLM must run on H100 GPUs under a strict latency SLA while minimizing accuracy degradation. Which quantization choice is the recommended trade-off?

A) FP8 quantization, leveraging native Hopper Transformer Engine support
B) INT4 weight-only quantization (e.g., AWQ) for maximum speed
C) FP32 full precision to guarantee no accuracy loss
D) INT8 quantization with no calibration

<details>
<summary>Answer</summary>

**Correct: A**

FP8 is natively accelerated by the Hopper Transformer Engine, offering close-to-FP16 accuracy retention while roughly doubling compute throughput versus FP16 — the best balance of speed and accuracy for sensitive domains.

**Why the others are wrong:**
- INT4 gives the highest speed and memory savings but typically incurs the largest accuracy degradation, risky for a medical use case
- FP32 has no meaningful accuracy benefit over FP8/FP16 for most LLMs but is far slower, likely violating the latency SLA
- Uncalibrated INT8 can introduce larger accuracy loss than FP8 due to coarser quantization without proper calibration

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** A long-context LLM service (up to 128K token contexts) with many concurrent sessions is running out of GPU memory due to growing attention caches. Which configuration change BEST addresses this?

A) Increase the batch size to improve GPU utilization
B) Enable paged KV cache management (e.g., TensorRT-LLM paged attention) to reduce memory fragmentation and allow finer-grained allocation
C) Store the KV cache in FP32 to improve numerical stability
D) Disable batching entirely to reduce memory pressure

<details>
<summary>Answer</summary>

**Correct: B**

Paged KV cache management allocates cache memory in fixed-size blocks rather than contiguous per-sequence buffers, reducing fragmentation and enabling more concurrent long-context sessions within the same GPU memory footprint.

**Why the others are wrong:**
- Increasing batch size without addressing cache memory layout worsens the out-of-memory pressure, since each sequence still needs contiguous space
- FP32 KV cache doubles or quadruples memory usage versus FP16/INT8 cache, making the memory problem worse, not better
- Disabling batching reduces throughput drastically and does not solve the underlying memory fragmentation issue

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** What is the key architectural difference between static/dynamic batching and continuous (in-flight) batching for LLM serving?

A) Continuous batching only works on CPU, while dynamic batching only works on GPU
B) There is no meaningful difference; the terms are interchangeable
C) Dynamic batching waits to fill a batch window before executing all requests together; continuous batching allows requests to join or leave the running batch at each generation step
D) Continuous batching requires disabling KV-cache reuse entirely

<details>
<summary>Answer</summary>

**Correct: C**

Dynamic batching groups requests within a time window and executes them together as a fixed unit; continuous/in-flight batching evaluates the batch composition at each decoding step, inserting new requests and removing completed ones, which is critical for variable-length LLM generation.

**Why the others are wrong:**
- Both batching strategies are GPU-focused techniques in modern inference servers; this option misstates the distinction
- The terms describe genuinely different scheduling strategies with different throughput/latency implications, so they are not interchangeable
- Continuous batching is typically paired with KV-cache reuse/paging, not disabling it, to manage memory efficiently

</details>

---

**Q8.** A real-time voice assistant built on a NIM microservice must meet a strict per-token p99 latency SLA under 100ms, and throughput is a secondary concern. Which configuration adjustment MOST directly helps meet this SLA?

A) Increase the maximum batch size to improve overall GPU efficiency
B) Switch model weights from FP8/FP16 to FP32 for higher fidelity
C) Reduce the batching window/max batch size to prioritize per-request latency over aggregate throughput
D) Increase the maximum supported context length to allow larger prompts

<details>
<summary>Answer</summary>

**Correct: C**

Larger batches and longer batching windows improve throughput but increase per-request latency because requests wait longer or share compute cycles with more sequences; reducing batch size/window trades some throughput for lower, more predictable per-token latency, aligning with a strict latency SLA.

**Why the others are wrong:**
- Larger batch sizes generally increase per-request latency, working against a strict latency SLA, even though they raise aggregate throughput
- FP32 is slower than FP8/FP16 per token, directly increasing latency rather than helping meet a tight SLA
- Increasing max context length does not address latency and can increase KV-cache memory pressure, indirectly hurting performance

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

