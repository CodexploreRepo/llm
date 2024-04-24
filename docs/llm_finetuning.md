# LLM Fine-tuning

## What is fine-tuning ?

- Fine-tuning is taking a pre-trained model and training at least one internal model parameter (i.e. weights).

## Fine-tuning Methods

- Method 1: Self-supervised
  - Self-supervised learning consists of training a model based on the inherent structure of the training data. In the context of LLMs, what this typically looks like is given a sequence of words (or tokens, to be more precise), predict the next word (token).
- Method 2: Supervised
  - This involves training a model on **input-output** pairs for a particular task.
- Method 3: Reinforcement Learning (RL)
  - Supervised Fine-tuning:
    - Choose fine-tuning task: (e.g. summarization, question answering, text classification)
    - Prepare training dataset
    - Choose a base model
    - Fine-tune model via supervised learning:
      - Option 1: Retrain all parameters
      - Option 2: Transfer learning - freeze a large portion of the model parameters and only fine-tuning the last few layers of the model
      - Option 3: Parameter Efficient Fine-tuning (PEFT) - freeze all of the weigths & augment a base model with a relatively small number of trainable parameters.
        - PEFT encapsulates a family of techniques, one of which is the popular LoRA (Low-Rank Adaptation) method
    - Evaluate model performance
  - Train Reward Model: start with a prompt and send to "Supervised Fine-tuning Model", and get multiple responses and let human rank it from best to worst to train the Reward model
  - RL with PPO: taking the prompt and send to "Supervised Fine-tuning Model", and the generated response will be sent to the Reward model. The reward model will send the feedback to the "Supervised Fine-tuning Model" to fine-tune even further

## Fine-tuning with Supervised Learning

### Parameter Efficient Fine-tuning (PEFT)

- Parameter Efficient Fine-tuning (PEFT) - freeze all of the weigths & augment a base model with a relatively small number of trainable parameters.
  - PEFT encapsulates a family of techniques, one of which is the popular LoRA (Low-Rank Adaptation) method

#### LORA

- [Notebook](.././notebooks/lora-finetuning.ipynb)
- The basic idea behind LoRA is to pick a subset of layers in an existing model and modify their weights as follows:
- **Original hidden layer** of a model: $h(x) = W_0*x$, where
  - $h()$ = the hidden layer that will be tuned
  - $W_0$ = the original weight matrix for the h with dimension $(d, k)$
    - For example, $d=1000$, $k=1000$, so $W_0$ will have 1M trainable parameters
  - $x$ = the input to the hidden layer
- **LORA** will add $\Delta W$ (a matrix of trainable parameters) into the current hidden layer:
  $$ H(x) = W_0*x + \Delta W * x$$

- $\Delta W$ is decomposed according to $\Delta W=BA$ , where
  - $\Delta W$ is a $(d, k)$ matrix,
  - $B$ is $(d, r)$
  - $A$ is $(r, k)$
  - $r$ is the assumed “intrinsic rank” of ΔW, which can be as small as 1 or 2
- **Key Point** $r$: if $d=1000$, $r=2$, and $k=1000$, so the number of trainable parameters drops from $1M$ to $1000*2 + 2*1000 = 4000$ in that layer
