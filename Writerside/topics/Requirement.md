# 环境要求

以下是运行Fooocus所需的条件，包括硬件要求和软件要求。

接下来是推荐配置，您可以使用不同版本。

已知的问题是 python 3.12 中的 `pyyaml==6.0` 无法正确安装，请参阅这个[问题](https://github.com/mrhan1993/FooocusAPI/issues/21)

- Python >= 3.10 < 3.12
- CUDA >= 12.1
- Git

如果您已经安装了Python，可以通过`python --version`命令检查版本。
如果您还没有安装Python，可以从[这里](https://www.python.org/downloads/)下载。

另外，您也可以使用[miniconda](https://docs.anaconda.com/miniconda/)来安装Python。您可以根据自己的喜好选择。我个人更喜欢miniconda。

Git是一个版本控制系统。您可以从[这里](https://git-scm.com/downloads)下载。

## 最低硬件需求

> 从 [Fooocus](https://github.com/lllyasviel/Fooocus) 复制过来的

以下是本地运行Fooocus所需的最小配置要求。如果您的设备性能低于这个规格，您可能无法在本地使用Fooocus。（无论哪种情况，如果您的设备性能较低但Fooocus仍然可以工作，请告诉我们。）

| 操作系统              | GPU              | 最低显存                         | 最低系统内存 | [虚拟内存](troubleshoot.md) | 备注                                                    |
|-------------------|------------------|------------------------------|--------|-------------------------|-------------------------------------------------------|
| Windows/Linux     | Nvidia RTX 4XXX  | 4GB                          | 8GB    | 需要                      | 最快                                                    |
| Windows/Linux     | Nvidia RTX 3XXX  | 4GB                          | 8GB    | 需要                      | 通常比 RTX 2XXX 快                                        |
| Windows/Linux     | Nvidia RTX 2XXX  | 4GB                          | 8GB    | 需要                      | 通常比 GTX 1XXX 快                                        |
| Windows/Linux     | Nvidia GTX 1XXX  | 8GB (&ast; 6GB 不确定)          | 8GB    | 需要                      | 比 CPU 强点                                              |
| Windows/Linux     | Nvidia GTX 9XX   | 8GB                          | 8GB    | 需要                      | 不一定比 CPU 强                                            |
| Windows/Linux     | Nvidia GTX < 9XX | 不支持                          | /      | /                       | /                                                     |
| Windows           | AMD GPU          | 8GB    (updated 2023 Dec 30) | 8GB    | 需要                      | DirectML (&ast; ROCm 还没做好), 大概比 Nvidia RTX 3XXX 慢 3 倍 |
| Linux             | AMD GPU          | 8GB                          | 8GB    | 需要                      | ROCm, 大概比 Nvidia RTX 3XXX 慢 1.5 倍                     |
| Mac               | M1/M2 MPS        | 共享                           | 共享     | 共享                      | 大概比 Nvidia RTX 3XXX 慢 9 倍                             |
| Windows/Linux/Mac | only use CPU     | 0GB                          | 32GB   | 需要                      | 大概比 Nvidia RTX 3XXX 慢 17 倍                            |

* AMD GPU ROCm（暂停支持）：AMD仍在努力支持Windows上的ROCm。
* Nvidia GTX 1XXX 6GB 不确定：有些人报告在GTX 10XX上6GB成功，但也有人报告失败案例。

*请注意，Fooocus仅用于生成极高画质的图像。我们不会支持更小的模型以降低要求并牺牲结果质量。*

请注意，最小要求是4GB Nvidia GPU内存（4GB VRAM）和8GB系统内存（8GB RAM）。这需要使用微软的虚拟交换技术，在大多数情况下，您的Windows安装会自动启用它，所以您通常不需要对此做任何事情。
但是，如果您不确定，或者如果您手动关闭了它（真的会有人这么做吗？），或者如果您看到任何"RuntimeError: CPUAllocator"错误，您可以在以下位置启用它：

![260322660-2a06b130-fe9b-4504-94f1-2763be4476e9.png](260322660-2a06b130-fe9b-4504-94f1-2763be4476e9.png)

## CUDA

再进行下一步之前，确认安装了 CUDA，这是历史版本列表: [CUDA Toolkit Archive](https://developer.nvidia.com/cuda-toolkit-archive)
