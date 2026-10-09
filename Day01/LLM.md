### Day 1 — LLM Fundamentals: What Actually Happens When You Send a Prompt?

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
                  
### A tokenizer is a software tool or algorithm that converts raw text into smaller pieces called tokens (words, subwords, characters, or bytes) and maps them to unique numerical IDs.

![token](/img/token.png)

## There are three types of tokens:

### Word-level Tokens

Divides text into individual words
Easy for languages like English where words are naturally separated
Harder for languages without clear word boundaries
Example: « Today is nice weather » → [« Today », « is », « nice », « weather »]

### Character-level Tokens

Splits text character by character
Example: « Hello » → [« H », « e », « l », « l », « o »]

### Subword-level Tokens

Further divides word units
These divisions are called subwords
Example: « playing » → [« play », « ##ing »]
The ## indicates this subword attaches to a previous one
