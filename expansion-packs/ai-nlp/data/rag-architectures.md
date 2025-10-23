# RAG (Retrieval-Augmented Generation) Architectures

## Overview

RAG combines information retrieval with LLM generation to provide accurate, source-grounded responses. Critical for intelligence applications where information must be synthesized from multiple classified documents and sources.

---

## Why RAG?

**Problem with Base LLMs**:
- Knowledge cutoff dates
- Hallucinations (making up information)
- Can't access proprietary/classified data
- Expensive to fine-tune for new knowledge

**RAG Solution**:
- Retrieve relevant information from knowledge base
- Augment prompt with retrieved context
- LLM generates response based on retrieved facts
- Can cite sources

---

## RAG Architecture Components

```
User Query
    ↓
1. Query Processing (embed query)
    ↓
2. Retrieval (search vector DB)
    ↓
3. Reranking (optional, reorder results)
    ↓
4. Context Augmentation (add to prompt)
    ↓
5. LLM Generation (generate response)
    ↓
Response with Citations
```

---

## 1. Document Ingestion Pipeline

### Ingestion Steps

```
Documents → Parsing → Chunking → Embedding → Vector DB Storage
```

**Parsing**:
- Extract text from PDFs, Word, PowerPoint, images (OCR)
- Preserve structure (headings, tables, metadata)
- Handle multi-page documents
- Preserve classification markings

**Chunking Strategies**:

**Fixed-Size Chunking**:
- Chunk size: 512-1024 tokens
- Overlap: 50-100 tokens (preserve context)
- Pros: Simple, predictable
- Cons: May split mid-sentence or mid-concept

**Semantic Chunking**:
- Chunk by paragraphs, sections, or topics
- Use NLP to detect topic boundaries
- Pros: Preserves semantic units
- Cons: Variable chunk sizes

**Hybrid Chunking**:
- Semantic chunking with max size limit
- Best of both worlds
- Recommended for intelligence documents

**Metadata Enrichment**:
- Document ID, title, author, date
- Classification level
- Source organization
- Document type (report, email, transcript)

---

## 2. Embedding Models

### Embedding Options

| Model | Dimensions | Cost | Use Case |
|-------|-----------|------|----------|
| **OpenAI text-embedding-3-large** | 3072 | $0.13/1M tokens | High quality, general |
| **OpenAI text-embedding-3-small** | 1536 | $0.02/1M tokens | Cost-effective |
| **Cohere embed-english-v3** | 1024 | $0.10/1M tokens | Good for search |
| **sentence-transformers** | 384-768 | Free (self-host) | Privacy, cost |
| **Voyage AI** | 1024-1536 | $0.12/1M tokens | Optimized for RAG |

### Embedding Strategy

**Considerations**:
- **Quality**: Larger models = better retrieval
- **Cost**: Embedding millions of documents = $$$
- **Latency**: Query-time embedding must be fast (<100ms)
- **Privacy**: Self-hosted for classified data

**Recommendation for Intelligence**:
- **sentence-transformers** (self-hosted) for classified data
- **OpenAI embed-3-small** for unclassified cost-effective
- **Cohere embed-v3** for highest quality retrieval

---

## 3. Vector Databases

### Vector DB Options

| Database | Type | Best For |
|----------|------|----------|
| **Pinecone** | Managed | Ease of use, scale |
| **Weaviate** | Open-source | Self-hosted, hybrid search |
| **Chroma** | Open-source | Simplicity, prototyping |
| **pgvector** | Postgres extension | Existing Postgres users |
| **Qdrant** | Open-source | Performance, filters |
| **Milvus** | Open-source | Large scale, production |

### Selection Criteria

**Use Pinecone when**:
- Want fully managed service
- Need to scale quickly
- Budget allows ($70+/month)

**Use Weaviate when**:
- Need hybrid search (keyword + vector)
- Want to self-host for security
- Need multi-tenancy

**Use pgvector when**:
- Already using PostgreSQL
- Want familiar SQL interface
- Small to medium scale

**Use Chroma when**:
- Prototyping/local development
- Simple embedded use case
- Don't need production features

**Recommendation for Intelligence**:
- **Weaviate** (self-hosted) for classified data
- **pgvector** if already using Postgres
- **Pinecone** for unclassified rapid deployment

---

## 4. Retrieval Strategies

### Similarity Search (Basic)

**Algorithm**: Cosine similarity, Euclidean distance

```python
query_embedding = embed_model.encode(query)
results = vector_db.search(
    query_embedding,
    top_k=10,
    min_similarity=0.7
)
```

**Pros**: Fast, simple
**Cons**: May retrieve irrelevant results with high similarity

---

### Hybrid Search (Keyword + Vector)

**Combine BM25 (keyword) + Vector Search**

```
Keyword Search Results (BM25) → Score 1
Vector Search Results         → Score 2
Combined Score = α * Score1 + (1-α) * Score2
```

**Pros**: Better recall, handles exact matches
**Cons**: More complex, requires both indexes

**When to Use**: Technical documents with specific terms

---

### Metadata Filtering

**Filter before or after similarity search**

```python
results = vector_db.search(
    query_embedding,
    top_k=10,
    filters={
        "classification": "SECRET",
        "date": {"gte": "2024-01-01"},
        "source": "CIA"
    }
)
```

**Use Cases**:
- Retrieve only from specific time period
- Filter by classification level
- Source-specific queries
- Document type filtering

---

### Multi-Query Retrieval

**Generate multiple variations of query**

```
User Query: "What are the capabilities of adversary X?"

LLM generates:
- "adversary X military capabilities"
- "adversary X technology advancement"
- "adversary X operational capacity"
- "adversary X strategic resources"

Retrieve for each variation → Combine results
```

**Pros**: Better recall, handles query ambiguity
**Cons**: Multiple embedding calls, slower

---

### Hypothetical Document Embeddings (HyDE)

**LLM generates hypothetical answer, embed answer, retrieve**

```
User Query: "What is adversary X planning?"

LLM generates hypothetical answer:
"Adversary X is planning military operations in region Y..."

Embed hypothetical answer → Search → Retrieve actual docs
```

**Pros**: Can improve retrieval when query is vague
**Cons**: LLM call overhead, may hallucinate

---

## 5. Reranking

**Problem**: Top-K results may not be best

**Solution**: Rerank with cross-encoder model

```
Initial Retrieval: top-100 results
    ↓
Reranker: Score each result with cross-encoder
    ↓
Return: top-10 after reranking
```

**Reranker Options**:
- **Cohere Rerank**: API-based, high quality
- **sentence-transformers cross-encoders**: Self-hosted
- **LLM-based reranking**: Use LLM to score relevance

**When to Use**: When precision is critical (intelligence reports)

**Cost**: Cohere $1/1K rerank requests

---

## 6. Context Augmentation

### Prompt Construction

```
System Prompt:
You are an intelligence analyst. Answer questions based ONLY on
the provided documents. Cite sources using [Doc X] format.

Retrieved Documents:
[Doc 1 - SECRET//NOFORN] Source: CIA Report 2024-123
Content: ...

[Doc 2 - TOP SECRET//SI] Source: NSA SIGINT 2024-456
Content: ...

User Question:
{user_query}

Answer:
```

### Context Window Management

**Challenge**: Token limits (4K-200K depending on model)

**Strategies**:
1. **Top-K Retrieval**: Only use top 5-10 most relevant
2. **Summarization**: Summarize long documents before adding
3. **Windowing**: Feed chunks sequentially if needed
4. **Compression**: Extract only relevant paragraphs

**Token Budget Example** (GPT-4 Turbo 128K):
- System prompt: 500 tokens
- Retrieved context: 10,000 tokens
- User query: 100 tokens
- **Leaves ~117K for conversation history and response**

---

## 7. Generation with Citations

### Citation Strategies

**Inline Citations**:
```
Adversary X has developed new capabilities in cyber operations [Doc 3].
Intelligence indicates deployment in region Y as of January 2024 [Doc 7].
```

**Footnote Style**:
```
Adversary X has developed new capabilities in cyber operations.¹
¹ CIA Report 2024-123, dated 15 Jan 2024 (SECRET//NOFORN)
```

**Source Block**:
```
Answer: ...

Sources:
- CIA Report 2024-123 (SECRET//NOFORN)
- NSA SIGINT Report 2024-456 (TOP SECRET//SI//NOFORN)
```

### Prompt for Citations

```
Important: Include citations for all factual claims.
Use format: [Doc X] immediately after each claim.
Only cite documents provided in context.
Do not make claims without supporting documents.
```

---

## RAG Evaluation

### Retrieval Metrics

**Recall@K**: % of relevant docs in top-K
**Precision@K**: % of top-K that are relevant
**MRR (Mean Reciprocal Rank)**: 1/rank of first relevant doc
**NDCG (Normalized Discounted Cumulative Gain)**: Quality ranking

**Example**:
- Query: "Adversary X cyber capabilities"
- Relevant docs: [5, 12, 23, 45]
- Retrieved top-10: [5, 7, 12, 18, 23, ...]
- Recall@10: 3/4 = 0.75 (got 3 out of 4 relevant)

### Generation Quality Metrics

**Faithfulness**: Response grounded in sources
**Relevance**: Response answers the question
**Citation Quality**: All claims cited
**Completeness**: No missing key information

**Evaluation Methods**:
- **Human Evaluation**: Analysts rate responses
- **LLM-as-Judge**: GPT-4 evaluates quality
- **Automated**: ROUGE, BLEU for summarization

---

## Advanced RAG Patterns

### Agentic RAG

**LLM decides when to retrieve and what to search for**

```
User Query → LLM (decides to search) → Retrieval → LLM (synthesizes)
              ↑                                        ↓
              └────────── Iterates if needed ──────────┘
```

**Use Case**: Complex multi-hop questions

---

### Multi-Index RAG

**Separate vector DBs for different document types**

```
User Query → Router → [Intel Reports Index]
                   → [SIGINT Index]
                   → [HUMINT Index]
                   → [OSINT Index]
                   ↓
            Combine results → LLM Generation
```

**Benefits**: Optimized embeddings per type, clearer citations

---

### Temporal RAG

**Weight recent documents higher**

```python
def temporal_score(similarity, doc_date):
    age_days = (today - doc_date).days
    decay_factor = 0.99 ** age_days  # Exponential decay
    return similarity * decay_factor
```

**Use Case**: Intelligence where recent info matters more

---

### Knowledge Graph-Enhanced RAG

**Combine vector search with graph traversal**

```
User Query → Vector Search (find entities)
          → Graph Traversal (find related entities)
          → Retrieve docs for all entities
          → LLM Generation
```

**Use Case**: Relationship-heavy queries ("Who does X work with?")

---

## Reference Architecture: Intelligence RAG System

```
┌─────────────────────────────────────────────────────────┐
│ Document Ingestion Pipeline                             │
├─────────────────────────────────────────────────────────┤
│ Documents (PDF, Word, Classified) → Parser → Chunker   │
│    ↓                                                     │
│ Embedding Model (sentence-transformers, on-prem)       │
│    ↓                                                     │
│ Vector DB (Weaviate, self-hosted, classified network)  │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ Query Pipeline                                           │
├─────────────────────────────────────────────────────────┤
│ User Query → Embed → Hybrid Search (vector + keyword)   │
│    ↓                                                     │
│ Metadata Filtering (classification, date, source)      │
│    ↓                                                     │
│ Reranking (Cohere or cross-encoder)                    │
│    ↓                                                     │
│ Top-10 Retrieved Documents                             │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ Generation Pipeline                                      │
├─────────────────────────────────────────────────────────┤
│ Prompt Construction:                                     │
│   - System: Intelligence analyst instructions          │
│   - Context: Retrieved docs with classifications       │
│   - Query: User question                               │
│    ↓                                                     │
│ LLM (GPT-4 or Claude 3.5)                              │
│    ↓                                                     │
│ Response with Inline Citations                         │
│    ↓                                                     │
│ Post-Processing:                                        │
│   - Validate citations exist                           │
│   - Add classification markings                        │
│   - Format for intelligence report                     │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ Human Review                                             │
├─────────────────────────────────────────────────────────┤
│ Analyst reviews generated report                        │
│ Validates sources and citations                         │
│ Approves for dissemination                             │
└─────────────────────────────────────────────────────────┘
```

---

## Security Considerations

### Classification Handling
- Embed classification metadata with chunks
- Filter retrieval by user clearance level
- Never mix classified and unclassified in same index
- Apply highest classification to generated response

### Access Control
- User authentication and authorization
- Audit log all queries and retrievals
- Need-to-know enforcement via metadata filtering
- Track dissemination via ORCON markings

### Data Isolation
- Separate vector DBs by classification level
- Network segmentation (SCIF for TS/SCI)
- Encryption at rest (FIPS 140-2)
- No external API calls for classified data (self-host)

---

## Cost Optimization

### Embedding Costs
- **Batch embed** documents (not one-by-one)
- **Cache embeddings** (don't re-embed same text)
- **Use smaller models** for cost-sensitive apps

### LLM Costs
- **Minimize context** (only include relevant chunks)
- **Cache responses** for common queries
- **Use cheaper models** for simple queries (GPT-3.5 vs GPT-4)

### Example Cost Analysis
- 1M intelligence documents
- Average 2K tokens per document
- Embed with OpenAI small: $40
- Store in Pinecone: $70/month
- Query with GPT-4: $0.01-$0.03 per query
- **Total initial**: $110, **ongoing**: $70/month + query costs

---

## Resources

- [RAG Survey Paper](https://arxiv.org/abs/2312.10997)
- [LlamaIndex RAG Guide](https://docs.llamaindex.ai/)
- [LangChain RAG Tutorial](https://python.langchain.com/docs/use_cases/question_answering/)
- [Pinecone RAG Handbook](https://www.pinecone.io/learn/retrieval-augmented-generation/)
- [Weaviate RAG](https://weaviate.io/developers/weaviate/more-resources/rag)
