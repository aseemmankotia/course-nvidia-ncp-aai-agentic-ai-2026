# Chapter 8 Lab: Serving Agentic Workloads on the NVIDIA Stack

**Estimated time:** 35 minutes

## Exercise

Deploy an LLM behind NVIDIA NIM and benchmark p50/p99 latency and throughput with different batching settings, then compare against a Triton Inference Server deployment using a TensorRT-LLM backend

## Key takeaway

NIM packages the microservice, Triton serves it, and TensorRT-LLM compiles the inference engine — tune batching, quantization, and KV-cache against the specific latency/throughput SLA given.
