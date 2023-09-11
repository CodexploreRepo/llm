# Large Language Model (LLM) Introduction

## Type of LLMs
There is two types of LLMs
- `Base LLM` trained on a huge data & predict the next words based on text training data
- `Instruction Tuned LLM` tried to follow instructions
  - based on Base LLM and fine-tune it based on a set of instructions with input/output &#8594; further tuned by Reinforcement Learning with Human Feedback (RLHF)
- Example of Base LLM vs Instruction Tuned LLM:
  - Base LLM: when prompting **What is the captial of France?**, maybe based on the training data, the most likely next words will be *What is France's largest city*
![Screenshot 2023-05-26 at 23 26 06](https://github.com/CodexploreRepo/prompt-engineering/assets/64508435/c6d1df59-fc06-4772-b6fd-aa9900d47d13)
