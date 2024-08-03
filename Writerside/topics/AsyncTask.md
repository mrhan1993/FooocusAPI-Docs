# AsyncTask

This section will demonstrate how to obtain different types of Response through various combinations of parameters.
The Generate interface provides the following types of Response:

- `ImageResponse`: Image bytes
- `JsonResponse`: Json string
- `StreamResponse`：Stream output

Among them, `ImageResponse` and `StreamResponse` are synchronous, while `JsonResponse` can be either synchronous or asynchronous depending on the parameters.

The example will be demonstrated with `Text to Image`.

## Synchronous Request
### JsonResponse

This is the return type with default parameters, which is also the type used in the previous examples.

```python
import requests

url = "http://localhost:7866/v1/engine/generate"

params = {
    "prompt": "a dog playing with a ball",
    "negative_prompt": ""
}

response = requests.post(url, json=params)
```

Upon completion, you can obtain the list of URLs for the generated images in the `result` field of the returned JSON.

### ImageResponse

Continuing with the code above, if you include `headers` in the request and set the `accept` parameter to `image/xxx`, 
you will receive an `ImageResponse`. The supported formats include `image/png`, `image/jpg`, `image/jpeg`, and `image/webp`. 
Nevertheless, you can write any format after `/`, but unsupported formats will be set to `image/png`.

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

Upon completion, you can obtain the binary data of the generated image in `response.content`, just as if you were downloading an image directly from the web.
If you print it out directly, it will look something like this:

```text
b'RIFF\xba-\x01\x00WEBPVP8 \xae-\x01\x00\x10\xe1\x07\x9d\x01*\x80\x04\x80\x03>m4\x95H$"\xa7)\xa2\xf3\xeb\xa10\r\x89gn-\x8d\xb6\xa3\x85\xff)\xa7S....
```

> Note: This method will force the `image_number` to be set to 1, meaning only one image will be returned.

### StreamResponse

If you have used LLMs like OpenAI, you should be familiar with streaming output, which returns a stream that you can access using `response.iter_lines()`.
Continuing with the code above, if you specify the `stream_output` parameter as `true` in the request, you will receive a `StreamResponse`:

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

You will get this:

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

By slightly modifying the code to remove unnecessary characters, you can obtain a series of JSON strings:

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

The output:
```text
{'progress': 13, 'preview': None, 'message': 'Preparing task 1/1 ...', 'images': []}
{'progress': 13, 'preview': 'data:image/png;base64,iVBORw0KGgoAAAANSU...', 'message': 'Sampling step 1/4, image 1/1 ...', 'images': []}
{'progress': 34, 'preview': 'data:image/png;base64,iVBORw0KGgoAAAANSU...', 'message': 'Sampling step 2/4, image 1/1 ...', 'images': []}
{'progress': 56, 'preview': 'data:image/png;base64,iVBORw0KGgoAAAANSU...', 'message': 'Sampling step 3/4, image 1/1 ...', 'images': []}
{'progress': 78, 'preview': 'data:image/png;base64,iVBORw0KGgoAAAANSU...', 'message': 'Sampling step 4/4, image 1/1 ...', 'images': []}
{'progress': 100, 'preview': None, 'message': 'Saving image 1/1 to system ...', 'images': []}
{'progress': 100, 'preview': None, 'message': 'Finished', 'images': ["http://127.0.0.1:7866/outputs/2024-07-26/2024-07-26_16-29-10_1354.png"]}
```

> Note: In the current version, the `preview` field always returns images in PNG format, regardless of whether `output_format` is set in the parameters.
> However, this does not affect the final generated result, which is the content in the `images` field.

## Async request

Asynchronous requests only have one type, `JsonResponse`. You simply need to specify the `async_process` parameter as `true` in the request. 
Its return format is consistent with `JsonResponse`, but it will return immediately with some fields being empty.

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

Output below：

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

> Note: Asynchronous requests do not return the specific content of `req_params`, and most other fields are just placeholders. The only useful field might be `task_id`.

## Priority

When the program processes these parameters, it checks `accept`, `stream_output`, and `async_process` in sequence. Once a condition is met, it does not proceed with subsequent checks.
This means that the priority order, from highest to lowest, is: `accept`, `stream_output`, `async_process`. This also means that if `accept`, `stream_output`, and `async_process`
are all specified at the same time, `stream_output` and `async_process` will be ignored.
