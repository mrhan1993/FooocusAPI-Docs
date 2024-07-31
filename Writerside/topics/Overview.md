# 概述

FooocusAPI 是对 [Fooocus-API](https://github.com/mrhan1993/Fooocus-API) 项目的重构，它们都是对 [Fooocus](https://github.com/lllyasviel/Fooocus) 项目的二次开发，
旨在解决原项目中 Gradio 的 API 调用难以理解，且难以满足将 Fooocus 作为服务部署时的使用问题。

> 由于该项目属于 Fooocus 分支，因此文档会有部分内容直接取自 Fooocus 文档，且对其有许多参考。

## How about Fooocus work

当你开始阅读 Fooocus 的代码时很容易发现，其对于参数的处理、任务管理等主要逻辑都集中于 [async_worker.py](https://github.com/lllyasviel/Fooocus/blob/main/modules/async_worker.py) 这个文件。

> 以下内容为本人阅读项目代码后的个人理解，不代表 Fooocus 官方，仅供参考。并且只设计基础的执行流程，不包括更加底层的模型推理部分。

由于 [Fooocus v2.5.0](https://github.com/lllyasviel/Fooocus/releases/tag/v2.5.0) 对该文件进行了重构，因此这里以 2.4.3 为例简要说明该文件的工作逻辑：

首先，该文件定义了一个 `AsyncTask` 类，它用来实例化任务对象

```python
class AsyncTask:
    def __init__(self, args):
        self.args = args
        self.yields = []
        self.results = []
        self.last_stop = False
        self.processing = False
```

- `args`：用于存储任务参数，它是一个列表，包含用户从 WebUI 提交来的信息
- `yields`：用于存储任务执行过程中产生的中间结果，它是一个列表，会包含进度、正在执行的操作、预览图信息
- `results`：用于存储任务执行完成后产生的结果，它是一个列表，会包含生成的图片的本地路径
- `last_stop`：用于记录任务中断信息，可以是 skip, stop，在任务执行的循环中会一直检查该属性
- `processing`：用于记录任务是否正在执行，如果为 True 则表示任务正在执行

然后，定义了一个列表 `async_tasks = []`，该列表作为一个待执行任务的队列，有一个死循环会尝试从中拿取任务并执行

接下来是一个长达一千行的方法 `worker`，这其中绝大部分是对于参数的各种处理，但是没有关系，我们一点点从头开始。

当一个任务被提交，也即 WebUI 上的 Generate 按钮被按下时，从 WebUI 上提交的信息会首先被实例化为一个 task 对象并添加到 `async_tasks` 列表中，
这个过程在 `webui.py` 的第 50 行，

之后，一直对 `async_tasks` 列表进行循环检查的循环会拿取这个任务，这部分代码位于 `async_tasks.py` 的末尾：

```python
while True:
    time.sleep(0.01)
    if len(async_tasks) > 0:
        task = async_tasks.pop(0)
        generate_image_grid = task.args.pop(0)

        try:
            handler(task)
            if generate_image_grid:
                build_image_wall(task)
            task.yields.append(['finish', task.results])
            pipeline.prepare_text_encoder(async_call=True)
        except:
            traceback.print_exc()
            task.yields.append(['finish', task.results])
        finally:
            if pid in modules.patch.patch_settings:
                del modules.patch.patch_settings[pid]
```

任务被拿取之后被传递给了 `handler` 处理（135行开始）

首先，`handler` 将所有参数取出，并对部分参数进行了初步的整理，这部分大概到 220 行结束

接下来，是对参数的进一步处理，比如大小写转换、根据选择的 `performance` 确定步数、模型、lora以及对应的其他参数的调整，进行可能的模型下载，这大概到 343 行

然后是两个列表的定义：`goals = []` 和 `tasks = []` ，它们分别用于存储图像处理标记也即 `uov` `inpaint` `ip`，`tasks` 则用于出处拆分任务，当 `image_number > 1` 时，会被拆分并暂存于此

接下来，根据是否勾选了 `input_image` 执行该部分代码，其作用是根据当前所在的 `tab` 以及 `mixing_image_prompt_and_vary_upscale`
和 `mixing_image_prompt_and_inpaint` 的勾选情况向 `goals` 列表增加标记，同时上传的各种图像进行预处理、以及可能的模型下载。这部分大概位于 `348-422` 行

无论是否勾选 `input_image` 都会在一个短暂的加载模型和覆盖参数之后进入到 `skip_prompt_processing` 的判断。这个逻辑位于 `448-549` 行，其作用是对描述词、反向描述词
根据选择的 `styles` 进行展开，模型优化

接下来的是根据 `golas` 中的内容进行的一系列处理。除开 upscale fast 会直接返回结果，其他几种情况依然是阶段性处理，直到 868 行对 tasks 列表进行循环。如果一切顺利，会在这里做最后的处理，比如格式化元数据、保存文件、返回结果

整个过程中，任务执行状态通过该任务对象的 `yields` 属性不断更新。通过 `callback` 函数，我们可以很清楚看到列表中的存储结构：

```python
def callback(step, x0, x, total_steps, y):
    done_steps = current_task_id * steps + step
    async_task.yields.append(['preview', (
        int(flags.preparation_step_count + (100 - flags.preparation_step_count) * float(done_steps) / float(all_steps)),
        f'Sampling step {step + 1}/{total_steps}, image {current_task_id + 1}/{image_number} ...', y)])
```

简单计算后会得出一个类似下边的列表 `['preview', (60, 'Sample step 60/100, image 1/1 ...', y)]`，这个列表中元素含义如下：

- `preview`：这类似于是一个阶段标识，其本身可以提供的信息有限
- 元组：
  - 60：进度，这很容易理解，而且是总进度
  - 'Sample step 60/100, image 1/1 ...'：进度的文字描述
  - y：这个是每一步的图像，也就是拿这个可以看到一个图像的生成过程

## Thinking When Reconstructing

在 [konieshadow](https://github.com/konieshadow) 创建的 [Fooocus-API](https://github.com/mrhan1993/Fooocus-API) 项目中，他实现了一个新的任务队列，并基于 FastAPI 构建了新的任务对象，然后通过重写 `async_worker.py` 的部分逻辑完成了对 Fooocus-API 的开发。

但是在接手并维护该项目半年后，这种处理方式带来的问题变得越发难以处理，最主要的是下面两个问题：

1. 在应对 Fooocus 更新版本时，需要同步更新 `async_worker.py` 中的代码，作为生成器，需要小心应对每一次变动
2. 项目的启动是以启动一个 FastAPI 服务为前提的，这导致 Fooocus 中的前置启动逻辑无法被复用，需要重新实现，比如依赖的安装、环境检测、配置文件读取等，尽管可以通过简单的复制代码实现

此外，还有一些历史遗留的小问题，比如：

无法和 WebUI 同时使用

获取进度图需要请求单独的 EndPoint

任务持久化信息保存不完整、返回数据格式不够统一

基于以上问题，我决定对 Fooocus-API 进行重构，其思路如下：

1. 使用 `async_worker.py` 中的任务处理逻辑，API 只负责接收参数并传递给任务处理逻辑
2. 放弃单独维护的队列，复用 `async_worker.py` 中的队列
3. 合并接口功能，既然所有参数最终都是通过 `async_worker.py` 中的 `handler` 函数处理的，那么 API 只需要负责接收参数并传递即可，单独分离的接口是不必要的

## How about FooocusAPI work

基于上述逻辑，我重新设计了 FooocusAPI 的结构。

首先，在 `webui.py` 中增加一个方法，用来启动 API 服务，以及 WebUI，此时，我们可以同时使用 WebUI 和 API 服务

```python
def run_gradio():
    """
    Run the gradio interface
    """
    shared.gradio_root.launch(
        inbrowser=args_manager.args.in_browser,
        server_name=args_manager.args.listen,
        server_port=args_manager.args.port,
        share=args_manager.args.share,
        auth=check_auth if (args_manager.args.share or args_manager.args.listen) and auth_enabled else None,
        allowed_paths=[modules.config.path_outputs],
        blocked_paths=[constants.AUTH_FILENAME]
    )


if not args_manager.args.nowebui:
    Thread(target=run_gradio, daemon=True).start()
run_server(args_manager.args)
```

然后，将所有的参数放到一个模型 `CommonRequest` 中。

接下来，通过新增一个 `pre_process` 方法，对参数进行预处理，比如保存文件、请求参数、以及一些必要的转换

之后，通过 `api_utils` 方法，将参数整理成 `async_worker.py` 中的 `handler` 方法需要的格式，将任务对象传递给 `async_worker.py` 中的 `worker` 方法

最后，重写 `call_worker` 方法，根据请求参数将不同的数据以不同的方式返回，在返回之前，任务对象会交由 `post_worker` 做最后处理，比如保存结果、发送 Hook 信息等

重构完成后，将最大限度保留原有的功能，同时，也使得 API 服务可以和 WebUI 共存，尽管由于对 Fooocus 中没有的功能如自定义放大倍数、Outpaint自定义的支持依然需要对 `async_worker.py` 进行修改，但是，其修改量已经大大减少
同时，由于对 Fooocus 的修改非常有限，因此，在非大版本变动的情况下，对于 API 的维护成本也大大降低，对于追踪 Fooocus 的更新也更加方便，对于小幅度更新，只需要简单的合并上游的代码即可。
