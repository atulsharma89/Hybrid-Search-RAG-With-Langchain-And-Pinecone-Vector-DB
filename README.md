# Hybrid-Search-RAG-With-Langchain-And-Pinecone-Vector-DB


# Hybrid Search with Langchain and Pinecone Vector DB

This project demonstrates how to implement a hybrid search system using [Langchain](https://python.langchain.com/en/latest/) and [Pinecone](https://www.pinecone.io/), leveraging Pinecone's vector database for fast and scalable similarity search. The hybrid search combines both sparse (traditional keyword) and dense (vector-based) retrieval techniques.

## Project Overview

The notebook integrates Langchain with Pinecone to create a hybrid search retriever. This allows users to perform both traditional keyword searches and dense vector searches within a single system, improving search accuracy and relevance.

### Features

- **Pinecone Integration**: Utilizes Pinecone for managing vector-based search.
- **Langchain Hybrid Search Retriever**: Combines dense and sparse search methods for enhanced search results.
- **Serverless Index Creation**: Dynamically creates and manages the index in Pinecone with cloud setup.
- **Retrieval Augmented Generation (RAG)**: Can be integrated with large language models (LLMs) for enhanced question-answering capabilities.

## Setup Instructions

### Prerequisites

- Python 3.7+
- Pinecone API Key (You can get one by signing up at [Pinecone](https://www.pinecone.io/))

### Installation

1. Install the required packages:
   ```bash
   %pip install --upgrade --quiet pinecone-client pinecone-text pinecone-notebooks
   ```

2. Clone the repository or download the notebook file.

3. Set up your Pinecone API key:
   Replace the placeholder with your actual API key in the following cell:
   ```python
   api_key = "your-pinecone-api-key"
   ```

### Pinecone Index Setup

The code automatically checks if the required Pinecone index exists. If not, it creates one with the following specifications:
- **Name**: `langchain-pinecone-hybrid-search`
- **Dimension**: 1536 (specific to the dense model used)
- **Metric**: `dotproduct` (supports sparse values)

### Usage

1. Import necessary modules and initialize the Pinecone client:
   ```python
   from pinecone import Pinecone, ServerlessSpec
   ```

2. Create and use the index for hybrid search:
   ```python
   pc = Pinecone(api_key=api_key)
   index_name = "langchain-pinecone-hybrid-search"
   
   if index_name not in pc.list_indexes().names():
       pc.create_index(name=index_name, dimension=1536, metric="dotproduct", spec=ServerlessSpec(cloud="aws", region="us-east-1"))

   # Use the index for search operations
   index = pc.Index(index_name)
   ```

3. Perform hybrid search using Langchain's `PineconeHybridSearchRetriever`:
   ```python
   from langchain_community.retrievers import PineconeHybridSearchRetriever
   retriever = PineconeHybridSearchRetriever(index=index)
   ```

## Notes

- Ensure that your Pinecone index is set up correctly before performing any searches.
- You can further integrate this setup with large language models (LLMs) to implement Retrieval Augmented Generation (RAG) systems for more advanced use cases.
- Adjust the dimensionality and metric according to the embedding model you use for dense search.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
