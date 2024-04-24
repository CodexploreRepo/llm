# Retrieval-Augmented Generation (RAG)

## Introduction

- RAG: augmenting LLM knowledge with additional data
  - Indexing:
    - Load (the data in internet),
    - Standardise in a same format (langchain called as document),
    - Split into chunks
    - Embeded using Embedding models
    - Store in vector database
  - Retrieval & Generation
    - Retrival: select the relevant data with your questions
    - Generation: generate the prompt wih the relevant data + LLM -> answer

## Current Limitations

- A model can take a long context doesn’t mean that it can efficiently leverage all the information. Reranking is needed.
- Today, most RAG systems are still text-based, but we’re seeing exciting work on RAG for tabular data and multimodal data. There are also discussions on new techniques for RAG such as RAFT (RAFT: Adapting Language Model to Domain Specific RAG) which combines finetuning and RAG.
- RAG Evaluation: very challenging as we need to evaluate not only the system end-to-end but also different components of the system, such as the embedding quality and retrieval quality.
- RAG Scalability - how to make RAG work with a lot of data (such as millions of text chunks or 200K of lines of code): you might need to filter out data by metadata before doing semantic search for retrieval.
