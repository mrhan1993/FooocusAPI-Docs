# InpaintOutpaint
## Generate mask

**New future from v2.5.0**

> Note: When you use generate mask, it is a separate step. It will not be automatically executed during the inpaint process.
> This is because generate mask is not part of image generation; it is a separate function, which is why it has a separate endpoint.

It corresponds to the functionality in the lower part of the WebUI, as shown in the image below:

![generate_mask_webui_01.png](generate_mask_webui_01.png)

Taking the most basic case in the image as an example:

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

The return value is a `base64` encoded string, without the `data:image/png;base64,` prefix. Like the `Generate` interface, `image` can be a `base64` encoded
string or a URL of an image.

In the example above, all the parameters that the `Generate mask` interface can accept are listed. In practice, you only need to pass the ones you see on the interface,
with `image` being the only required field.

Most of these parameters correspond to the `sam` model and have default values, so you don't need to pass all of them. Only pass the ones you need.

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

This example shows the above image and the generated `mask`. However, please note that the redrawing area will strictly adhere to the parts in the `mask`.
For instance, with the code above, the result is:

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
