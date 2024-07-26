# Startup and configuration

尽管称作配置，但 Fooocus 并没有独立的配置文件，几乎所有可调节的东西都是通过命令行进行，在重构时有过把这些选项改为配置文件的想法，不过还是选择了沿用 Fooocus 的做法。
这意味着，你无需改变使用习惯，仍可以按照 Fooocus 的方式启动 FooocusAPI。也因为配置都是通过命令行选项进行，所以启动和配置被放到了一起。

绝大多数情况下，无论是 Fooocus 还是 FooocusAPI，默认配置已经足够使用。但如果你想进行一些自定义配置，那么本章节将提供一些帮助。

> 本节中，将主要描述 FooocusAPI 新增的部分，以及受其影响的 Fooocus 部分，其他一些必要的部分也会被提及。

你可以使用 `python launch.py --help` 参数查看所有可用的命令行参数。

## FooocusAPI 选项

这些配置选项是 FooocusAPI 独有的，用来控制 FooocusAPI 的行为。

- `--nowebui`: 不启动 WebUI，只启动 API 服务。
- `--base-url BASE_URL`: 用来替换返回的 URL 中的 `base_url`，默认为 `http://127.0.0.1`。
- `--apikey APIKEY`: 如果指定了该参数，那么请求 API 时，需要在 `header` 中添加 `X-API-KEY`，否则返回 403。
- `--webhook-url WEBHOOK_URL`: 指定该参数时，当任务完成时，会向该 URL 发送 POST 请求，这对异步任务非常有用，其优先级低于任务参数中传递的 `webhook_url`。

## Fooocus 选项

这些是 Fooocus 原本就有的选项，但现在(可能)也会影响 FooocusAPI 的行为。

- `--listen [IP]`: 用来指定监听的 IP 地址，默认为 `127.0.0.1`，如果不指定具体的值，仅指定 `--listen`，等同于指定 `--listen 0.0.0.0`。该参数同时影响 WebUI 和 API 服务。
- `--port PORT`: 用来指定监听的端口，默认为 `7865`, API 默认使用该端口 `+1` 的端口，该参数同时影响 WebUI 和 API 服务。
- `--output-path OUTPUT_PATH`: 用来指定输出路径，默认为会输出到项目根目录下的 `outputs` 文件夹中，当前我不建议修改该选项，原因是当前版本没有对这个进行测试，可能会影响 API 结果路径的准确性。
- `--disable-image-log`: 用来禁止保存图像，即不保存图片到 `output_path` 中，禁用会导致 API 服务无法正常工作。

还有一些选项，主要影响 WebUI 的行为，对 API 无效：

- `--in-browser`
- `--disable-in-browser`
- `--disable-preset-selection`
- `--disable-server-info`
- `--language LANGUAGE`
- `--theme THEME`
- `--disable-analytics`
- `--enable-auto-describe-image`

## 端口配置

Fooocus 有两种方式修改其 WebUI 监听的端口，分别是通过 `--port PORT` 和环境变量 `GRADIO_SERVER_PORT`。
这两种方式都会影响 API 服务的端口，除此之外，还可以通过环境变量 `API_PORT` 单独修改 API 服务的端口。它们的优先级为：
`API_PORT > --port PORT > GRADIO_SERVER_PORT`

> 之所以变成这样一个原因是我觉得没必要单独增加一个选项来修改 API 服务的端口，而且使用率低，所以复用了 WebUI 的端口配置。但更改端口又确实有需求，使用 `--port PORT` 尽管也可以，但不够直观，所以增加了 `API_PORT`。
> 另一个原因是为了降低配置要求，即开箱即用，不需要额外配置，此时沿用 WebUI 的端口可以尽可能保留用户的使用习惯。

