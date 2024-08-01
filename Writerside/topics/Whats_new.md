# 更新日志

## 2024-08-01 -- v2.5.2

- 合并 Fooocus 2.5.2, Fooocus 的[升级日志](https://github.com/lllyasviel/Fooocus/releases/tag/v2.5.2)

【新特性】
- 当使用了多个图像增强时，使用 `save_final_enhanced_image_only` 来只获取最终图像

【修复】
- 修复一个关于图像增强的文档错误
- 请求图像不存在时，返回 404 而不再是 Internal error

## 2024-07-31 -- v2.5.0

- 合并 Fooocus 2.5.0，Fooocus 的更新日志在[这里](https://github.com/lllyasviel/Fooocus/releases/tag/v2.5.0)

【新特性】

- 新文档 [FooocusAPI-Docs](https://mrhan1993.github.io/FooocusAPI-Docs)
- 自定义放大倍数
- 自定义扩图距离
- `generate mask` endpoint
- 预设中支持 `vae`
- `save_name` 参数回归，issue #29

【修复】

- 修复由参数更改引起的错误
- 修复 `sam_max_detections` 的最小值，应为0
- 修复描述图像，现在可以工作了
- 修复当 inpaint mask 格式为 png 时可能会遇到的问题
- 修复 webhook 的错误
