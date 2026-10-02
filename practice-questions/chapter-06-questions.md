## Chapter 6: Retrieval-Augmented Generation for Agents — Practice Questions

**Q1.** An agent must answer questions from 50-page technical manuals, cite the exact source passage, and respond in under 1 second per query at scale. Which architecture BEST meets these requirements?

A) Load the full manual into a 200K-token context window on every query
B) Fine-tune the base LLM on the manual text and query it directly with no retrieval
C) Chunk the manuals (~300-500 tokens/chunk), embed them, and retrieve the top relevant chunks to ground the answer
D) Return the single best keyword match verbatim with no LLM involvement

<details>
<summary>Answer</summary>

**Correct: C**

Chunk-embed-retrieve is the standard RAG pattern: it keeps per-query token cost and latency low, scales to large corpora, and preserves traceable source chunks for citation.

**Why the others are wrong:**
- Stuffing 200K tokens every call multiplies latency and cost linearly with corpus size and doesn't scale
- Fine-tuning bakes in static knowledge, doesn't provide citations, and requires retraining for updates — it doesn't solve grounding
- Verbatim keyword match has no semantic understanding and produces no synthesized, natural-language answer

</details>

---

**Q2.** A legal-research agent must find statutes by their exact ID string ('Section 12.4(b)') AND retrieve conceptually related provisions the user described in plain language. Which retrieval design BEST satisfies both needs?

A) Dense embedding retrieval only
B) Hybrid retrieval combining BM25/keyword search with dense embeddings, followed by a re-ranker
C) Pure BM25 keyword search only
D) Stuff the entire statute corpus into the context window each time

<details>
<summary>Answer</summary>

**Correct: B**

BM25 reliably matches exact identifiers and rare tokens that embeddings often blur, while dense retrieval captures semantic/paraphrased intent; a re-ranker then reconciles both result sets by relevance.

**Why the others are wrong:**
- Dense-only retrieval frequently misses exact IDs and rare terms because embedding space smooths lexical distinctions
- Pure BM25 fails on paraphrased or conceptual queries with no lexical overlap
- Full corpus stuffing is cost/latency-prohibitive and unnecessary when retrieval can narrow scope

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A production agent has a 300ms end-to-end latency budget and must minimize per-query LLM cost while maintaining answer quality. Which retrieval pipeline design BEST fits this constraint?

A) Retrieve top-1000 candidates and re-rank all of them with a cross-encoder before generation
B) Skip retrieval entirely and rely on the model's parametric knowledge
C) Retrieve top-20-50 candidates with a fast bi-encoder/ANN index, then re-rank only those down to top 3-5 before grounding
D) Pass the model a 1M-token context window containing the entire knowledge base on every call

<details>
<summary>Answer</summary>

**Correct: C**

A narrow first-stage ANN retrieval followed by re-ranking a small candidate set balances latency (bi-encoder search is fast) and quality (cross-encoder re-ranking is precise but expensive, so it's applied to few items).

**Why the others are wrong:**
- Re-ranking 1000 candidates with a cross-encoder is computationally expensive and blows the latency budget
- No retrieval risks hallucination and stale/incorrect answers with no grounding
- 1M-token context per call is far too costly and slow for a 300ms budget at scale

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** You need a vector store for billion-scale embeddings with sub-100ms approximate nearest-neighbor search and metadata filtering support in production. Which category of solution should you select?

A) A relational database using LIKE/substring text search
B) An in-memory flat list scanned linearly on each query
C) A local single-node FAISS flat index with brute-force exact search
D) A managed vector database with an ANN index (e.g., HNSW) and metadata filtering

<details>
<summary>Answer</summary>

**Correct: D**

Managed vector databases with ANN indexes (HNSW/IVF) are built to scale to billions of vectors with low latency while supporting metadata filters needed for real applications.

**Why the others are wrong:**
- Relational LIKE search has no semantic capability and doesn't operate on embeddings
- Linear scan doesn't scale — latency grows linearly with corpus size, unacceptable at billion scale
- Brute-force exact search (flat index) is accurate but too slow at billion scale without an approximate index

</details>

---

**Q5.** Your retrieval corpus consists of short user queries (5-10 words) searching against long documents (2000+ words). Which embedding model characteristic should you prioritize?

A) A symmetric embedding model tuned for equal-length text pairs
B) An asymmetric (query-document) embedding model specifically trained for short-query-to-long-passage retrieval
C) Any general-purpose sentence similarity model regardless of training objective
D) A model with the largest possible embedding dimension, ignoring training objective

<details>
<summary>Answer</summary>

**Correct: B**

Asymmetric embedding models are trained specifically to align short queries with long passages, improving retrieval quality over symmetric models designed for similar-length text comparison.

**Why the others are wrong:**
- Symmetric models are optimized for comparing texts of similar length/structure (e.g., sentence-to-sentence similarity), not short query vs. long document
- Ignoring training objective risks poor alignment between query and passage vector spaces
- Dimension size alone doesn't determine retrieval quality — training objective and data distribution matter more

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** You are chunking 800-word technical documents for a dense retrieval index. Which chunking configuration reflects best practice for retrieval quality?

A) One chunk per entire document, no splitting
B) 50-token chunks with zero overlap
C) 4000-token chunks with no overlap
D) 300-500 token chunks with roughly 10-20% overlap between adjacent chunks

<details>
<summary>Answer</summary>

**Correct: D**

Chunks in the 300-500 token range preserve enough local context for coherent embeddings while staying small enough for precise retrieval; overlap prevents splitting relevant information across chunk boundaries.

**Why the others are wrong:**
- Whole-document chunks dilute the embedding with unrelated content, hurting retrieval precision
- 50-token chunks with no overlap fragment context and risk cutting relevant statements mid-thought
- 4000-token chunks are too coarse for precise retrieval and increase irrelevant content passed to the LLM

</details>

---

**Q7.** In a retrieve-then-rerank pipeline, what is the correct ordering of retrieval stages before grounding an agent's response?

A) Re-rank the entire corpus first, then run ANN retrieval on the re-ranked subset
B) Run ANN/bi-encoder retrieval to get a broad candidate set (e.g., top 20-50), then apply a cross-encoder re-ranker to select the top few (e.g., top 3-5) for grounding
C) Skip first-stage retrieval and re-rank the full corpus with a cross-encoder for every query
D) Ground the response first, then retrieve chunks afterward to verify

<details>
<summary>Answer</summary>

**Correct: B**

The standard two-stage pattern uses a cheap, fast bi-encoder/ANN search to narrow a large corpus to a manageable candidate set, then a more expensive but precise cross-encoder re-ranks only those candidates before grounding.

**Why the others are wrong:**
- Re-ranking the entire corpus first defeats the purpose of ANN retrieval and is computationally infeasible at scale
- Cross-encoder re-ranking every document in the corpus is far too slow/expensive for production latency budgets
- Grounding before retrieval reverses the pipeline and produces answers with no verified source

</details>

---

**Q8.** Which statement BEST differentiates RAG from a long-context 'stuffing' approach for an agent that needs current, verifiable knowledge?

A) RAG and long-context stuffing are functionally identical in cost and latency at any scale
B) Long-context stuffing always produces more accurate answers because it exposes the model to strictly more information
C) RAG requires no vector store or indexing infrastructure, unlike long-context stuffing
D) RAG retrieves only relevant chunks per query, controlling cost/latency and enabling source citation, while stuffing passes the entire knowledge base as context on every call, increasing token cost and latency as the corpus grows

<details>
<summary>Answer</summary>

**Correct: D**

RAG's core trade-off is retrieval selectivity: narrowing the passed context reduces per-query token cost and latency and supports citation, whereas stuffing scales token cost/latency with corpus size and lacks selective grounding.

**Why the others are wrong:**
- Cost and latency diverge sharply as corpus size grows — they are not equivalent
- More context does not guarantee accuracy; irrelevant content can dilute attention and increase hallucination risk ('lost in the middle')
- RAG explicitly requires indexing/embedding infrastructure (chunking, vector store) to perform retrieval

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

