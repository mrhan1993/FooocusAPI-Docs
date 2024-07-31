# Overview

FooocusAPI is a refactor of [Fooocus-API](https://github.com/mrhan1993/Fooocus-API), they are both the secondary development of [Fooocus](https://github.com/lllyasviel/Fooocus).
It is used to solve the problem that the API call of Gradio in the original project is difficult to understand and difficult to meet the use of Fooocus as a service deployment.

> As this project is a branch of Fooocus, so the document will have some content directly taken from Fooocus document, and there are many references to it.

## How about Fooocus work

When you start reading the code of Fooocus, you will find that most of the main logic of the parameters, task management, etc.
are concentrated in the [async_worker.py](https://github.com/lllyasviel/Fooocus/blob/main/modules/async_worker.py) file.

> Next is my personal understanding of the code, which does not represent the official Fooocus, and is for reference only.
> And only the basic execution flow is designed, not the more low-level model inference part.

Because [Fooocus v2.5.0](https://github.com/lllyasviel/Fooocus/releases/tag/v2.5.0) refactored this file, so here is a brief explanation of the work logic of this file at 2.4.3:

First，a class `AsyncTask` is defined to instantiate task objects

```python
class AsyncTask:
    def __init__(self, args):
        self.args = args
        self.yields = []
        self.results = []
        self.last_stop = False
        self.processing = False
```

- `args`: Used for store task parameters, it is a list, which contains the information submitted by the user from WebUI
- `yields`: Used for storing the progress, intermediate results, and preview images generated during the task execution. It is a list
- `results`：Used for store the final result of the task, it is a list, and will contain the local path of the generated image
- `last_stop`：Used for record the last stop status of the task, which can be `skip` or `stop`, and will be checked in the loop of the task execution
- `processing`：Used for record the current status of the task, if it is True, it means that the task is being executed

And then, defined a list `async_tasks = []`, which is a queue of tasks to be executed, and there is a loop that will try to take tasks from it and execute them.

Next is a long 1000-line method `worker`, most of which are various parameter processing, but no problem, we start from the beginning one by one.

When a task is submitted, the information submitted by the user from WebUI will be instantiated as a task object and added to the `async_tasks` list,
the process is in the 50th line of `webui.py`, next, the loop that checks the `async_tasks` list will take the task and execute it.
This code is located at the end of `async_tasks.py`:

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

After task is taken, it will be passed to `handler` for processing (135 line)

First, `handler` will take all the parameters and do some preliminary processing, this part end at 220 line

Next, there is further processing of the parameters, such as case conversion, determining the number of steps based on the selected performance, adjusting the model, lora,
and corresponding other parameters, and performing possible model downloads, which should be done around line 343

And then defined two list: `goals = []` and `tasks = []`, They are respectively used to store image processing labels, i.e `uov` `inpaint` `ip`,
 `tasks` Used to split tasks, when `image_number > 1` the task will be split into multiple tasks, and the `tasks` list will be used to store the split tasks

Next, depending on whether `input_image` is checked, this part of the code is executed. Its function is to add markers to the `goals` list based on
the current `tab` and the checked status of `mixing_image_prompt_and_vary_upscale` and `mixing_image_prompt_and_inpaint`. At the same time,
various uploaded images are preprocessed and possible model downloads are performed. This section is located approximately at lines `348-422`.

Regardless of whether `input_image` is checked or not, the code will proceed to the `skip_prompt_processing` judgment after
a brief loading of the model and overlay of parameters. This logic is located at lines `448-549`. Its function is to expand
the description words and reverse description words based on the selected `styles` for model optimization.

What follows is a series of processing steps based on the content of `goals`. Apart from `upscale fast`, which will return the result directly,
the other situations are still processed in stages until line 868, where the `tasks` list is iterated over. If everything goes smoothly, 
the final processing will be done here, such as formatting metadata, saving files, and returning the results.

Throughout the entire process, the status of task execution is continuously updated through the `yields` property of the task object.
By using the `callback` function, we can clearly see the storage structure in the list:

```python
def callback(step, x0, x, total_steps, y):
    done_steps = current_task_id * steps + step
    async_task.yields.append(['preview', (
        int(flags.preparation_step_count + (100 - flags.preparation_step_count) * float(done_steps) / float(all_steps)),
        f'Sampling step {step + 1}/{total_steps}, image {current_task_id + 1}/{image_number} ...', y)])
```

After a simple calculation, a list similar to the following will be obtained: `['preview', (60, 'Sample step 60/100, image 1/1 ...', y)]`.
The meaning of the elements in this list is as follows:

- `preview`：This is similar to a phase identifier, and the information it can provide is limited.
- Tuple：
  - 60: The progress, which is easy to understand, refers to the overall progress.
  - 'Sample step 60/100, image 1/1 ...': The description of the current step
  - y：This refers to the image for each step, which means that using this, you can see the process of an image being generated.

## Thinking When Reconstructing

In the [Fooocus-API](https://github.com/mrhan1993/Fooocus-API) project created by [konieshadow](https://github.com/konieshadow), he implemented a new task queue and built new task objects based on FastAPI. Then, by rewriting some of the logic in `async_worker.py`, he completed the development of Fooocus-API.

After taking over and maintaining the project for half a year, the issues caused by this processing method have become increasingly difficult to handle.
The main problems are the following two:

1. When dealing with updates to the Fooocus version, it is necessary to synchronously update the code in `async_worker.py`. As a generator, one must be careful to handle each change.
2. The startup of the project is based on the premise of starting a FastAPI service, which prevents the reuse of the pre-startup logic in Fooocus and requires reimplementation. 
This includes tasks such as dependency installation, environment detection, configuration file reading, etc. Although these can be achieved through simple code duplication.

Additionally, there are some historical minor issues, such as:

- Inability to use WebUI simultaneously.
- Need to request a separate EndPoint to obtain progress images.
- Incomplete persistence of task information and inconsistent return data formats.

Based on the above issues, I have decided to refactor Fooocus-API with the following approach:


1. Utilize the task handling logic in `async_worker.py`, with the API solely responsible for receiving parameters and passing them to the task handling logic.
2. Abandon the separately maintained queue and reuse the queue in `async_worker.py`.
3. Merge interface functions, since all parameters are ultimately processed through the `handler` function in `async_worker.py`, the API only needs to be responsible for receiving parameters and passing them on. Separate interfaces are unnecessary.


## How about FooocusAPI work

So, I redesigned the structure of FooocusAPI.

Add a method in `webui.py` to start the API service and WebUI, at this time, we can use WebUI and API service at the same time

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

And then, put all the parameters into a model [`CommonRequest`](https://github.com/mrhan1993/FooocusAPI/blob/main/apis/models/requests.py)

After that, I added a new function [`pre_process`](https://github.com/mrhan1993/FooocusAPI/blob/main/apis/utils/pre_process.py), which is used to preprocess the parameters,
e.g. convert params, save image, download model, etc.

And then, use [`api_utils`](https://github.com/mrhan1993/FooocusAPI/blob/main/apis/utils/api_utils.py) to process the parameters,
and add it to `async_tasks` list in [`async_worker.py`](https://github.com/mrhan1993/FooocusAPI/blob/main/modules/async_worker.py)

In the end, [`call_worker`](https://github.com/mrhan1993/FooocusAPI/blob/main/apis/utils/call_worker.py) will monitor the execution status of the task
and return different results based on different parameters when the task is completed.

After the refactoring is completed, the original functionality will be preserved to the maximum extent possible, while also allowing API services to coexist with WebUI.
Although modifications to `asyncw_worker.py` are still required for features that are not available in Fooocus, such as custom magnification and support for Outpaint customization,
the amount of modifications has been greatly reduced. Additionally, due to the limited modifications made to Fooocus, the maintenance cost of the API is greatly
reduced in the absence of major version changes, making it easier to track updates to Fooocus. For minor updates, simply merging upstream code is sufficient.