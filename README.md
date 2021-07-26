![logo](banner.png)

## Overview
Sukima is a ready-to-deploy container that implements a REST API for Language Models.

### Curent API Functions
- **models** : Fetch a list of ready-to-use Language Models for inference.
- **load** : Allocate a Language Model.
- **delete** : Free a Language Model from memory.
- **generate** : Use a Language Model to generate tokens.

### Setup
1. Customize the ports and host in the [Dockerfile](Dockerfile) to your liking.
2. Install Docker and run ``docker-compose up`` in the directory with the Dockerfile to deploy.
3. That's it!

### Todo
- HTTPS Support
- Rate Limiting
- Support for models other than GPT-Neo.
- Support for other Language Modeling tasks such as Sentiment Analysis and Named Entity Recognition.

### Example API Usage
```python
import requests

# List currently available models
r = requests.get("http://localhost:8080/v1/models")
for i in r.json()["models"]:
  print(i)


# Load the model! In order to do this, we use a simple request body.
request_body_model_init = {
    "model": "gpt-neo-125M"
}

res = requests.post('http://localhost:8080/v1/load', json=request_body_model_init)
print(res.json())


# Generate from the model!
# For generation, we must fill every generation parameter in this request body.
request_body = {
    "model": "gpt-neo-125M",
    "prompt": "Sukima is a ready-to-deploy container that serves a REST API for Language Models. Not only does",
    "generate_num": 32,
    "temperature": 0.1,
    "top_p": 1.0,
    "repetition_penalty": 3.0
}

res = requests.post('http://localhost:8080/v1/generate', json=request_body)
print(res.json()['completion']['text'], '\n')


# And finally, delete the model that we have allocated.
res = requests.post('http://localhost:8080/v1/delete', json=request_body_model_init)
print(res.json())
```

### License
[Simplified BSD License](LICENSE)
