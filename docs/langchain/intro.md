# Lang Chain

## Summary

- What is Langchain ? to develop LLM-powered application, get access to the LLM API
- **LangChain-core**: LangChain Expression Language (LCEL) - unified interface
  - ChatPromptTemplate, Output Parsers
  - Message: HumanMessage, ToolMessage: output from tool or function
  - Pipeline orchestrationyou want to specialised agent to search on internet, another agent to call LLM API
  - Parallelization, fallbacks, batching, streaming, async composition
- **LangChain-Community**:
  - Model I/O: chat model
  - Retriever: document loader (like can load from jira, confluence, csv), vector store, text splitter, embedding models
  - Agent tooling: provide more capablity to agent - Tool, Toolkit
- **LangSmith**: a portal to trace the flow what is recevied from the LLM, and response from the users
  - Observability:
  - Debug:
  - Evaluation
  - Annotation
- **LangServe**: deployment - to expose the API to be used by other application - based on Fast API & provide the "LangServe Playground" portal localhost/rag/playground
- **LangGraph**: workflow orchestration
- Example of Langchain workflow:
  - `prompt = ChatPromptTemplate.from("tell me about the {topic}")`
  - `model = ChatAnthropic(model="claude-3")`
  - `output_parser = StrOutputParser()`
  - `chain = prompt | model | output_parser # create the chain by combining prompt, LLM model and output parser`
  - `chain.invoke({"topic":"bear"})`
  - You can invoke any component in the chain, such as:

```Python
prompt_value = prompt.invoke({"topic": "bears"})
message = model.invoke(prompt_value)
```

- Example of RAG:
  - Model: `ChatAnthropic("claude-3")`
  - Vector store: `langchain_community.vectorstores import DocArrayInMemorySearch` (in-memory -> no need to setup infra for this vectorDB)
  - Retriever = `vectorstore.as_retriever()` -> this help to find the relavent documents from the user query
    - can limit how many document it will be retrieved
  - Embedding: `OpenAIEmbeddings from langchain_openai`
  - Prompt: `ChatPromptTEmplate.from_template(template)`
    `template = ""Anser this question based on the following context: {context}, Question: {question}"`
  - Output_parser:
  - `setup_and_retrieval = RunnableParallel({"context": retriever, "question"" RunnablePassthrough()})`
  - `chain = setup_and_retrieval | prompt | model | output_parser`
  - `chain.invoke("What is EFFR meaning")` -> Based on the context EFFR is effective
  - Try to ask out of context: chain.invoke("what is ilu") # LLM: sorry the context is not given
- Example of complexed Langchain workflow: LangGraph
  - Agent node: decide to use a tool if we go to vector store or going to the internet based on the query
    - if search in the internet, we can create function (tool) `covert_to_openai_tool` (function_name) -> `model.bind(tools=tool)` binding the tool to the LLM function to tell the model here are the list of tool, you can decide which one to use
  - Define graph: graph = MessageGraph()

## Intro

- Lang Chain is an open-source development framework Large Language Models (LLMs).


## Installation

```python
pip install --upgrade langchain
```

## Models

### LLM

- `temperature` to control randomness and creativity of the generated text by an LLM
  - `temperature=0.7` default value
  - `temperature=0.0` no randomness

## Memory

- Memory, which is basically how do you remember previous parts of the conversation and feed that into the language model so that they can have this conversational flow as you're interacting with them.
- If the conversation becomes long, the amounts of memory needed becomes really, really long and does the cost of sending a lot of tokens to the LLM, which usually charges based on the number of tokens it needs to process, will also become more expensive.

### Memory Types

- `ConversationBufferMemory` allows for storing of messages and then extracts the messages in a variable
- `ConversationBufferWindowMemory` keeps a list of the interactions of the conversation over time.
  - It only uses the last `k` interactions
- `ConversationTokenBufferMemory` keeps a buffer of recent interactions in memory by token length rather than number of interactions to determine when to flush previous interaction tokens
- `ConversationSummaryMemory` creates a summary of the converstation over time
- **Vector data memory**: stores text (from converstaion or else where) in a vector databsae and retrieves the most relevant blocks of text
- **Entity memories**: using an LLM, it remembers details about specific
