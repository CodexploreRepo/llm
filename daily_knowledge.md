# Day 1

- **API rate limit**: Most API services have rate limits, so if the code was not designed to wait in-between API calls, you may not receive embeddings for all batches of text.
  - This particular service can handle 20 calls per minute. In calls per second, that's 20 calls divided by 60 seconds, or `20/60`

## LLM Hyper-parameters

### Temperature

- Decoding strategies is to determine the next words in LLM
  - Greedy decoding: select the one with highest probability, in this example is flower (0.5)
  - Random sample: use the probabilies to sample a random token, say bugs (0.03)
  <p align="center"><img src="./assets/img/temperature-example.png" ></p>
- `temperature` [0 to 1.0] controls the randomness

  - **Low temp:** used for the deterministic or less open-end responses say classification or extraction tasks
  - **High temp:** used for more randomness, namely open-end tasks like brainstorming or summarisation
  <p align="center"><img src="./assets/img/temperature-values.png" ></p>

- Formula for temperature
  <p align="center"><img src="./assets/img/temperature-formula.png" ></p>
