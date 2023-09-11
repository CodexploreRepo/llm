# Vertex AI

## Text Embeddings

### What is Text Embedding

#### How are sentence embeddings computed ?

- Simple Method: embedded each word separately &#8594; take sum or mean of all word embeddings
  - Drawback: lose the word position/context, say 2 sentneces have the same words, but those words which are placed differently will make the sentence's meaning different from each other. However, the sentence embedding vector will be the same as we will take the sum or mean of those words only.
- Modern Method:
  - Use transformer neural network to compute a context-aware representation of each word, then take average the context-aware representations
  - Compute embeddings for each token (e.g. sub-word) rather than words, which enables the algorithm to work even with novel or mispelt word

#### Text Embedding Application

Refer [notebook](../notebooks/vertex-ai-applications-of-embeddings.ipynb)

- Classification
- Clustering
- Outlier Detection
- Semantic Search
- Recommendation

### Text Embeddings with Vertex AI

- If you were running this notebook locally, you would first install Vertex AI.

```Python
!pip install google-cloud-aiplatform
```

- Setup Google Cloud

```Python
# Import and initialize the Vertex AI Python SDK

import vertexai

REGION = 'us-central1'

vertexai.init(project = PROJECT_ID,
              location = REGION,
              credentials = credentials)
```

- Load the embedding model and get word embeddings

```Python
from vertexai.language_models import TextEmbeddingModel

embedding_model = TextEmbeddingModel.from_pretrained(
    "textembedding-gecko@001")

# Generate a word embedding
embedding = embedding_model.get_embeddings(
    ["life"])

vector = embedding[0].values
print(f"Length = {len(vector)}")
print(vector[:10])
"""
Length = 768
[-0.006005102302879095, 0.015532972291111946, -0.030447669327259064, 0.05322219058871269, 0.014444807544350624, -0.0542873740196228, 0.045140113681554794, 0.02127358317375183, -0.06537645310163498, 0.019103270024061203]
"""
```

- Generate a sentence embedding.

```Python
embedding = embedding_model.get_embeddings(
    ["What is the meaning of life?"])
vector = embedding[0].values
print(f"Length = {len(vector)}") #Length = 768
```

## Large Language Model

```Python
import vertexai
vertexai.init(project=PROJECT_ID,
              location=REGION,
              credentials = credentials)

from vertexai.language_models import TextGenerationModel

# text-bison@001 use for instant response instead of multi-turn dialouge
generation_model = TextGenerationModel.from_pretrained(
    "text-bison@001")
```
