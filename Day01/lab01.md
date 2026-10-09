### For the first few days, you don't need a GPU. Your Ubuntu system is sufficient.

#### Create our working environment:
`` sh
mkdir -p ~/fde-llmops
cd ~/fde-llmops

python3 -m venv .venv
source .venv/bin/activate

python --version
pip install --upgrade pip
``

#### Create the first module:

`` sh
mkdir 01-llm-fundamentals
cd 01-llm-fundamentals
``
#### Install:

`` sh
pip install transformers
``
### creatre file

`` sh
vi tokenizer_lab.py
``

`` sh
from transformers import AutoTokenizer

model = "bert-base-uncased"

tokenizer = AutoTokenizer.from_pretrained(model)

text = "Kubernetes is a container orchestration platform."

tokens = tokenizer.tokenize(text)
token_ids = tokenizer.encode(text)

print("Original:")
print(text)

print("\nTokens:")
print(tokens)

print("\nToken IDs:")
print(token_ids)

print("\nNumber of tokens:")
print(len(token_ids))

print("\nDecoded:")
print(tokenizer.decode(token_ids))
``
