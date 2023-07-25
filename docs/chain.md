# Chain
## Sequential Chain
- **Sequential Chain** to combine multiple chains where the output of the one chain is the input of the next chain
- There is 2 types of sequential chains:
  - `SimpleSequentialChain` single Input & Output
  - `SequentialChain` multiple Input & Output
### Simple Sequential Chain
<p align="center"><img width=600 src="https://github.com/CodexploreRepo/llm/assets/64508435/0b30ee0e-54f1-4d4d-b522-12c9071cd0b8"></p>

### Sequential Chain
<p align="center"><img width=700 src="https://github.com/CodexploreRepo/llm/assets/64508435/f8769451-1ae1-46ad-abe9-2e51796e52d6"></p>

## Router Chain
- **Router Chain** is to route an input to a chain depending on what exactly that input is
<p align="center"><img width=700 src="https://github.com/CodexploreRepo/llm/assets/64508435/aaf961fb-60b1-4440-afa7-f65ea1494aa6"></p>

- For example, we are routing between different types of chains depending on the subject that seems to come in.
```Python
# One prompt is good for answering physics questions. 
physics_template = """You are a very smart physics professor. \
You are great at answering questions about physics in a concise\
and easy to understand manner. \
When you don't know the answer to a question you admit\
that you don't know.

Here is a question:
{input}"""

# The second prompt is good for answering math questions
math_template = """You are a very good mathematician. \
You are great at answering math questions. \
You are so good because you are able to break down \
hard problems into their component parts, 
answer the component parts, and then put them together\
to answer the broader question.

Here is a question:
{input}"""

# The third for history
history_template = """You are a very good historian. \
You have an excellent knowledge of and understanding of people,\
events and contexts from a range of historical periods. \
You have the ability to think, reflect, debate, discuss and \
evaluate the past. You have a respect for historical evidence\
and the ability to make use of it to support your explanations \
and judgements.

Here is a question:
{input}"""

# The fourth for computer science
computerscience_template = """ You are a successful computer scientist.\
You have a passion for creativity, collaboration,\
forward-thinking, confidence, strong problem-solving capabilities,\
understanding of theories and algorithms, and excellent communication \
skills. You are great at answering coding questions. \
You are so good because you know how to solve a problem by \
describing the solution in imperative steps \
that a machine can easily interpret and you know how to \
choose a solution that has a good balance between \
time complexity and space complexity. 

Here is a question:
{input}"""
```
  - Create `prompt_infos` list and define `ChatPromptTemplate` & and make it as `LLMChain` (i.e combining `ChatPromptTemplate` & `llm` model together)

```Python
# define an LLM: in this case we use gpt-3.5-turbo from OpenAI
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(temperature=0)

# define prompt_infos list 
prompt_infos = [
    {
        "name": "physics", 
        "description": "Good for answering questions about physics", 
        "prompt_template": physics_template
    },
    {
        "name": "math", 
        "description": "Good for answering math questions", 
        "prompt_template": math_template
    },
    {
        "name": "history", 
        "description": "Good for answering history questions", 
        "prompt_template": history_template
    },
    {
        "name": "computer science", 
        "description": "Good for answering computer science questions", 
        "prompt_template": computerscience_template
    }
]

# define `ChatPromptTemplate` for each subject & and make it as `LLMChain` & store in destination_chains
from langchain.chains import LLMChain # this is to  combine ChatPromptTemplate & llm model together as a single node in chain

destination_chains = {}
for p_info in prompt_infos:
    name = p_info["name"]
    prompt_template = p_info["prompt_template"]
    prompt = ChatPromptTemplate.from_template(template=prompt_template) # define the prompt template
    chain = LLMChain(llm=llm, prompt=prompt)                            # make it as LLM Chain
    destination_chains[name] = chain                                    # store in destination_chains dict

# create destinations of each route   
destinations = [f"{p['name']}: {p['description']}" for p in prompt_infos]
destinations_str = "\n".join(destinations)
print(destinations_str)
"""
physics: Good for answering questions about physics
math: Good for answering math questions
History: Good for answering history questions
computer science: Good for answering computer science questions
"""
```
- Create default prompt & chain in-case the input is not above to map with any above defined subjects
```Python
default_prompt = ChatPromptTemplate.from_template("{input}") # create default prompt which only receive the user input
default_chain = LLMChain(llm=llm, prompt=default_prompt)     # convert it to default chain
```
