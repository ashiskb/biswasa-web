---
title: 'Setting up your Ubuntu 22.04 to work with Tensorflow 2 (with GPU support)'
date: 2023-08-30
permalink: /posts/2023/08/biswas/blog-ubuntu-tensorflow/
tags:
  - ubuntu
  - ubuntu 22.04
  - linux
  - tensorflow
  - python
  - miniconda
  - cuda
  - cudnn
  - vscode
  - conda
---
Configuring Ubuntu/Linux operating system for experimenting Deep learning / Machine learning problems is simple enough. Here below are few steps / suggestions you can take to do the following: i) Install Python, ii) Install Microsoft Visual Studio Code (*aka* VSCode), iii) Install Tensorflow 2.0 with GPU support. For those of you who would just want to install Tensorflow 2.0 without GPU support, you can skip the **GPU setup** part below.

## Install Miniconda, Python, and Python Virtual Environments
  * `curl https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -o Miniconda3-latest-Linux-x86_64.sh`
  * Then, run it to complete the installation:
      * `bash Miniconda3-latest-Linux-x86_64.sh`
  * Close and re-open the current shell.
  * If you don’t want conda `(base)` environment to be activated every time, do the following:
      * `conda config —set auto_activate_base false`
* Create a conda environment
  * `conda create --name venv-tf2 python=3.9`
  * To activate this environment, use `conda activate venv-tf2`
  * To deactivate this environment, use `conda deactivate`

## GPU setup
  * Install NVIDIA GPU driver from https://www.nvidia.com/Download/index.aspx
  * In my case the options were (as of 8/11/2023): 
      * Product type: `GeForce`
      * Product Series: `GeForce 10 Series (Notebooks)`
      * Product: `GeForce GTX 1080`
      * Operating System: `Linux 64-bit`
      * Download type: `Production Branch`
      * Language: `English (US)`
  * This will download a `NVIDIA*.run` file. Make it executable with 
      * `chmod +x NVIDIA*.run`
  * Then, run it `./NVIDIA*.run` (as root)
      * **Issue 1**: Your system already has a proprietary driver installed. If you install this one, it may remove the proprietary driver.
          * Solution: Hit the “Continue Installation” button.
      * **Issue 2**: Unable to find the development tool `cc` in your path; please make sure that you have the package `gcc` installed. If `gcc` installed on your system, then please check that `cc` is in your PATH.
          * Solution: Hit the OK button. Now, install the development tools.
              * `sudo apt update`
              * `sudo apt install build-essential manpages-dev`
              * `sudo apt install gobjc gfortran gnat`  
              * Now, check for `gcc` and `g++` installation:
                  * `gcc --version` (as of 8/11/2023, the version is 11.4.0)
                  * `g++ --version` (as of 8/11/2023, the version is 11.4.0)
      * **Issue 3:** Kernel has probably the `CONFIG_MODULE_SIG` set for UEFI secure boot enabled system, which in my case is enabled:
        * Solution: 
          * Create a new pair of private and public key by running the following command:
              * `openssl req -new -x509 -newkey rsa:2048 -keyout /home/ashiskb/Nvidia-ashiskb-priv.key -outform DER -out /home/ashiskb/Nvidia-ashiskb-pub.der -nodes -days 36500 -subj "/CN=Graphics Drivers"`
          * Enroll the generated public key to MOK (Machine Owner Key) by entering the following command:
              * `sudo mokutil --import /home/ashiskb/NVIDIA-ashiskb-pub.der`
                  * This command requires you to create a password for enrolling.
                  * Afterwards, reboot the computer, and in the next boot, when system asks you to enroll, continue and enter the password you created during the enrollment.
                      * The firmware gives you option of viewing the new MOK or continue.
                      * After you hit continue, it will ask you to confirm the enrollment. Hit Yes.
                      * Then, it will ask you for the password you created.
          * Hit the `Sign the kernel` button.
          * Then, hit `Use existing key pair` button, and then it will ask you for the PATH of the private and public key pairs.
          * It may restart the system, or show blank screen couple of times. Then, the driver should be installed.
      * **Issue 4:** Unable to determine the path to install the `libglvnd` EGL vendor library config files. Check that you have `pkg-config` and the `libglvnd` development libraries installed, or specify a path with`--gland-egl-config-path`.
          * **Solution**: Hit OK
  * Verify GPU driver install:
      * Run `nvidia-smi`
          * It should show details about the GPU driver and configurations.
* **Install CUDA and cuDNN with condo and pip**:
  * Do the following with `$conda activate venv-tf2` environment.
  * `conda install -c conda-forge cudatoolkit=11.8.0`
  * `pip install nvidia-cudnn-cu11==8.6.0.163`
  * Now, configure system path and cuDNN paths:
      * Do the following steps to automatically configure so that these get set when you activate the conda environment:
      * `mkdir -p $CONDA_PREFIX/etc/conda/activate.d`
      * `echo 'CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn; print(nvidia.cudnn.__file__)"))' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh`
      * `echo 'export LD_LIBRARY_PATH=$CONDA_PREFIX/lib/:$CUDNN_PATH/lib:\$LD_LIBRARY_PATH' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh`
  
## Install Tensorflow
  * Again, do the following the `$conda activate venv-tf2` environment.
  * `pip install --upgrade pip`
  * Do not install tensorflow with conda (as of 8/11/2023).
  * `pip install tensorflow==2.13.*`
* Verify installation:
  * In the activated python shell:
  * `import tensorflow as tf`
  * `print(tf.config.list_physical_devices('GPU'))`
  * This should list a GPU instead of an empty list, if you have a GPU configured above.
  * **Issue 1**: In Ubuntu 22.04, you may encounter an Internal error: `libdevice not found at ./libdevice.10.bc [Op:__some_op]`. In my case, I did not get it luckily!
      * Solution: 
          * Install NVCC: 
            * `conda install -c nvidia cuda-nvcc=11.3.58`
          * Configure the XLA cuda directory:
            * `mkdir -p $CONDA_PREFIX/etc/conda/activate.d`
            * `$printf 'export XLA_FLAGS=--xla_gpu_cuda_data_dir=$CONDA_PREFIX/lib/\n' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh`
            * `source $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh `
          * Copy libdevice file to the required path
            * `mkdir -p $CONDA_PREFIX/lib/nvvm/libdevice`
            * `cp $CONDA_PREFIX/lib/libdevice.10.bc $CONDA_PREFIX/lib/nvvm/libdevice/`

## Configure VSCode
* In VSCode, hit <CTRL><SHIFT><p> to open the command prompt, where type:
  * `python: select interpreter`
      * By now, the conda environments should all the listed. Choose the one you like.

## Reference (08/12/2023):
[https://www.tensorflow.org/install/pip#linux](https://www.tensorflow.org/install/pip#linux)