
## Day 03 — Transformer Architecture: From Fundamentals to GPU Memory Engineering

Level: Beginner → Intermediate → Advanced

Duration: 60 minutes

Environment: Ubuntu Linux + Python + PyTorch + Hugging Face

Hardware: CPU sufficient; NVIDIA GPU optional

Career target: Forward Deployed Engineer — LLMOps / AI 
Infrastructure

### PART 1 — TRANSFORMER FUNDAMENTALS

#### What is a Transformer?

A Transformer is a neural network architecture that uses attention mechanisms to process relationships between elements of a sequence.

It was introduced in the 2017 research paper Attention Is All You Need.
Modern language models such as GPT, Llama, Qwen, and many others are based on Transformer architectures or variants.
Let's understand this using your Kubernetes background.

Suppose you ask an LLM:
How does Kubernetes automatically restart failed containers?

The processing pipeline looks approximately like this:

```text

                    USER QUESTION
                          |
                          v
              +----------------------+
              |      TOKENIZER       |
              |                      |
              | Text → Token IDs     |
              +----------------------+
                          |
                          v
              +----------------------+
              |   TOKEN EMBEDDINGS   |
              |                      |
              | IDs → Vectors        |
              +----------------------+
                          |
                          v
              +----------------------+
              | POSITION INFORMATION |
              |                      |
              | Token ordering       |
              +----------------------+
                          |
                          v
          +--------------------------------+
          |       TRANSFORMER BLOCK        |
          |                                |
          |  Normalization                 |
          |        |                       |
          |        v                       |
          |  Multi-Head Self-Attention     |
          |        |                       |
          |  Residual Connection           |
          |        |                       |
          |  Normalization                 |
          |        |                       |
          |        v                       |
          |  Feed-Forward Network          |
          |        |                       |
          |  Residual Connection           |
          +--------------------------------+
                          |
                          v
                  REPEATED LAYERS
                          |
                          v
                 FINAL HIDDEN STATE
                          |
                          v
                 OUTPUT PROJECTION
                          |
                          v
                       LOGITS
                          |
                          v
                 TOKEN SELECTION
                          |
                          v
                    NEXT TOKEN
                          |
                          v
                       REPEAT

```
#### Important: This is a simplified decoder-style Transformer diagram. Exact layer ordering, normalization, positional encoding, and attention implementations vary by architecture.

#### Why should an LLMOps engineer understand this?
Because Transformer architecture affects:
- GPU memory consumption
- Inference latency
- KV-cache size
- Maximum context length
- Throughput
- Multi-GPU requirements
- Quantization
- Model deployment architecture
When deploying a 70B model with vLLM, understanding these concepts helps you make informed infrastructure decisions.

### PART 2 — THREE TYPES OF TRANSFORMERS

#### Encoder-only, decoder-only, and encoder–decoder

There are three major architectural categories.

#### A. Encoder-only Transformer
Examples include BERT and many embedding-model backbones.

```text
Input Sentence
      |
      v
  Tokenizer
      |
      v
  Embeddings
      |
      v
  Encoder Layer 1
      |
      v
  Encoder Layer 2
      |
      v
      ...
      |
      v
Contextual Representations
```
The encoder produces representations of the input.

Typically, encoder self-attention is bidirectional: tokens can attend to preceding and following tokens.

Common uses:

Semantic search, embeddings, classification, named-entity recognition, and information extraction.

### B. Decoder-only Transformer

Examples include GPT-style models and many Llama/Qwen language models.

```text
Prompt
   |
   v
Tokenizer
   |
   v
Embeddings
   |
   v
Decoder Transformer
   |
   v
Next-Token Prediction
   |
   v
Generated Token
   |
   +----> Repeat
```

Decoder-only language models generally use causal attention.

That means a token cannot attend to future tokens during next-token prediction.

#### For example:
```text
Kubernetes manages containerized ..
```
The model predicts the next token using the available preceding context.

This is the architecture most relevant to our future vLLM inference labs.

### C. Encoder–decoder Transformer

Examples include T5 and BART.

```text
                  INPUT TEXT
                      |
                      v
                   ENCODER
                      |
                      v
             Contextual Representations
                      |
                      v
                   DECODER
                      |
                      v
                 OUTPUT TEXT
```
The decoder uses both its previously generated tokens and information produced by the encoder.

These architectures are useful for tasks such as translation and summarization.

![Tab-1](/Day03/img/tab01.png)


### PART 3 — UNDERSTANDING TRANSFORMER COMPONENTS

### Token embeddings
You already understand tokenization.

Let's extend it.
Suppose:
```text
Kubernetes manages containers
```
The tokenizer produces IDs:
```text

[13970, 5677, 7159, 2229, ...]
```

The model looks up learned embedding vectors corresponding to those IDs.

Conceptually:

```text
Token ID
   |
   v
Embedding Table
   |
   v
[0.12, -0.35, 0.84, ...]
```
If a model has a hidden dimension of 768, each token is 
represented by a vector containing 768 values at this stage.

For a sequence of 10 tokens:

```text
Embedding matrix shape:

(10, 768)
```
Where:
- 10 = sequence length
- 768 = hidden dimension
The model processes these vectors through its Transformer layers.

### Positional information

Consider these sentences:

Sentence A: The server restarted the container.
Sentence B: The container restarted the server.

They contain similar words but express different meanings.

The model must account for token positions.

Transformer architectures use positional information in different ways, including learned positional embeddings, sinusoidal encodings, and rotary positional embeddings (RoPE).

RoPE is especially relevant to modern LLM architectures.


### Self-attention

Self-attention helps the model determine how tokens should incorporate information from other tokens.

Consider:
The Kubernetes node failed because it exhausted its memory.

The model can learn relationships among words such as:

```text
node
  |
  +---- failed
  |
  +---- memory
```
These relationships are represented mathematically using attention.

### Query, Key, and Value

Each token representation is transformed into three components:
Q — Query
K — Key
V — Value

Conceptually:

```text

              TOKEN EMBEDDINGS
                     |
          +----------+----------+
          |          |          |
          v          v          v
        QUERY       KEY       VALUE
          |          |          |
          +----------+----------+
                     |
                     v
             ATTENTION SCORES
                     |
                     v
                 SOFTMAX
                     |
                     v
              WEIGHTED VALUES
                     |
                     v
              ATTENTION OUTPUT

```
The standard scaled dot-product attention formula is:

![QKV-1](/Day03/img/qkv01.png)


Where:
- \(Q\) represents queries.
- \(K\) represents keys.
- \(V\) represents values.
- \(d_k\) is the key dimension.
- \(M\) is an optional attention mask, such as a causal mask.

We'll implement a simplified version in Lab 3.2.