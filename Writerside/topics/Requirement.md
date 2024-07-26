# Requirement

Here is a list of requirements for running Fooocus. include hardware requirement and software requirement.

The next is recommended, you can use different version.

A known issue is pyyaml==6.0 in python 3.12 can not install correctly, see this [issues](https://github.com/mrhan1993/FooocusAPI/issues/21) 

- Python >= 3.10 < 3.12
- CUDA >= 12.1
- Git

If you already have a Python installed, you can check the version by `python --version`.
If you don't have Python installed, you can download it from [here](https://www.python.org/downloads/).

Alternatively, you can use [miniconda](https://docs.anaconda.com/miniconda/) to install Python. You can choose according to your preference. Personally, I prefer miniconda.

Git is a version control system. You can download it from [here](https://git-scm.com/downloads).

## Minimal Requirement

> This table is from [Fooocus](https://github.com/lllyasviel/Fooocus)

Below is the minimal requirement for running Fooocus locally. If your device capability is lower than this spec, you may not be able to use Fooocus locally. (Please let us know, in any case, if your device capability is lower but Fooocus still works.)

| Operating System  | GPU                          | Minimal GPU Memory           | Minimal System Memory     | [System Swap](troubleshoot.md) | Note                                                                       |
|-------------------|------------------------------|------------------------------|---------------------------|--------------------------------|----------------------------------------------------------------------------|
| Windows/Linux     | Nvidia RTX 4XXX              | 4GB                          | 8GB                       | Required                       | fastest                                                                    |
| Windows/Linux     | Nvidia RTX 3XXX              | 4GB                          | 8GB                       | Required                       | usually faster than RTX 2XXX                                               |
| Windows/Linux     | Nvidia RTX 2XXX              | 4GB                          | 8GB                       | Required                       | usually faster than GTX 1XXX                                               |
| Windows/Linux     | Nvidia GTX 1XXX              | 8GB (&ast; 6GB uncertain)    | 8GB                       | Required                       | only marginally faster than CPU                                            |
| Windows/Linux     | Nvidia GTX 9XX               | 8GB                          | 8GB                       | Required                       | faster or slower than CPU                                                  |
| Windows/Linux     | Nvidia GTX < 9XX             | Not supported                | /                         | /                              | /                                                                          |
| Windows           | AMD GPU                      | 8GB    (updated 2023 Dec 30) | 8GB                       | Required                       | via DirectML (&ast; ROCm is on hold), about 3x slower than Nvidia RTX 3XXX |
| Linux             | AMD GPU                      | 8GB                          | 8GB                       | Required                       | via ROCm, about 1.5x slower than Nvidia RTX 3XXX                           |
| Mac               | M1/M2 MPS                    | Shared                       | Shared                    | Shared                         | about 9x slower than Nvidia RTX 3XXX                                       |
| Windows/Linux/Mac | only use CPU                 | 0GB                          | 32GB                      | Required                       | about 17x slower than Nvidia RTX 3XXX                                      |

&ast; AMD GPU ROCm (on hold): The AMD is still working on supporting ROCm on Windows.

&ast; Nvidia GTX 1XXX 6GB uncertain: Some people report 6GB success on GTX 10XX, but some other people report failure cases.

*Note that Fooocus is only for extremely high quality image generating. We will not support smaller models to reduce the requirement and sacrifice result quality.*

Note that the minimal requirement is 4GB Nvidia GPU memory (4GB VRAM) and 8GB system memory (8GB RAM). This requires using Microsoft’s Virtual Swap technique, which is automatically enabled by your Windows installation in most cases, so you
often do not need to do anything about it. However, if you are not sure, or if you manually turned it off (would anyone really do that?), or if you see any "RuntimeError: CPUAllocator", you can enable it here:

![260322660-2a06b130-fe9b-4504-94f1-2763be4476e9.png](260322660-2a06b130-fe9b-4504-94f1-2763be4476e9.png)

## CUDA

Before the next step, install cuda driver first, you can find version list here: [CUDA Toolkit Archive](https://developer.nvidia.com/cuda-toolkit-archive)
