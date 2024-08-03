# MixImagePrompt

To be honest, you need to study Fooocus to understand how it works and what effects it has. From the perspective of operation and actual process, 
it involves mixing multiple operations, such as the example below:

```python
import requests

url = "http://127.0.0.1:7866/v1/engine/generate"

params = {
    "uov_input_image": "http://localhost:7866/outputs/2024-07-26/2024-07-26_16-13-33_3951.png",
    "uov_method": "Vary (Strong)",
    "mixing_image_prompt_and_vary_upscale": True,
    "controlnet_image": [
        {
            "cn_img": "http://localhost:7866/outputs/2024-07-27/2024-07-26_16-13-33_3951.png",
            "cn_stop": 0.5,
            "cn_weight": 0.6,
            "cn_type": "ImagePrompt"
    }]
}

response = requests.post(url, json=params)

print(response.json())
```

Its corresponding WebUI looks like this:

![image_prompt_vary_01.png](image_prompt_vary_01.png)
![image_prompt_vary_02.png](image_prompt_vary_02.png)

Use with Inpaint:

```python
import requests

url = "http://127.0.0.1:7866/v1/engine/generate"

params = {
    "inpaint_input_image": "http://localhost:7866/outputs/2024-07-27/2024-07-26_16-13-33_3951.png",
    "inpaint_mask_image_upload": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAABIAAAAOACAA...",
    "mixing_image_prompt_and_inpaint": True,
    "controlnet_image": [
        {
            "cn_img": "http://localhost:7866/outputs/2024-07-27/2024-07-26_16-13-33_3951.png",
            "cn_stop": 0.5,
            "cn_weight": 0.6,
            "cn_type": "ImagePrompt"
    }]
}

response = requests.post(url, json=params)
```

All together:

```python
import requests

url = "http://127.0.0.1:7866/v1/engine/generate"

params = {
    "inpaint_input_image": "http://localhost:7866/outputs/2024-07-27/2024-07-26_16-13-33_3951.png",
    "inpaint_mask_image_upload": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAABIAAAAOACAA...",
    "uov_input_image": "http://localhost:7866/outputs/2024-07-26/2024-07-26_16-13-33_3951.png",
    "uov_method": "Vary (Strong)",
    "mixing_image_prompt_and_vary_upscale": True,
    "mixing_image_prompt_and_inpaint": True,
    "controlnet_image": [
        {
            "cn_img": "http://localhost:7866/outputs/2024-07-27/2024-07-26_16-13-33_3951.png",
            "cn_stop": 0.5,
            "cn_weight": 0.6,
            "cn_type": "ImagePrompt"
    }]
}

response = requests.post(url, json=params)
```
