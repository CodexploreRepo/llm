# Retrieval-Augmented Generation (RAG)

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
