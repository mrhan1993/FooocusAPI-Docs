# QuickStart

This section will guide you on how to deploy and use the project, Suppose you have confirmed that your equipment can run the project and have the relevant tools ready. 
Otherwise, you should read first [Requirement](Requirement.md).

## Install and run

Change to the directory where you want to place the project, if windows, open terminal, cmd or powershell, and then run the following commands:

> Note: The model file usually very large, so leave enough space for the project.

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

> You may be getting `running scripts is disabled on this system` when running the above command use powershell, Run powershell as administrator and execute `Set-ExecutionPolicy RemoteSigned` to solve this problem.

## Generate your first image

Wait for the program to complete the installation of dependencies and the download of the basic model, open `http://localhost:7866` in your browser, you will see the following page:

![openapi_page.png](openapi_page.png)

Click the `Try it out` button on the right side of the `POST /v1/engine/generate/` interface, and then click the `Execute` button to generate an image.

Base the performance of your equipment, it may take a few seconds or minutes to generate an image. After that, you can find an url in the `Response body` field,
open it in your browser, that is the image you just generated.

![image_2407251128.png](image_2407251128.png)

## Next step

[Tutorial](Tutorial.md)
