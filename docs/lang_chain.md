# Lang Chain

## Intro
- Lang Chain is an open-source development framework Large Language Models (LLMs).
- Lang Chain components:
  - **Models**: there are 3 types of models
    - LLMs: 20+ integrations
    - Chat Models:
    - Text Embedding Models: 20+ integrations
  - **Prompts**: the style of creating inputs to pass into the models 
    - Prompt Templates
    - Output Parsers: convert LLM output to more structured formats
      - Retry, Fixing Logic
    - Example Selectors:  5+ integrations
  - **Indexes**:
    - Document Loaders
    - Text Splitter
    - Vector Stores
    - Retrievers
  - **Chains**:
    - Prompt + LLM + Output Parsing
    - Can be building blocks for larger chains
  - **Agents**:
    - Agent Types: algorithm for getting LLMs to use tools
    - Agent Toolkit: 
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
- Memory, which is basically how  do you remember previous parts of the conversation and feed that into the language model so that they can have this conversational flow as you're interacting with them.
- If the conversation becomes long, the amounts of memory needed becomes really, really long and does the cost of sending a lot of tokens to the LLM, which usually charges based on the number of tokens it needs to process, will also become more expensive.
### Memory Types
- `ConversationBufferMemory` allows for storing of messages and then extracts the messages in a variable
- `ConversationBufferWindowMemory` keeps a list of the interactions of the conversation over time.
  - It only uses the last `k` interactions
- `ConversationTokenBufferMemory` keeps a buffer of recent interactions in memory by token length rather than number of interactions to determine when to flush previous interaction tokens
- `ConversationSummaryMemory` creates a summary of the converstation over time
- **Vector data memory**: stores text (from converstaion or else where) in a vector databsae and retrieves the most relevant blocks of text
- **Entity memories**: using an LLM, it remembers details about specific
