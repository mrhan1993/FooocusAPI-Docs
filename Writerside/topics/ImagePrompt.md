# ImagePrompt

首先，我们看下在 WebUI 中如何使用 ImagePrompt

![image_prompt_01.png](image_prompt_01.png)

这里有四个完全相同的区域，包含图片、停止位置、权重以及所要进行的操作。因此这里会是一个列表，同时，在程序中它的名字叫做 ControlNet，
在参数上应该就是下面的样子，请注意，你可以传入更多的元素，但会被截断，只取前四个。当然，你也不是必须传四个，几个都行。

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

然后，使用 `requests` 进行请求就好了：

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

这是生成的结果：

![2024-07-28_23-44-02_9377.png](2024-07-28_23-44-02_9377.png)
