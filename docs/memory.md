# Memory
- Memory, which is basically how  do you remember previous parts of the conversation and feed that into the language model so that they can have this conversational flow as you're interacting with them.
- If the conversation becomes long, the amounts of memory needed becomes really, really long and does the cost of sending a lot of tokens to the LLM, which usually charges based on the number of tokens it needs to process, will also become more expensive.
## Memory Types
- `ConversationBufferMemory` allows for storing of messages and then extracts the messages in a variable
- `ConversationBufferWindowMemory` keeps a list of the interactions of the conversation over time.
  - It only uses the last `k` interactions
- `ConversationTokenBufferMemory` keeps a buffer of recent interactions in memory by token length rather than number of interactions to determine when to flush previous interaction tokens
- `ConversationSummaryMemory` creates a summary of the converstation over time
- **Vector data memory**: stores text (from converstaion or else where) in a vector databsae and retrieves the most relevant blocks of text
- **Entity memories**: using an LLM, it remembers details about specific

### `ConversationBufferMemory`
- Define LLM model, in this case is ChatOpenAI or `model=gpt-3.5-turbo`
```Python
# create LLM model
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(temperature=0.0)
```
- Create the `ConversationChain` with `ConversationBufferMemory` memory
```Python
# create a chain & memory
from langchain.chains import ConversationChain
from langchain.memory import ConversationBufferMemory

memory = ConversationBufferMemory()
conversation = ConversationChain(
    llm=llm, 
    memory = memory,
    verbose=True # to display the full output (including the prompt to LLM from LangCahin)
)
```
- Starting a conversation
```Python
conversation.predict(input="Hi, my name is Andrew")

"""
> Entering new ConversationChain chain...
Prompt after formatting:
The following is a friendly conversation between a human and an AI. The AI is talkative and provides lots of specific details from its context. If the AI does not know the answer to a question, it truthfully says it does not know.

Current conversation:

Human: Hi, my name is Andrew
AI:

> Finished chain.
"Hello Andrew! It's nice to meet you. How can I assist you today?"

"""
conversation.predict(input="What is 1+1?")

"""
> Entering new ConversationChain chain...
Prompt after formatting:
The following is a friendly conversation between a human and an AI. The AI is talkative and provides lots of specific details from its context. If the AI does not know the answer to a question, it truthfully says it does not know.

Current conversation:
Human: Hi, my name is Andrew
AI: Hello Andrew! It's nice to meet you. How can I assist you today?
Human: What is 1+1?
AI:

> Finished chain.
'1+1 is equal to 2.'
"""

conversation.predict(input="What is my name?")
# 'Your name is Andrew.'
```
