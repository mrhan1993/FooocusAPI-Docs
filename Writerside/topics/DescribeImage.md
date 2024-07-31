# DescribeImage

You can get prompt from an image by using DescribeImage.

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

you can get the following response, the result may be different when you request multiple times:

```json
{
    "describe": "a girl in a brown coat and hat is holding a piece of food"
}
```
