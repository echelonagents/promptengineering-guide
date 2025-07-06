# Natural Language Processing

Natural Language Processing (NLP) is the discipline focused on understanding and generating human language with computers. In our first week we build the foundational toolkit for working with raw text. The goal is to transform unstructured sentences into representations that can be fed into language models or traditional machine learning pipelines.

## 1. Overview of NLP Tasks
NLP spans a broad range of tasks from tokenization and part-of-speech tagging to machine translation and text generation. We examine the classic pipeline of text classification, named entity recognition and summarization. Each task benefits from accurate preprocessing and token management, topics that we will explore in depth.

### Example
A typical text classification workflow reads a document, splits it into tokens, removes non-essential words and converts the remaining terms into numerical features. These features are then fed into a classifier, such as logistic regression or a neural network, to predict sentiment or other labels.

## 2. Text Preprocessing
Before we can use text in downstream models, we clean and normalize it. Typical steps include lowercasing, removing punctuation and expanding contractions. Tokenization divides the text into units—either words or subwords—so that the model can assign numerical values.
- **Normalization**: Convert text to lowercase and standardize punctuation or white space.
- **Tokenization**: Using rules-based or statistical approaches, break text into words or subwords. Libraries such as spaCy or NLTK provide tokenizers for many languages.
- **Stop Word Removal**: Many analyses remove common words ("the", "and") to focus on informative terms.
- **Stemming and Lemmatization**: Reduce words to their root forms. Stemming uses heuristics to chop off endings, while lemmatization references dictionaries for correct morphological forms.

### Code Example
```python
import spacy
nlp = spacy.load('en_core_web_sm')
doc = nlp("Cats running and dogs run")
print([token.lemma_ for token in doc])
```

## 3. Classical Representations
Early NLP models relied on bag-of-words features. Each document is represented by a vector that counts the occurrences of every term. TF‑IDF weighting scales the counts to emphasize distinctive words and downplay common terms. Despite their simplicity, these representations still perform well in some tasks.

### N-grams
To capture short phrases, we can use n-gram features. A bigram model represents consecutive word pairs; a trigram model uses three-word sequences. These features help with tasks like language detection and topic modeling.

## 4. Distributed Embeddings
Neural embeddings offer dense representations that encode semantic similarity between words. Word2vec and GloVe learn vectors by predicting contexts or factorizing co-occurrence matrices. Similar words appear close together in vector space.

### Contextual Embeddings
Recent models such as ELMo and BERT produce token embeddings that depend on the sentence. A word like "bank" yields different vectors in "river bank" versus "bank account." These embeddings are the starting point for modern large language models.

### Training Word2vec
```python
from gensim.models import Word2Vec
sentences = [['this','is','a','sentence'], ['this','is','another']]
model = Word2Vec(sentences, vector_size=100, window=5, min_count=1)
vector = model.wv['sentence']
```

## 5. Embedding Spaces and Similarity
Once words or sentences are embedded into vectors, we can measure similarity with distances such as cosine or Euclidean metrics. Nearby vectors indicate semantic closeness. This property underpins search, clustering and recommendation systems.

### Vector Arithmetic
Word embeddings support analogies like `king - man + woman ≈ queen`. By subtracting and adding vectors we capture relationships between concepts.

## 6. Semantic Search and Clustering
To build a semantic search engine, we embed user queries and corpus documents into the same vector space. A similarity search (often using approximate nearest neighbors) retrieves documents with vectors nearest to the query vector. Clustering algorithms like K-means can reveal topical structure in the corpus.

## 7. Evaluation Metrics
Common metrics for evaluating NLP pipelines include precision, recall and F1 score for classification, or BLEU and ROUGE for machine translation and summarization. Proper evaluation helps compare algorithms and tune hyperparameters.

## 8. Practical Exercise
The assignment for this module is to implement a small semantic search engine. Using Python libraries such as spaCy and scikit-learn, you will:
1. Preprocess a text dataset by tokenizing, lemmatizing and removing stop words.
2. Generate TF‑IDF features and train a basic classifier.
3. Create sentence embeddings using a transformer model like `all-MiniLM` from the `sentence-transformers` package.
4. Perform a similarity query and evaluate the retrieval quality.

### Sample Setup Code
```python
from sentence_transformers import SentenceTransformer, util
model = SentenceTransformer('all-MiniLM-L6-v2')
corpus = ["NLP is fun", "We study language", "Transformers are powerful"]
corpus_embeddings = model.encode(corpus, convert_to_tensor=True)
query = "language models"
query_embedding = model.encode(query, convert_to_tensor=True)
results = util.semantic_search(query_embedding, corpus_embeddings, top_k=2)
print(results)
```

By the end of week one, you should be comfortable preparing text for advanced models, generating embeddings and measuring similarity. These skills are essential for all subsequent weeks of the course.
