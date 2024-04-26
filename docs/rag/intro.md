# Retrieval-Augmented Generation (RAG)

## Introduction

- Retrieval-Augmented Generation (or RAG) is an architecture used to help large language models like GPT-4 provide better responses by using relevant information from additional sources and reducing the chances that an LLM will leak sensitive data, or ‘hallucinate’ incorrect or misleading information.
- In summary - RAG: augmenting LLM knowledge with additional data
  - Indexing:
    - Load (the data in internet),
    - Standardise in a same format (langchain called as document),
    - Split into chunks
    - Embeded using Embedding models such as OpenAI's `text-embedding-ada-002` ([see here for more information](https://platform.openai.com/docs/guides/embeddings/use-cases))
    - Store in vector database
  - Retrieval & Generation
    - Retrival: select the relevant data with your questions
    - Generation: generate the prompt wih the relevant data + LLM -> answer

## Current Limitations

- A model can take a long context doesn’t mean that it can efficiently leverage all the information. Reranking is needed.
- Today, most RAG systems are still text-based, but we’re seeing exciting work on RAG for tabular data and multimodal data. There are also discussions on new techniques for RAG such as RAFT (RAFT: Adapting Language Model to Domain Specific RAG) which combines finetuning and RAG.
- RAG Evaluation: very challenging as we need to evaluate not only the system end-to-end but also different components of the system, such as the embedding quality and retrieval quality.
- RAG Scalability - how to make RAG work with a lot of data (such as millions of text chunks or 200K of lines of code): you might need to filter out data by metadata before doing semantic search for retrieval.
