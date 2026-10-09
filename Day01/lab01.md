### For the first few days, you don't need a GPU. Your Ubuntu system is sufficient.

#### Create our working environment:
```bash
mkdir -p ~/fde-llmops
cd ~/fde-llmops

python3 -m venv .venv
source .venv/bin/activate

python --version
pip install --upgrade pip
````

#### Create the first module:

```bash
mkdir 01-llm-fundamentals
cd 01-llm-fundamentals
````
#### Install:

```bash
pip install transformers
````
### creatre file

```bash
vi tokenizer_lab.py
````

```bash
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
````

![llm15](/img/output01.png)
