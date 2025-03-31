# README: Advanced Legal Document RAG Pipeline for Mortgage Documents

## Overview

This project focuses on creating a sophisticated Retrieval-Augmented Generation (RAG) pipeline for extracting information from legal mortgage documents. A key component of this pipeline is a custom **Hybrid Retriever** class, designed to enhance retrieval accuracy by combining semantic and keyword-based search methods. Additionally, we explore the impact of reranking on retrieval results, demonstrating the significance of this step in improving the relevance of the output.

## Custom Hybrid Retriever Class

### Purpose

The `HybridRetriever` class was developed to address the unique challenges of retrieving information from legal documents, where both semantic context and precise legal terminology are crucial. Traditional retrieval methods might fall short in capturing the nuanced balance required in legal texts.

### Implementation Details

- **Class Definition**: `HybridRetriever` inherits from `BaseRetriever` in LlamaIndex, allowing it to integrate seamlessly with the RAG framework.

- **Initialization**:
  ```python
  def __init__(self, vector_retriever, bm25_retriever, vector_weight=0.5, top_k_per_retriever=10, top_n=10, dedup_threshold=0.9):
  ```
  - **Vector Retriever**: Utilizes embeddings for semantic search, capturing the context and related concepts within legal documents.
  - **BM25 Retriever**: Employs the BM25 algorithm for keyword-based retrieval, which is effective for matching specific legal terms.
  - **Parameters**:
    - `vector_weight`: Balances the influence of vector and BM25 scores.
    - `top_k_per_retriever`: Limits the number of results from each retriever before fusion.
    - `top_n`: Final number of results returned after fusion and deduplication.
    - `dedup_threshold`: Threshold for deduplication to avoid redundant results.

- **Retrieval Process**:
  - **Parallel Retrieval**: Uses threading to concurrently fetch results from both retrievers, enhancing efficiency.
  - **Score Normalization**: Normalizes scores from both retrievers to ensure fair comparison.
  - **Score Fusion**: Combines scores using the `vector_weight` to produce a hybrid score.
  - **Deduplication**: Removes duplicate entries based on document IDs to provide unique results.
  - **Final Selection**: Limits the output to `top_n` results, ensuring a manageable set of relevant documents.

### Learning Outcomes

- **Understanding Retrieval Techniques**: Gained insight into how different retrieval methods (semantic vs. keyword) can be combined to leverage their strengths in a legal context.
- **Custom Class Development**: Learned to extend existing frameworks by creating custom classes that integrate multiple retrieval strategies.
- **Efficiency and Performance**: Explored how parallel processing can improve the performance of retrieval systems.

## Reranking: Comparison and Significance

### Without Reranking

Initially, the retrieval results are based solely on the hybrid scoring mechanism. This might lead to:
- **Relevance Issues**: Some results might be technically relevant but not contextually appropriate due to the complexity of legal language.
- **Ambiguity**: Legal documents often contain ambiguous terms where keyword matching might not capture the intended context.

### With Reranking

Reranking uses an LLM (Language Model) to reorder the retrieved documents:
- **Contextual Relevance**: The LLM assesses the context of each document in relation to the query, improving the order of results based on nuanced understanding.
- **User Intent**: It better aligns results with the user's intent, which might not be fully captured by initial retrieval methods.

### Significance of Reranking

- **Enhanced Precision**: Reranking significantly improves the precision of results by prioritizing documents that are not just keyword matches but are contextually relevant.
- **Handling Legal Nuances**: Legal documents often require understanding beyond simple keyword presence. Reranking helps in interpreting legal nuances, clauses, and conditions.
- **User Experience**: By providing more relevant results at the top, it reduces the time users spend sifting through less relevant information, enhancing the practical utility of the system.

### Learning Outcomes from Reranking

- **Importance of Context**: Understood the critical role of context in legal document retrieval, beyond mere keyword matching.
- **LLM Capabilities**: Explored how advanced language models can be used to refine search results, showcasing the power of AI in understanding complex texts.
- **Practical Application**: Demonstrated the real-world application of reranking in improving the efficiency and accuracy of information retrieval in specialized domains like law.

## Conclusion

This project not only showcases the technical implementation of a RAG pipeline but also highlights the learning journey in developing custom retrieval solutions and understanding the impact of reranking in legal document analysis. It's a practical demonstration of how AI can be applied to enhance productivity in legal, financial, and real estate sectors by making document retrieval more precise and user-friendly.
