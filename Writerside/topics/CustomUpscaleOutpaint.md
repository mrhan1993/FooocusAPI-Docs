# CustomUpscaleOutpaint

## Upscale

当 `uov_method` 是 'Upscale (Custom)' 时，指定 `upscale_multiple` 可以自定义图像放大倍数，默认是 1，最大是 5。

```python
import requests

params = {
    'uov_input_image': 'https://avatars.githubusercontent.com/u/50648276',
    'uov_method': 'Upscale (Custom)',
    'upscale_multiple': 2.5
}

response = requests.post('http://127.0.0.1/v1/engine/generate', json=params)
```

结果如下：

![2024-07-31_14-29-57_9351.png](2024-07-31_14-29-57_9351.png)

## Outpaint

```python
import requests

params = {
    "inpaint_input_image": "https://raw.githubusercontent.com/mrhan1993/Fooocus-API/main/examples/imgs/target_face.png",
    "outpaint_selections": ["Left", "Right"], # 扩图方向取决于这个参数
    "outpaint_distance": [120, 400, 0, 200] # 四个数字对应 [left, right, top, bottom]，如果 `outpaint_selections` 中没有指定对应方向，这里的数值不起作用
}

response = requests.post('http://127.0.0.1/v1/engine/generate', json=params)
```

结果如下，应该还是很容易理解的：

![2024-07-31_14-39-27_1195.png](2024-07-31_14-39-27_1195.png)