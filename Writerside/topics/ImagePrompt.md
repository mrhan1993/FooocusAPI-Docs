# ImagePrompt

First, let's take a look how to use ImagePrompt in WebUI:

![image_prompt_01.png](image_prompt_01.png)

There are four identical areas here, each containing an image, stop position, weight, and the operation to be performed. Therefore, this will be a list,
and in the program, it is called ControlNet. In the parameters, it should look like the following. Please note that you can pass more elements,
but only the first four will be taken. Of course, you are not required to pass four, any number can be used.

```python
"controlnet_image": [
    {
        "cn_img": "",
        "cn_stop": 0.5,
        "cn_weight": 0.6,
        "cn_type": "ImagePrompt"
    },{
        "cn_img": "",
        "cn_stop": 0.5,
        "cn_weight": 0.6,
        "cn_type": "ImagePrompt"
    },{
        "cn_img": "",
        "cn_stop": 0.5,
        "cn_weight": 0.6,
        "cn_type": "ImagePrompt"
    },{
        "cn_img": "",
        "cn_stop": 0.5,
        "cn_weight": 0.6,
        "cn_type": "ImagePrompt"
    }
]
```

Then, request use `requests`:

```python
import requests

url = "http://localhost/v1/engine/generate/"

params = {
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

Here is the result:

![2024-07-28_23-44-02_9377.png](2024-07-28_23-44-02_9377.png)
