# Preset

Preset is a collection of predefined parameters used for quickly generating an image with a specific style or set of parameters.
These predefined configuration files are located in the `preset` directory, with each file corresponding to a preset.

> These presets typically use different models, and if they are used for the first time, it may take a considerable amount of time to download the models.

Using them is simple, you just need to specify the `preset` parameter when passing the arguments. For example:

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

For instance, let's switch to using the `sai` preset:

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
