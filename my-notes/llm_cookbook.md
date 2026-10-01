# LLM Cookbook (Swapnil)

## Setup: load key and client
```python
from dotenv import load_dotenv
from openai import OpenAI
load_dotenv(override=True)   # load .env BEFORE creating the client
client = OpenAI()
```

## Basic call
```python
messages = [
    {"role": "system", "content": SYSTEM_PROMPT},
    {"role": "user", "content": USER_PROMPT},
]
r = client.chat.completions.create(model="gpt-4.1-mini", messages=messages, temperature=0)
print(r.choices[0].message.content)
print(r.usage)   # token counts
```

## Raw HTTP version (what the library does underneath)
```python
import requests
headers = {"Authorization": f"Bearer {api_key}", "Content-Type": "application/json"}
payload = {"model": "gpt-5-nano", "messages": [{"role": "user", "content": "Hi"}]}
resp = requests.post("https://api.openai.com/v1/chat/completions", headers=headers, json=payload, timeout=30)
resp.raise_for_status()
print(resp.json()["choices"][0]["message"]["content"])
```

## Prompt template (fixed instructions + variable data)
```python
def messages_for(data):
    return [
        {"role": "system", "content": system_prompt},
        {"role": "user", "content": user_prompt_prefix + data},
    ]
```

## Render markdown in a notebook
```python
from IPython.display import Markdown, display
display(Markdown(text))
```

## Gotchas
- Missing comma between arguments -> SyntaxError reported on the NEXT line
- getenv returns None if missing -> check `if not api_key` first
- Edited a cell? Rerun it. Weird results? Restart + Run All
- requests.get(..., timeout=10) always
- JS-rendered sites need Playwright, not requests
- Never print the API key or the headers dict
