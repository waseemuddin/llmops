Day 2 interview checkpoint

What is an embedding?

A dense numerical vector representing information in a form that allows models or retrieval systems to work with semantic relationships.

Token vs embedding?
A token is a discrete unit produced by tokenization. An embedding is a continuous numerical vector representation.

What is cosine similarity?
A measure of similarity based on the angle between vectors.

Why use embeddings instead of keyword search?
Embeddings can retrieve semantically related content even when the query and document don't use exactly the same words.

How are embeddings used in RAG?

Documents → chunks → embeddings → vector DB

Question → embedding → similarity search
                     ↓
              relevant chunks
                     ↓
                    LLM

Does the vector database generate embeddings?

Not necessarily. Usually an embedding model generates the vectors, while the vector database stores, indexes and searches them. Some managed systems can integrate embedding generation.


