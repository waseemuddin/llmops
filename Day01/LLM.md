Day 1 — LLM Fundamentals: What Actually Happens When You Send a Prompt?

1. The mental model
Suppose you type:
What is Kubernetes?

The LLM does not directly understand this as a sentence.
Conceptually:

"What is Kubernetes?"
        │
        ▼
┌───────────────────┐
│     Tokenizer     │
└─────────┬─────────┘
          │
          ▼
 [Token IDs]
          │
          ▼
┌───────────────────┐
│     Embeddings    │
└─────────┬─────────┘
          │
          ▼
   Numerical vectors
          │
          ▼
┌───────────────────┐
│    Transformer    │
│                   │
│ Self-Attention    │
│ Feed Forward      │
│ Multiple Layers   │
└─────────┬─────────┘
          │
          ▼
 Probability distribution
 over next token
          │
          ▼
     Next Token
          │
          └──────────┐
                     │
                repeat...
                     │
                     ▼
                  Response
                  

![token](/img/token.png)
