# ImageEnhance

这是 2.5 新增的功能，它的参数更加复杂也更多。但和 ImagePrompt 一样，逻辑不变。还是以 WebUI 演示：

![enhance_01.png](enhance_01.png)

将 Enhance 勾选，切换到 Enhance 标签页，还有四个标签页，其中 `Upscale or Variation` 用于对图像预先或者之后进行处理，
`#1, #2, #3` 表示多次处理，从逻辑上看，它就是先生成一个 `mask` 然后根据描述词进行重绘。你可以将这里看做是 `Upscale or Variation`
，`Inpaint`，`Generate Mask` 进行了合并。

需要注意的是，传递多个 `enhance` 时，比如我们要对一个图像进行如下处理：Vary (Strong) -> Enhance face -> Enhance mouse -> Enhance eye
你会得到三张结果图像。使用 `save_final_enhanced_image_only` 参数可以只获取最终图像。

> `enhance_checkbox` must set to True

```python
import requests

url = "http://127.0.0.1:7866/v1/engine/generate"
params = {
    "enhance_input_image": "http://localhost:7866/outputs/2024-07-28/2024-07-28_23-44-02_9377.png",
    "enhance_checkbox": True,
    "enhance_uov_method": "Vary (Strong)",
    "enhance_uov_processing_order": "Before First Enhancement",
    "enhance_uov_prompt_type": "Original Prompts",
    "enhance_ctrls": [
        {
            "enhance_enabled": True,
            "enhance_mask_dino_prompt": "face",
            "enhance_prompt": "",
            "enhance_negative_prompt": "",
            "enhance_mask_model": "sam",
            "enhance_mask_cloth_category": "full",
            "enhance_mask_sam_model": "vit_b",
            "enhance_mask_text_threshold": 0.25,
            "enhance_mask_box_threshold": 0.3,
            "enhance_mask_sam_max_detections": 0,
            "enhance_inpaint_disable_initial_latent": False,
            "enhance_inpaint_engine": "v2.6",
            "enhance_inpaint_strength": 1.0,
            "enhance_inpaint_respective_field": 0.618,
            "enhance_inpaint_erode_or_dilate": 0.0,
            "enhance_mask_invert": False
        },
        {
            "enhance_enabled": True,
            "enhance_mask_dino_prompt": "mouse",
            "enhance_prompt": "",
            "enhance_negative_prompt": "",
            "enhance_mask_model": "sam",
            "enhance_mask_cloth_category": "full",
            "enhance_mask_sam_model": "vit_b",
            "enhance_mask_text_threshold": 0.25,
            "enhance_mask_box_threshold": 0.3,
            "enhance_mask_sam_max_detections": 0,
            "enhance_inpaint_disable_initial_latent": False,
            "enhance_inpaint_engine": "v2.6",
            "enhance_inpaint_strength": 1.0,
            "enhance_inpaint_respective_field": 0.618,
            "enhance_inpaint_erode_or_dilate": 0.0,
            "enhance_mask_invert": False
        },
        {
            "enhance_enabled": True,
            "enhance_mask_dino_prompt": "eye",
            "enhance_prompt": "",
            "enhance_negative_prompt": "",
            "enhance_mask_model": "sam",
            "enhance_mask_cloth_category": "full",
            "enhance_mask_sam_model": "vit_b",
            "enhance_mask_text_threshold": 0.25,
            "enhance_mask_box_threshold": 0.3,
            "enhance_mask_sam_max_detections": 0,
            "enhance_inpaint_disable_initial_latent": False,
            "enhance_inpaint_engine": "v2.6",
            "enhance_inpaint_strength": 1.0,
            "enhance_inpaint_respective_field": 0.618,
            "enhance_inpaint_erode_or_dilate": 0.0,
            "enhance_mask_invert": False
        }
    ]
}

response = requests.post(url, json=params)
```