# Chapter 6 Cheat Sheet — RAG for Agents

## RAG vs Long-Context Stuffing
| Factor | RAG (chunk+retrieve) | Long-Context Stuffing |
|---|---|---|
| Cost/query | Low, scales with retrieved chunks | High, scales with full corpus size |
| Latency | Low (narrow retrieval) | High (large token count processed every call) |
| Citation/traceability | Yes — chunks are sourced | Difficult — no selective sourcing |
| Freshness | Update index incrementally | Must reprocess entire context |
| Accuracy risk | Missed retrieval (recall failure) | 'Lost in the middle' dilution |

**Rule of thumb:** Use RAG when corpus is large, growing, or requires citations. Use long context only for small, static, tightly-scoped knowledge that fits comfortably and cheaply.

## Chunking Best Practices
| Parameter | Recommended Value |
|---|---|
| Chunk size | 300-500 tokens |
| Overlap | 10-20% of chunk size |
| Splitting boundary | Semantic (paragraph/section), not arbitrary token cutoff |

## Retrieval Method Comparison
| Method | Strength | Weakness |
|---|---|---|
| Dense (embeddings) | Semantic/paraphrase matching | Misses exact IDs/rare terms |
| BM25 (keyword) | Exact term/ID matching | No semantic understanding |
| Hybrid (dense+BM25) | Covers both | Requires score fusion (weighted sum or RRF) |

## Re-Ranking Pipeline
1. First-stage retrieval (bi-encoder/ANN): fetch top 20-50 candidates — fast, approximate.
2. Re-ranker (cross-encoder): scores each (query, chunk) pair jointly — slow, precise.
3. Ground generation on top 3-5 re-ranked chunks only.

**Exam trap:** Re-ranking the entire corpus (not just first-stage candidates) is computationally infeasible — always narrow first.

## Embedding Model Selection
| Scenario | Model Type |
|---|---|
| Short query → long document | Asymmetric (query-doc) model |
| Compare similar-length texts | Symmetric model |
| Small dataset, latency-sensitive | Small dims (~384) |
| High-accuracy, larger budget | Larger dims (~1536-3072) |

## BM25 Defaults
- k1 ≈ 1.2-2.0 (term frequency saturation)
- b ≈ 0.75 (length normalization)

## Vector Store Selection
| Scale | Recommended |
|---|---|
| Prototype/small | Local flat index (exact search) |
| Production/large scale | Managed ANN index (HNSW/IVF) with metadata filtering |

## Grounding Pattern
Retrieved chunk → passed as context with source → agent must cite before acting → reduces hallucination, enables audit trail.

**Exam callout:** Grounding tests whether the agent can trace a claim back to a retrieved, trusted source — not just whether the answer sounds correct.
