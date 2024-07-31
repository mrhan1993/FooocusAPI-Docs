# UpscaleVary

唯一需要注意的是，不要把 `uov_method` 的值写错了：

```python
import requests

url = "http://127.0.0.1/v1/engine/generate/"

params = {
    "uov_input_image": "http://localhost:7866/outputs/2024-07-26/2024-07-26_16-13-33_3951.png",
    "uov_method": "Vary (Strong)"
}

response = requests.post(url, json=params)
```

和 WebUI 上完全一致，你只需要传递不同的 `uov_method` 就好了，比如放大这个图片：

```python
import requests

url = "http://127.0.0.1/v1/engine/generate/"

params = {
    "uov_input_image": "http://localhost:7866/outputs/2024-07-26/2024-07-26_16-13-33_3951.png",
    "uov_method": "Upscale (Fast 2x)"
}

response = requests.post(url, json=params)
```
