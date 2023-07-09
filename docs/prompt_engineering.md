# Prompt Engineering with LangChain
## Prompt Template
- Define a template string (template format following f-string)
```Python
template_string = """Translate the text \
that is delimited by triple backticks \
into a style that is {style}. \
text: ```{text}```
"""
```
- Create a prompt template
```Python
from langchain.prompts import ChatPromptTemplate
# create a prompt template from template_string with 2 input variables
prompt_template = ChatPromptTemplate.from_template(template_string)

# to view input variables, in this case: ['style', 'text']
prompt_template.messages[0].prompt.input_variables
```
- Format the prompt with two inputs 
```Python
# input 1: style
customer_style = """American English \
in a calm and respectful tone
"""
# input 2: context
customer_email = """
Arrr, I be fuming that me blender lid \
flew off and splattered me kitchen walls \
with smoothie! And to make matters worse, \
the warranty don't cover the cost of \
cleaning up me kitchen. I need yer help \
right now, matey!
"""
# format the prompt with two inputs 
customer_messages = prompt_template.format_messages(
                    style=customer_style,
                    text=customer_email)
print(type(customer_messages))       # <class 'list'>
print(type(customer_messages[0]))    # <class 'langchain.schema.HumanMessage'>
```
- Call the LLM to perform the prompt
```Python
from langchain.chat_models import ChatOpenAI
# to control the randomness and creativity of the generated text by an LLM, use temperature = 0.0
chat = ChatOpenAI(temperature=0.0)

# call the LLM to translate to the style of the customer message
customer_response = chat(customer_messages)

print(customer_response.content)
"""
Arrr, I am quite upset that my blender lid flew off and made a mess on my kitchen walls with smoothie! And to add to my frustration, the warranty does not cover the cost of cleaning up my kitchen. I kindly request your assistance at this moment, matey!
"""
```
- 
## Ouput Parser
- Chain-of-thought Reasoning (ReAct):
  - `Thought`: give a topic to LLM to think
  - `Action`: to carry a specific action
  - `Observation`: to show what it learn from that action
- If you have a prompt that instructs the LLM to use these specific keywords, thought, action, and observation, then this prompt can be coupled with a parser to extract out the text that has been tagged with these specific keywords.
