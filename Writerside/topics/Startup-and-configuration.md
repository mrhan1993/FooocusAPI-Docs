# Startup and configuration

Although Fooocus is called a configuration, it does not have a separate configuration file. Almost all adjustable things are done through command line,
and there was a thought to change the options to a configuration file during the refactoring, but I chose to continue using Fooocus's way.

This means that you don't need to change your usage habits, you can still use Fooocus in the way of Fooocus.

Most of the time, you don't need to change your usage habits, you can still use Fooocus in the way of Fooocus.

> This section will mainly describe the new parts of FooocusAPI, and the parts that are affected by FooocusAPI, and some other necessary parts will also be mentioned.

You can use `python launch.py --help` to view all available command line arguments.

## FooocusAPI options

This configuration options are FooocusAPI's own, used to control FooocusAPI's behavior.

- `--nowebui`: Do not launch WebUI，Only API
- `--base-url BASE_URL`: Used for replacing `base_url` in the returned URL, default is `http://127.0.0.1`.
- `--apikey APIKEY`: If you specify this option, you need to add `X-API-KEY` to the `header` when requesting API, otherwise it will return 403.
- `--webhook-url WEBHOOK_URL`: When the task is completed, a POST request will be sent to this URL, which is very useful for asynchronous tasks, and its priority is lower than the `webhook_url` passed in the task parameters.

## Fooocus options

These are the options that Fooocus originally had, but now (maybe) will also affect the behavior of FooocusAPI.

- `--listen [IP]`: Used for listening on the specified IP, the default is `127.0.0.1`, if you do not specify the specific value, only specify `--listen`,
    which is equivalent to specifying `--listen 0.0.0.0`. This parameter also affects the WebUI and API services.
- `--port PORT`: Used for listening on the specified port, the default is `7865`, API uses the next port, the default is `7866`. This parameter also affects the WebUI and API services.
- `--output-path OUTPUT_PATH`: Used to specify the output path, the default is `outputs`, which is in the project root directory.
    I do not recommend modifying this option, because the current version has not been tested, which may affect the accuracy of the API result path.
- `--disable-image-log`: Used to disable saving images, which will cause the API service to fail to work.

Some options mainly affect the behavior of WebUI, which are invalid for API:

- `--in-browser`
- `--disable-in-browser`
- `--disable-preset-selection`
- `--disable-server-info`
- `--language LANGUAGE`
- `--theme THEME`
- `--disable-analytics`
- `--enable-auto-describe-image`

## Port configuration

There are two ways to modify the port that Fooocus's WebUI listens to, which are `--port PORT` and the environment variable `GRADIO_SERVER_PORT`.

These two ways will affect the port that Fooocus's API listens to, in addition, you can also modify the port of API service separately through the environment variable `API_PORT`.
The priority is as follows:

`API_PORT > --port PORT > GRADIO_SERVER_PORT`

> The reason for this is that I think it is not necessary to add a separate option to modify the port of the API service, and the usage is low, so reuse the port configuration of WebUI.
> But change the port is indeed necessary, and using `--port PORT` can also be used, but it is not intuitive enough, so `API_PORT` is added.
> Another reason is to reduce the configuration requirements, that is, out of the box, no additional configuration is required, at this time, the port of WebUI can be retained as much as possible to retain the user's usage habits.
