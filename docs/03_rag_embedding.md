# RAG, Embedding and Vector Databases

Retrieval Augmented Generation (RAG) combines the reasoning abilities of language models with the accuracy of external knowledge bases. In week three we learn to build systems that fetch relevant context and feed it to a generator model.

## 1. The RAG Workflow
1. **Query Encoding**: The user question or prompt is embedded using a transformer encoder.
2. **Vector Retrieval**: The embedding is compared to vectors stored in a database. The most similar passages are retrieved.
3. **Generation**: The retrieved text chunks are provided to a language model which produces a final answer grounded in that context.

By retrieving supporting evidence, RAG reduces hallucination and enables up-to-date information without retraining the model.

## 2. Embedding Models
Sentence transformers (e.g., `all-MiniLM` or `bge-large`) convert text into high-dimensional vectors. These vectors capture semantic meaning, allowing similarity search. Fine-tuning on domain data improves retrieval quality.

### Training Strategies
- **Unsupervised**: Use contrastive learning on large corpora to build general-purpose embeddings.
- **Supervised**: Train on question–answer pairs or labelled data for more accurate retrieval.

## 3. Vector Databases
Specialized databases store millions of embeddings and provide efficient similarity search. Popular options include:
- **FAISS**: A library for indexing and searching high-dimensional vectors. It offers product quantization and GPU acceleration.
- **Qdrant** and **Weaviate**: Standalone services that store vectors with associated metadata. They support filtering, sharding and persistence.

### Indexing
Vectors are often compressed or quantized for speed. Inverted file (IVF) indexes and HNSW graphs allow approximate nearest-neighbor search with controllable accuracy.

## 4. Building a RAG System
1. Chunk documents into passages (e.g., 200–300 words).
2. Embed each chunk and store it in a vector database along with metadata.
3. When a query arrives, encode it and retrieve the top-k similar chunks.
4. Assemble a prompt that includes the retrieved context and ask the language model to answer based only on that information.

### Example Code Snippet
```python
from langchain.embeddings import HuggingFaceEmbeddings
from langchain.vectorstores import FAISS

embed = HuggingFaceEmbeddings(model_name='all-MiniLM-L6-v2')
texts = ["Document chunk one", "Another chunk"]
store = FAISS.from_texts(texts, embed)
question = "What is in the document?"
matched_docs = store.similarity_search(question)
```

## 5. Challenges and Best Practices
- **Chunking Strategy**: Overlapping windows can preserve context but increase storage.
- **Metadata**: Storing source links allows the generation step to cite references.
- **Freshness**: Periodically re-embed new documents so the knowledge base stays current.

## 6. Implementation Exercise
Create a simple question-answering system that reads a set of articles, embeds them into a FAISS index and uses an LLM to respond to user questions with retrieved passages. Evaluate how retrieval quality changes when using different embedding models or index parameters.

By completing week three you will know how to integrate vector search with language models to build applications that provide grounded, accurate answers.
