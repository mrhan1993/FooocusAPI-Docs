# 快速开始

本节将指导您如何部署和使用项目，假设您已经确认您的设备可以运行项目并且已经准备好了相关工具。
否则，您应该首先阅读[要求](Requirement.md)。

## 安装运行

切换到您想要放置项目的目录，如果是 Windows 系统，请打开终端、cmd 或 PowerShell，然后运行以下命令：

> 注意：模型文件通常非常大，因此请为项目留出足够的空间。

### Use conda

**Linux or Windows**

```shell
git clone https://github.com/mrhan1993/FooocusAPI
cd FooocusAPI
conda env create -f environment.yml
conda activate fooocus
python launch.py --nowebui
```

### Use venv

**Linux**

```bash
git clone https://github.com/mrhan1993/FooocusAPI
cd FooocusAPI
python3 -m venv venv
source venv/bin/activate
python launch.py --nowebui
```

**Windows**

```bash
git clone https://github.com/mrhan1993/FooocusAPI
cd FooocusAPI
python -m venv venv
venv\Scripts\activate
python launch.py --nowebui
```

> 当您使用 PowerShell 运行上述命令时，可能会遇到 `running scripts is disabled on this system` 的错误。请以管理员身份运行 PowerShell ，并执行 `Set-ExecutionPolicy RemoteSigned` 命令来解决这个问题。

## 生成第一张图片

等待程序完成依赖项的安装和基础模型的下载，然后在您的浏览器中打开`http://localhost:7866`，您将看到以下页面：

![openapi_page.png](openapi_page.png)

点击`POST /v1/engine/generate/`接口右侧的`Try it out`按钮，然后点击`Execute`按钮以生成图像。根据您的设备性能，生成图像可能需要几秒钟或几分钟。之后，您可以在`Response body`字段中找到一个url，

在您的浏览器中打开它，那就是您刚刚生成的图像。

![image_2407251128.png](image_2407251128.png)

## Next step

[Tutorial](Tutorial.md)
