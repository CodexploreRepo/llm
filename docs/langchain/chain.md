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
from langchain.prompts import ChatPromptTemplate
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
[
  'physics: Good for answering questions about physics',
  'math: Good for answering math questions',
  'history: Good for answering history questions',
  'computer science: Good for answering computer science questions'
]
"""
```

- Create default prompt & chain in-case the input is not above to map with any above defined subjects

```Python
default_prompt = ChatPromptTemplate.from_template("{input}") # create default prompt which only receive the user input
default_chain = LLMChain(llm=llm, prompt=default_prompt)     # convert it to default chain
```

- Define `MULTI_PROMPT_ROUTER_TEMPLATE`

````Python
MULTI_PROMPT_ROUTER_TEMPLATE = """Given a raw text input to a \
language model select the model prompt best suited for the input. \
You will be given the names of the available prompts and a \
description of what the prompt is best suited for. \
You may also revise the original input if you think that revising\
it will ultimately lead to a better response from the language model.

<< FORMATTING >>
Return a markdown code snippet with a JSON object formatted to look like:
```json
{{{{
    "destination": string \ name of the prompt to use or "DEFAULT"
    "next_inputs": string \ a potentially modified version of the original input
}}}}
```

REMEMBER: "destination" MUST be one of the candidate prompt \
names specified below OR it can be "DEFAULT" if the input is not\
well suited for any of the candidate prompts.
REMEMBER: "next_inputs" can just be the original input \
if you don't think any modifications are needed.

<< CANDIDATE PROMPTS >>
{destinations}

<< INPUT >>
{{input}}

<< OUTPUT (remember to include the ```json)>>


Example:
<< INPUT >>
"What is black body radiation?"
<< OUTPUT >>
```json
{{{{
"destination": "physics",
"next_inputs": "What is black body radiation?"
}}}}
```
"""
````

- Create the Prompt Template for the Multi Prompt Router

```Python
router_template = MULTI_PROMPT_ROUTER_TEMPLATE.format(
    destinations=destinations_str
)

from langchain.prompts import PromptTemplate
from langchain.chains.router.llm_router import RouterOutputParser # Router Output Parser

router_prompt = PromptTemplate(
    template=router_template,
    input_variables=["input"],
    output_parser=RouterOutputParser(),
)
```

- Define a router chain

```Python
router_chain = LLMRouterChain.from_llm(llm, router_prompt)
```

- Create a multi prompt chain by combining a `router_chain` and `destination_chains`

```Python
chain = MultiPromptChain(router_chain=router_chain,
                         destination_chains=destination_chains,
                         default_chain=default_chain, verbose=True
                        )
chain.run("What is black body radiation?")
"""
> Entering new MultiPromptChain chain...
physics: {'input': 'What is black body radiation?'}
> Finished chain.
'Black body radiation refers to the electromagnetic radiation emitted by an object that absorbs all incident radiation and reflects or transmits none. It is called "black body" because it absorbs all wavelengths of light, appearing black at room temperature. \n\nAccording to Planck\'s law, black body radiation is characterized by a continuous spectrum of wavelengths and intensities, which depend on the temperature of the object. As the temperature increases, the peak intensity of the radiation shifts to shorter wavelengths, resulting in a change in color from red to orange, yellow, white, and eventually blue at very high temperatures.\n\nBlack body radiation is a fundamental concept in physics and has significant applications in various fields, including astrophysics, thermodynamics, and quantum mechanics. It played a crucial role in the development of quantum theory and understanding the behavior of light and matter.'
"""
chain.run("Why does every cell in our body contain DNA?")
"""
> Entering new MultiPromptChain chain...
None: {'input': 'Why does every cell in our body contain DNA?'}
> Finished chain.
'Every cell in our body contains DNA because DNA is the genetic material that carries the instructions for the development, functioning, and reproduction of all living organisms. DNA contains the information necessary for the synthesis of proteins, which are essential for the structure and function of cells. It serves as a blueprint for the production of specific proteins that determine the characteristics and traits of an organism. Additionally, DNA is responsible for the transmission of genetic information from one generation to the next during reproduction. Therefore, every cell in our body contains DNA to ensure the proper functioning and continuity of life.'
"""
```

-
