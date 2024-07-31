# UpscaleVary

The only thing to be mindful of is not to mistype the value for uov_method:

```python
import requests

url = "http://127.0.0.1/v1/engine/generate/"

params = {
    "uov_input_image": "http://localhost:7866/outputs/2024-07-26/2024-07-26_16-13-33_3951.png",
    "uov_method": "Vary (Strong)"
}

response = requests.post(url, json=params)
```

Completely consistent with the WebUI, you just need to pass a different `uov_method`. For example, to enlarge this image:

```python
import requests

url = "http://127.0.0.1/v1/engine/generate/"

params = {
    "uov_input_image": "http://localhost:7866/outputs/2024-07-26/2024-07-26_16-13-33_3951.png",
    "uov_method": "Upscale (Fast 2x)"
}

response = requests.post(url, json=params)
```
