# InpaintOutpaint
## Generate mask

**New future from v2.5.0**

> Note：当你使用 generate mask 的时候，这是一个单独的步骤。它不会在 inpaint 的过程中自动执行。这是因为 
> generate mask 不是图像生成的一部分，它是一个单独的函数，这也是为什么它有单独的 endpoint。

它对应的是 WebUI 中下图部分的功能：

![generate_mask_webui_01.png](generate_mask_webui_01.png)

以图中最基础的情况为例：

```python
import requests

url = "http://localhost:7866/v1/tools/generate_mask"

params = {
  "image": "http://localhost:7866/outputs/2024-07-27/2024-07-26_16-13-33_3951.png",
  "mask_model": "isnet-general-use",
  "cloth_category": "full",
  "dino_prompt_text": "",
  "sam_model": "vit_b",
  "box_threshold": 0.3,
  "text_threshold": 0.25,
  "sam_max_detections": 0,
  "dino_erode_or_dilate": 0,
  "dino_debug": False
}

response = requests.post(url, json=params)
```

返回值是一个 `base64` 编码的字符串，不包含 `data:image/png;base64,` 前缀。和 `Generate` 接口一样，`image` 可以是 `base64`
编码的字符串，也可以是图片的 URL。

上面的示例中，是 `Generate mask` 接口可以接受的所有参数，实际上，你只需要传递在界面上看到的即可，只有 `image` 是必填的。
它们中的大多数的都是对应 `sam` 这个模型的，而且都有默认值，所以你不需要全部都传递，只需要传递你需要的即可。

## Inpaint

Pass an image and a mask, with or without prompt to the generate endpoint to get a new image.

```python
import requests

url = "http://localhost:7866/v1/engine/generate"

params = {
    "prompt": "",
    "inpaint_additional_prompt": "a red ball",
    "inpaint_input_image": "http://localhost:7866/outputs/2024-07-27/2024-07-26_16-13-33_3951.png",
    "inpaint_mask_image_upload": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAABIAAAAOACAA..."
}

response = requests.post(url, json=params)
```

这个示例就是上面的图片和生成的 `mask`，不过请注意，重绘区域将严格遵循 `mask` 中的部分。比如上面的代码，其结果是：

![2024-07-28_20-10-08_8815.png](2024-07-28_20-10-08_8815.png)

## Outpaint

```python
import requests

url = "http://localhost:7866/v1/engine/generate"

params = {
    "inpaint_input_image": "http://localhost:7866/outputs/2024-07-27/2024-07-26_16-13-33_3951.png",
    "outpaint_selections": ["Left", "Bottom"]
}

response = requests.post(url, json=params)
```

you will get this：

![2024-07-28_20-18-25_1371.png](2024-07-28_20-18-25_1371.png)

> the outpaint distance is 1/3 of image length or height
