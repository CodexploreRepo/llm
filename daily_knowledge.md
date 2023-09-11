# Day 1

- **API rate limit**: Most API services have rate limits, so if the code was not designed to wait in-between API calls, you may not receive embeddings for all batches of text.
  - This particular service can handle 20 calls per minute. In calls per second, that's 20 calls divided by 60 seconds, or `20/60`

## LLM Models

### Bison

- `text-bison@001` use for instant response instead of multi-turn dialouge
- `chat-bison@001`. (chat-bison model, version 1) use for multi-turn dialogue

## Adjusting Creativity/Randomness

- You can control the behavior of the language model's *decoding strategy* by adjusting the **temperature**, **top-k**, and **top-n** parameters.
- *Decoding strategy* is to determine the next words in LLM
  - Greedy decoding: select the one with highest probability, in this example is flower (0.5)
  - Random sample: use the probabilies to sample a random token, say bugs (0.03)
  <p align="center"><img src="./assets/img/temperature-example.png" ></p>
- The decoding strategy applies `top_k`, then `top_p`, then `temperature` (in that order).
  - Step 1: Tokens will be filter by `top_k`
  - Step 2: Further filter by `top_p`
  - Step 3: Output will be selected based on the temperature
  <p align="center"><img src="./assets/img/decoding-strategies.png" ></p>

### Temperature

- `temperature` [0 to 1.0] controls the randomness

  - **Low temp:** used for the deterministic or less open-end responses say classification or extraction tasks
  - **High temp:** used for more randomness, namely open-end tasks like brainstorming or summarisation
  <p align="center"><img src="./assets/img/temperature-values.png" ></p>

- Formula for temperature
  <p align="center"><img src="./assets/img/temperature-formula.png" ></p>

### Top K

- Sample from tokens with the `top_k` probabilities
- The default value for `top_k` is `40`.
- You can set `top_k` to values between `1` and `40`.

### Top P

- Top p: sample the minimum set of tokens whose probabilities add up to probability `p` or greater.
- The default value for `top_p` is `0.95`.
- If you want to adjust `top_p` and `top_k` and see different results, remember to set `temperature` to be greater than zero, otherwise the model will always choose the token with the highest probability.
  <p align="center"><img src="./assets/img/top-p.png" ></p>
