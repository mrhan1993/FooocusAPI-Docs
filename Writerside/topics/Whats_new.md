# What's new

## 2024-08-01 -- v2.5.2

- Merge Fooocus 2.5.2, update log for Fooocus is [here](https://github.com/lllyasviel/Fooocus/releases/tag/v2.5.2)

【New feature】
- Use `save_final_enhanced_image_only` to get the final result when using multiple `enhance`

【Fix】
- fix a doc error for enhance
- When the requested image does not exist, a 404 is returned instead of an Internal error

## 2024-07-31 -- v2.5.0

- Merge Fooocus 2.5.0, update log for Fooocus is [here](https://github.com/lllyasviel/Fooocus/releases/tag/v2.5.0)

【New feature】
- New documentation [FooocusAPI-Docs](https://mrhan1993.github.io/FooocusAPI-Docs)
- Custom upscale multiple
- Custom outpaint distance
- Endpoint for generate mask
- vae support in preset
- save_name parameter is back, issue #29

【Fix】
- fix an error caused by parameter changes
- fix ge value for sam_max_detections, it should be 0
- fix describe image, now is working
- fix an error when inpaint mask is png
- fix an error for webhook
