# WebHook

You need running a server like this:

```python
from fastapi import FastAPI, Request
import uvicorn

app = FastAPI()

@app.post("/post_data/")
async def receive_post_data(request: Request):
    try:
        data = await request.json()
        print("Received data:", data)
    except Exception as e:
        return {"error": str(e)}

uvicorn.run(app, host="0.0.0.0", port=8000)
```

and then, add `webhook_url` to your parameters

```python
import requests

params = {
    "prompt": "a girl",
    "performance_selection": "Lightning",
    "webhook_url": "http://127.0.0.1:8000/post_data"
}
response = requests.post("http://127.0.0.1:7866/v1/engine/generate", json=params)

print(response.json())
```

and you will get the result in your server. the data is serialization by TaskOBJ, like the return of `generate` API.
