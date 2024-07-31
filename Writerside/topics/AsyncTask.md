# AsyncTask

本节将演示如何通过不同的参数组合获取不同类型的 Response，Generate 接口提供了以下几种 Response 类型：

- `ImageResponse`：直接返回图片
- `JsonResponse`：返回 JSON 格式的文本
- `StreamResponse`：流式输出

其中，`ImageResponse` 和 `StreamResponse` 都是同步的，`JsonResponse` 根据参数的不同，可能是同步的，也可能是异步的。

示例将以 `Text to Image` 进行演示。

## 同步请求
### JsonResponse

这是默认参数时的返回类型，这也是之前示例所使用的类型。

```python
import requests

url = "http://localhost:7866/v1/engine/generate"

params = {
    "prompt": "a dog playing with a ball",
    "negative_prompt": ""
}

response = requests.post(url, json=params)
```

完成之后你可以在返回的 JSON 中的 `result` 字段中获取生成的图片的 URL 列表。

### ImageResponse

继续使用上面的代码，在请求中携带 `headers` 并将 `accept` 参数设置为 `image/xxx`，就可以得到一个 `ImageResponse`。支持的
格式有 `image/png`、`image/jpg`、`image/jpeg`、`image/webp`。尽管如此，你可以在 `/` 后面写上任何格式，但不受支持的格式会被设置为 `image/png`。

```python
import requests

url = "http://localhost:7866/v1/engine/generate"

headers = {
    "accept": "image/webp"
}

params = {
    "prompt": "a dog playing with a ball",
    "negative_prompt": ""
}

response = requests.post(url, headers=headers, json=params)
```

完成之后你可以在 `response.content` 中获取生成的图片的二进制数据，就像直接从 Web 上下载图片一样。如果将其直接打印出来，会是类似下面的样子：

```text
b'RIFF\xba-\x01\x00WEBPVP8 \xae-\x01\x00\x10\xe1\x07\x9d\x01*\x80\x04\x80\x03>m4\x95H$"\xa7)\xa2\xf3\xeb\xa10\r\x89gn-\x8d\xb6\xa3\x85\xff)\xa7S....
```

> 注意：这种方式会强制将 `image_number` 设置为 1，即只返回一张图片。

### StreamResponse

如果你使用过 OpenAI 之类的 LLM，应该对流式输出并不陌生，它会返回一个流，你可以使用 `response.iter_lines()` 来获取流中的数据。
继续使用上面的代码，在请求中指定 `stream_output` 参数为 `true`，就可以得到一个 `StreamResponse`:

```python
import requests

url = "http://localhost:7866/v1/engine/generate"

params = {
    "prompt": "a dog playing with a ball",
    "negative_prompt": "",
    "stream_output": "true"
}

response = requests.post(url, json=params, stream=True)

for line in response.iter_lines():
    if line:
        print(line.decode('utf-8'))
```

你会得到一个类似下面的输出：

```text
data: {"progress": 2, "preview": null, "message": "Loading models ...", "images": []}
data:
data: {"progress": 13, "preview": null, "message": "Preparing task 1/1 ...", "images": []}
data:
data: {"progress": 13, "preview": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAASAAAA...", 'message': 'Sampling step 1/4, image 1/1 ...', 'images': []}
data:
data: {"progress": 34, "preview": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAASAAAA...", 'message': 'Sampling step 2/4, image 1/1 ...', 'images': []}
data:
data: {"progress": 56, "preview": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAASAAAA...", 'message': 'Sampling step 3/4, image 1/1 ...', 'images': []}
data:
data: {"progress": 78, "preview": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAASAAAA...", 'message': 'Sampling step 4/4, image 1/1 ...', 'images': []}
data:
data: {"progress": 100, "preview": null, "message": "Saving image 1/1 to system ...", "images": []}
data:
data: {"progress": 100, "preview": null, "message": "Finished", "images": ["http://127.0.0.1:7866/outputs/2024-07-26/2024-07-26_16-29-10_1354.png"]}
data:
```

稍微修改下代码，去除掉无用的字符，可以获得一系列 Json 字符串：

```python
for line in res.iter_lines(chunk_size=8192):
    line = line.decode('utf-8').split('\n')[0]

    try:
        json_data = json.loads(line[6:])
        if json_data["preview"] is not None:
            json_data["preview"] = "data:image/png;base64,iVBORw0KGgoAAAANSU..."
    except json.decoder.JSONDecodeError:
        continue
    print(json_data)
```

输出如下：
```text
{'progress': 13, 'preview': None, 'message': 'Preparing task 1/1 ...', 'images': []}
{'progress': 13, 'preview': 'data:image/png;base64,iVBORw0KGgoAAAANSU...', 'message': 'Sampling step 1/4, image 1/1 ...', 'images': []}
{'progress': 34, 'preview': 'data:image/png;base64,iVBORw0KGgoAAAANSU...', 'message': 'Sampling step 2/4, image 1/1 ...', 'images': []}
{'progress': 56, 'preview': 'data:image/png;base64,iVBORw0KGgoAAAANSU...', 'message': 'Sampling step 3/4, image 1/1 ...', 'images': []}
{'progress': 78, 'preview': 'data:image/png;base64,iVBORw0KGgoAAAANSU...', 'message': 'Sampling step 4/4, image 1/1 ...', 'images': []}
{'progress': 100, 'preview': None, 'message': 'Saving image 1/1 to system ...', 'images': []}
{'progress': 100, 'preview': None, 'message': 'Finished', 'images': ["http://127.0.0.1:7866/outputs/2024-07-26/2024-07-26_16-29-10_1354.png"]}
```

> 注意：当前版本，它返回的 preview 字段总是 PNG 格式的图像，无论是否在参数中设置 `output_format`，不过这并不会影响最终的生成结果，也就是 `images` 字段中的内容。

## 异步请求

异步请求只有 `JsonResponse` 一种类型，只需要在请求中指定 `async_process` 参数为 `true` 即可。它的返回格式和 `JsonResponse` 一致，只是会立刻返回，只是部分字段为空。

```python
import requests

url = "http://localhost:7866/v1/engine/generate"

params = {
    "prompt": "a dog playing with a ball",
    "negative_prompt": "",
    "async_process": "true"
}

response = requests.post(url, json=params)

print(response.json())
```

输出如下：

```python
{
    'id': -1,
    'task_id': '0ff078640d894bc5a59242c05657a994',
    'req_params': {},
    'in_queue_mills': -1,
    'start_mills': -1,
    'finish_mills': -1,
    'task_status': 'pending',
    'progress': -1,
    'preview': '',
    'webhook_url': '',
    'result': []
}
```

> 注意：异步不会返回 `req_params`，多数字段仅作为占位符，有用的字段可能只有 `task_id`。

## 优先级

程序在处理这些参数的时候，会依次检查 `accept` `stream_output` `async_process`，一旦符合条件，则不在进行后续检查，这意味着优先级
从高到低依次为：`accept`、`stream_output`、`async_process`。这也就是会说如果同时指定了`accept`、`stream_output` 以及 `async_process`，则 `stream_output` 和 `async_process` 会被忽略。
