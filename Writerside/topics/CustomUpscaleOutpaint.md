# CustomUpscaleOutpaint

## Upscale

When `uov_method` is 'Upscale (Custom)', you can specify the `upscale_multiple` parameter to control the magnification of the image,
default is 1, max is 5.

```python
import requests

params = {
    'uov_input_image': 'https://avatars.githubusercontent.com/u/50648276',
    'uov_method': 'Upscale (Custom)',
    'upscale_multiple': 2.5
}

response = requests.post('http://127.0.0.1/v1/engine/generate', json=params)
```

here is the result

![2024-07-31_14-29-57_9351.png](2024-07-31_14-29-57_9351.png)

## Outpaint

```python
import requests

params = {
    "inpaint_input_image": "https://raw.githubusercontent.com/mrhan1993/Fooocus-API/main/examples/imgs/target_face.png",
    "outpaint_selections": ["Left", "Right"], # The expansion direction is determined by this parameter
    "outpaint_distance": [120, 400, 0, 200] # [left, right, top, bottom], if direction not in `outpaint_selections`, num only used as a placeholder
}

response = requests.post('http://127.0.0.1/v1/engine/generate', json=params)
```

here is the result, I think is easy to understand

![2024-07-31_14-39-27_1195.png](2024-07-31_14-39-27_1195.png)