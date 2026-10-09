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
                  
A tokenizer is a software tool or algorithm that converts raw text into smaller pieces called tokens (words, subwords, characters, or bytes) and maps them to unique numerical IDs.

![token](/img/token.png)
