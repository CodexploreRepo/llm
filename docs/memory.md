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
- Check what is store inside the memory
```Python
print(memory.buffer)
"""
Human: Hi, my name is Andrew
AI: Hello Andrew! It's nice to meet you. How can I assist you today?
Human: What is 1+1?
AI: 1+1 is equal to 2.
Human: What is my name?
AI: Your name is Andrew.
Human: What is my name?
AI: Your name is Andrew.
"""
print(memory.load_memory_variables({}))
"""
{'history': "Human: Hi, my name is Andrew\nAI: Hello Andrew! It's nice to meet you. How can I assist you today?\nHuman: What is 1+1?\nAI: 1+1 is equal to 2.\nHuman: What is my name?\nAI: Your name is Andrew.\nHuman: What is my name?\nAI: Your name is Andrew."}
"""
```

### `ConversationBufferWindowMemory`
-  keeps a list of `k` interactions of the conversation over time.
```Python
from langchain.memory import ConversationBufferWindowMemory

memory = ConversationBufferWindowMemory(k=1) # k=1, keep track on the most recent converstation

memory.save_context({"input": "Hi"},
                    {"output": "What's up"})
memory.save_context({"input": "Not much, just hanging"},
                    {"output": "Cool"})

print(memory.load_memory_variables({}))
# {'history': 'Human: Not much, just hanging\nAI: Cool'}
```

### `ConversationTokenBufferMemory` 
- keeps a buffer of recent interactions in memory by token length rather than number of interactions to determine when to flush previous interaction tokens
```Python
from langchain.memory import ConversationTokenBufferMemory
# define max_token_limit=40
memory = ConversationTokenBufferMemory(llm=llm, max_token_limit=40)

memory.save_context({"input": "AI is what?!"},
                    {"output": "Amazing!"})
memory.save_context({"input": "Backpropagation is what?"},
                    {"output": "Beautiful!"})
memory.save_context({"input": "Chatbots are what?"}, 
                    {"output": "Charming!"})
print(memory.load_memory_variables({}))

# {'history': 'Human: Backpropagation is what?\nAI: Beautiful!\nHuman: Chatbots are what?\nAI: Charming!'}
```

### ConversationSummaryMemory
- `ConversationSummaryMemory`: use an LLM to write a summary of the conversation so far, and let that be the memory.
```Python
# create a long string
schedule = "There is a meeting at 8am with your product team. \
You will need your powerpoint presentation prepared. \
9am-12pm have time to work on your LangChain \
project which will go quickly because Langchain is such a powerful tool. \
At Noon, lunch at the italian resturant with a customer who is driving \
from over an hour away to meet you to understand the latest in AI. \
Be sure to bring your laptop to show the latest LLM demo."

memory = ConversationSummaryBufferMemory(llm=llm, max_token_limit=100)
memory.save_context({"input": "Hello"}, {"output": "What's up"})
memory.save_context({"input": "Not much, just hanging"},
                    {"output": "Cool"})
memory.save_context({"input": "What is on the schedule today?"}, 
                    {"output": f"{schedule}"})

print(memory.load_memory_variables({}))
"""
{'history': 'System: The human and AI exchange greetings. The human mentions that they are not doing much. The AI informs the human about their schedule for the day, including a meeting with the product team, working on the LangChain project, and having lunch with a customer to discuss the latest in AI. The AI also reminds the human to bring their laptop to show a demo.'}
"""
```
  
