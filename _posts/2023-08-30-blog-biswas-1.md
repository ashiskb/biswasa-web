---
title: 'Setting up your Windows 10 to work with Tensorflow 2 (with GPU support)'
date: 2023-08-30
permalink: /posts/2023/08/biswas/blog-win10-tensorflow/
tags:
  - windows 10
  - windows
  - tensorflow
  - python
  - miniconda
  - cuda
  - cudnn
  - vscode
  - conda
---
Configuring Windows 10 operating system for experimenting Deep learning / Machine learning problems may seem challenging at first. But, you can live with it. Here below are few steps / suggestions you can take to do the following: i) Install Python, ii) Install Microsoft Visual Studio Code (*aka* VSCode), iii) Install Tensorflow 2.0 with GPU support. For those of you who would just want to install Tensorflow 2.0 without GPU support, you can skip the **GPU setup** part below.


## Install and Configure Python
* Install Python from [https://www.python.org/downloads/windows/](https://www.python.org/downloads/windows/).
  * This comes with both `python` interpreter and `pip`.
* Add python install directory to the systen Path. Reference on how to do it can be found [here](https://learn.microsoft.com/en-us/previous-versions/office/developer/sharepoint-2010/ee537574(v=office.14)). 
* Reopen a terminal/command prompt to see if the `python` command works. Please note the installed python version.
* 
* Install `Miniconda` via the installer from the [Miniconda site](https://docs.conda.io/projects/conda/en/latest/user-guide/install/windows.html).
* Upon completion, at the start menu, search: `Anaconda Prompt`
  * Check the `conda` version by typing `conda --version`
  * On a regular basis, update `conda` with the command: `conda update conda`

## Managing Python Virtual environments
* Create a virtual environment named `venv-p39` by the command: `conda create --name venv-p39 python=3.9`
* Activate the environment by: `conda activate venv-p39`
* Within the activated environment, install packages with `conda`. For example,
  * `conda install conda-forge scikit-learn`
* You can list all packages in the activated environment: `conda list`
* To deactivate the currently activated virtual environment, do this: `conda deactivate`
* You can list all virtual environments and their absolute paths by: `conda info --envs`

## Configuring VSCode to use the Conda virtual environments you create
* If you would like the conda environments to be listed as interpreters, Go: `File` > `Settings`. Then, search for `venv`, and then to `Python: Venv Path` as `C:\Users\xyz\conda\envs`, where replace `xyz` with your username in the system, and `conda` may either remain as `conda` or `miniconda3` depending on your installation. Please double check the absolute paths by inspecting `conda info --envs`.
* Reopen VSCode.

## Installing Tensorflow 2.0
* Tensorflow 2.0 requirements are:
  * python3.8-3.11
  * Pip 19.0+
  * Microsoft Visual C++ Redistributable for Visual Studio 2015, 2017, 2019
  * NVIDIA GPU Drivers: 450.80.02+
  * CUDA toolkit 11.8 ** (please be precise) — check for archived releases.
  * cuDNN SDK 8.6.0 ** (please be precise) — check for archived releases.
* `Tensorflow 2.10`` was the last Tensorflow release supporting GPU on `native windows``. Starting from `Tensorflow 2.11``, you will need to install Tensorflow in Windows Subsystem for Linux (WSL2), or try Tensorflow-DirectML-Plugin. Below are the steps to install `Tensorflow 2.10`:
  * Create a conda environment: `conda create --name venv-tf2 python=3.9`
  * Activate the environment: `conda activate venv-tf2`
  * Deactivate the environment: `conda deactivate`
  * Activate it again. Haha! Just checking whether you are following the steps.
  * **GPU setup**:
      * Install NVIDIA GPU driver (in my case it was `NVIDIA GeFroce GTX 1080` (Notebook) from their [official website](https://www.nvidia.com/Download/index.aspx?lang=en-us).
      * Then with `conda` install `CUDA`, `cuDNN` using the following command:
          * `conda install -c conda-forge cudatoolkit=11.2 cudnn=8.1.0`
  * `pip install --upgrade pip`
  * Since, `windows 10 native` will not support beyond Tensorflow 2.10 version. Install it with:
      * `pip install "tensorflow<2.11"`
  * Verify install:
      * Run the following code in activated python shell:
          * `import tensorflow as tf`
          * `tf.config.list_physical_devices('GPU')`
              * This should list all installed GPU devices in the system (if available) instead of an empty list (if not available)
          * `!nvidia-smi`
              * This will show you further details about the GPU and its configurations.


## References
  * Official Guide to Tensorflow installation [ link ](https://www.tensorflow.org/install/pip#windows-native)