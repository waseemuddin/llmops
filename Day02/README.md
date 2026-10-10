Day 02 of LLMOPS


### What exactly is an embedding?

Computers cannot directly reason over:

### "Kubernetes manages containers"

Neural models work with numbers.
An embedding model converts text into a numerical vector:
"Kubernetes manages containers"
              ↓
        Embedding Model
              ↓
[0.124, -0.351, 0.782, 0.091, ...]

Suppose, purely for illustration:

"Kubernetes manages containers"
       [0.82, 0.71, 0.15]

"K8s orchestrates container workloads"
        [0.79, 0.74, 0.18]

Their vectors should point in relatively similar directions because the sentences have related meanings.

But:
"I like chocolate cake"

[-0.31, 0.12, 0.91]
should be farther away.
That property enables semantic search.

### Token embeddings vs sentence/document embeddings

Yesterday we discussed:

```text
Token
 ↓
Token ID
 ↓
Token Embedding
```

Inside a transformer, individual tokens are represented as vectors.

"Kubernetes manages containers"
              ↓
        Embedding Model
              ↓
    One vector representing
       the entire sentence

Later in RAG:

Document
   ↓
Chunk
   ↓
Embedding Model
   ↓
Vector
   ↓
Vector Database

For example:
Document 1
"Kubernetes provides container orchestration."

       ↓

[0.21, -0.53, 0.84, ....]

The database stores that vector alongside the original text and metadata.