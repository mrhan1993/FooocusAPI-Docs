# ImageEnhance

This is a feature added in version 2.5, with more complex and numerous parameters. However, like ImagePrompt, the logic remains the same. Let's demonstrate with the WebUI:

![enhance_01.png](enhance_01.png)

By checking Enhance and switching to the Enhance tab, there are four additional tabs, with `Upscale or Variation` used for pre- or post-processing of the image.
`#1, #2, #3` indicate multiple treatments. Logically, it first generates a `mask` and then redoes the drawing based on the description words. You can consider
this as a combination of `Upscale or Variation`, `Inpaint`, and `Generate Mask`.

By default, the program returns images for each stage, For example, if you want to process an image as
follows: Vary (Strong) -> Enhance face -> Enhance mouse -> Enhance eye, you will get three result images. Use `save_final_enhanced_image_only` to get the final result.

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