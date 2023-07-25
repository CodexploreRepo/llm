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
