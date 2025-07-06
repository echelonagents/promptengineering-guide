# RAG, Embedding and Vector Databases

Retrieval Augmented Generation (RAG) integrates external knowledge retrieval with language model generation. Week three dives into this architecture.

## The RAG Workflow
- Query encoding and retrieval from knowledge sources
- Feeding retrieved contexts into a generator model

## Embedding Models
We survey sentence transformers and other models that convert text into dense vectors suitable for similarity search.

## Vector Databases
Vector stores such as FAISS, Qdrant and Weaviate support fast similarity search at scale. Concepts of indexing, sharding and metadata filtering are discussed.

## Implementation Exercise
Build a simple RAG pipeline that embeds documents, stores them in a vector database and answers questions by retrieving relevant chunks.
