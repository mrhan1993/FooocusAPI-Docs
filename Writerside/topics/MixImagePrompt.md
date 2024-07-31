# MixImagePrompt

这个东西说实话你得去研究 Fooocus，我也很难解释它具体是怎么工作的以及有什么效果。从操作以及实际过程来看，是混合多个操作，比如下面的示例：

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

它对应的 WebUI 是这样的：

![image_prompt_vary_01.png](image_prompt_vary_01.png)
![image_prompt_vary_02.png](image_prompt_vary_02.png)

也可以和 Inpaint 一起使用：

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

甚至可以来个大杂烩：

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
