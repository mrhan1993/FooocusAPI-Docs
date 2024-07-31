# Preset

Preset 是一系列预设参数的集合，用于快速生成一个特定风格或参数的图像。这些预设的配置文件位于 `preset` 目录下，每个文件对应一个预设。

> 这些预设通常会使用不同的模型，如果是初次使用，可能会花费较长时间用来下载模型。

使用它们很简单，只需要在参数传递时指定 `preset` 参数即可。例如：

```python
import requests

url = "http://127.0.0.1:7866/v1/engine/generate"

params = {
    "preset": "anime",
    "prompt": "a dog playing with a ball"
}

response = requests.post(url, json=params)
```

![2024-07-29_13-34-59_6906.png](2024-07-29_13-34-59_6906.png)

比如我们换一种，使用 `sai` 预设：

```python
import requests

url = "http://127.0.0.1:7866/v1/engine/generate"

params = {
    "preset": "sai",
    "prompt": "a dog playing with a ball"
}

response = requests.post(url, json=params)
```

![2024-07-29_13-37-12_6520.png](2024-07-29_13-37-12_6520.png)
