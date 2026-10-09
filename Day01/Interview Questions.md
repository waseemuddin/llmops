### Q: What is a token?
A token is a unit of text that an LLM processes. A tokenizer converts text into tokens and maps those tokens to numerical IDs before they enter the model.
###  Q: Does an LLM generate the entire response simultaneously?
Generally no. Autoregressive LLMs predict the next token based on the existing sequence and repeat this process until generation stops.
###  Q: Why does token count matter to an LLMOps engineer?
Because it influences context usage, inference compute, latency, KV-cache memory, throughput and often API cost.
###  Q: What is the difference between tokenization and embeddings?
Tokenization breaks text into discrete units and maps them to IDs. Embedding maps those discrete representations into continuous numerical vectors that neural networks can process.
###  Q: What is inference?
Inference is running a trained model to generate predictions/output. For a generative LLM, that generally means repeatedly predicting subsequent tokens from the prompt and already-generated tokens.