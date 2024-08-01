# How to

本章节目前多为示例，后续或许会增加其他内容。目前我将本章分为了两部分来演示。
其中第一部分是 Fooocus 的原生功能、特性，第二部分是 API 提供的独有功能、特性。

> 由于参数数量众多，且许多参数为公共参数，因此本章节示例中的请求不会使用太多的参数，此外，我可能会经常使用 `Lightning` 来加速生成，
> 完整的 `params` 解释位于 [Tutorial/Generate](GenerateEndpoint.md)

此外，Generate 接口提供了多种不同 Response 类型，它被统一放在了 [异步/同步](AsyncTask.md) 章节中。

- 第一部分：Fooocus 的原生功能、特性
  - [文生图](Text-to-image.md)
  - [局部重绘/扩图](InpaintOutpaint.md)
  - [超分/Vary](UpscaleVary.md)
  - [图生图](ImagePrompt.md)
  - [图像增强/修复](ImageEnhance.md)
  - [图像描述](DescribeImage.md)
  - [预设](Preset.md)
  - [高级功能(mix_ip and vary,inpaint)](MixImagePrompt.md)
- 第二部分：API 提供的独有功能、特性
  - [历史记录/任务管理](ManageTasks.md)
  - [hook](WebHook.md)
  - [自定义放大倍数/扩图距离](CustomUpscaleOutpaint.md)
  - [异步/同步](AsyncTask.md)
