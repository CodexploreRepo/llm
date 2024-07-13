# Llama Introduction

## Llama 2

| Llama 2 | Size (weights) | Best for                |
| ------- | -------------- | ----------------------- |
| 7B      | 13.5 GB        | Simpler tasks           |
| 13B     | 26GB           | Ordinary taks           |
| 70B     | 138GB          | Sophisticated reasoning |

### Chat vs Foundation (Base) Models

<p align="center"><img width=400 src="../../assets/img/llama2-family.png"></p>

- **Foundation (Base) model** did not answer the question, but it returns similar questions to the original questions as it was trained to predict the next word in the sequence.
  - Base models will be used to fine-tune for the specific tasks
  - Note: foundation model is not trained to receive the instruction tag.

```Python
### base model
prompt = "What is the capital of France?"
response = llama(prompt,
                 verbose=True,
                 add_inst=False, # since the foundation model didnt train to get the instruction tag, so set add_inst=False to avoid add instruction tag
                 model="togethercomputer/llama-2-7b") # foundation model "llama-2-7b"
# Prompt:
# What is the capital of France?
print(response[:5])
# 10. What is the capital of Germany?
# 11. What is the capital of Greece?
# 12. What is the capital of Hungary?
# 13. What is the capital of Iceland?
# 14. What is the capital of India?
# 15. What is the capital of Indonesia?
```

- Chat model will add in the [INST][/INST] tag around the prompt

```Python
prompt = "What is the capital of France?"
response = llama(prompt,
                 verbose=True,
                 model="togethercomputer/llama-2-7b-chat") # chat version of "llama2-7b chat"
# Prompt:
# [INST]What is the capital of France?[/INST]
print(reponse)
# The capital of France is Paris.
```

### Evaluation

<p align="center"><img width=500 src="../../assets/img/llama-benchmark.png"></p>

#### Quatitative Evaluation of Llama-2 Models

- General Performance: the larger model, the better score

| Llama 2 | Commonsense Reasoning | World Knowledge | Reading Comprehension |
| ------- | --------------------- | --------------- | --------------------- |
| 7B      | 63.9                  | 48.9            | 61.3                  |
| 13B     | 66.9                  | 55.4            | 65.8                  |
| 70B     | 71.9                  | 63.6            | 69.4                  |

- Safety/Alignment Performance
  - Truthful QA Benchmark: to measure whether the LLM produces the truthful answer for the question
  - Toxigen Benchmark: what percentage of the LLM response can be harmful and toxic

| Llama 2  | Truthful QA | Toxigen |
| -------- | ----------- | ------- |
| 7B       | 33.3        | 21.3    |
| 13B      | 41.9        | 26.1    |
| 70B      | 50.2        | 24.6    |
| 7B Chat  | 57.0        | 0       |
| 13B Chat | 62.2        | 0       |
| 70B Chat | 64.1        | 0       |

- Note: the chat-models contain no toxic information

## How to access Llama models

- There are couple of methods to access the Llama models
  - Hosting API service: Amazon Bedrock, AnyScale, Google Cloud, Microsoft Azure, Replicate, Together.ai, etc
  - Self-configured Cloud: such as self-own hosting in Amazon Sagemaker
  - Local PC

<p align="center"><img width=300 src="../../assets/img/llama2-hosting.png"></p>

### Code Llama

- **Code Llama** is a family of large language models for code based on Llama 2 providing state-of-the-art performance among open models, infilling capabilities, support for large input contexts, and zero-shot instruction following ability for programming tasks.
- Code Llama cover a wide range of applications:
  - Foundation model (Code Llama)
  - Python specialization model (Code Llama - Python)
  - Instruction-following model (Code Llama - Instruct) which is fine-tuned for understanding natural language instructions.

<p align="center"><img width=400 src="../../assets/img/code-llama-family.png"></p>
