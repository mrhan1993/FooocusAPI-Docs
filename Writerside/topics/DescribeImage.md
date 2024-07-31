# DescribeImage

通过图像反推提示词

```python
import requests

url = "http://127.0.0.1:7866/v1/tools/describe-image"

params = {
    "image": "http://localhost:7866/outputs/2024-07-28/2024-07-28_23-44-02_9377.png",
    "image_type": "Photo"
}

response = requests.post(url, json=params)

print(response.text)
```

下面是一个示例结果，同一张图片多次执行会得到不同的结果:

```json
{
    "describe": "a girl in a brown coat and hat is holding a piece of food"
}
```
