# WebHook

启动一个类似下面的服务器:

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

然后，在任务参数中增加 `webhook_url`

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

当任务完成后，服务器会收到一个 POST 请求，其中包含任务的结果。
