---
title: 'Setting up your Ubuntu 22.04 to work with Tensorflow 2 and PyTorch (+GPU support) in VSCode, Jupyter Notebook, and Jupyter Hub (optional)'
date: 2023-08-30
toc: true
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
  - jupyter
  - jupyter notebook
  - jupyterhub
---
Configuring Ubuntu/Linux operating system for experimenting Deep learning / Machine learning problems is simple enough. Here below are few steps / suggestions you can take to do the following: i) Install Python, ii) Install Microsoft Visual Studio Code (*aka* VSCode), iii) Install Tensorflow 2.0 with GPU support, iv) Install Pytorch with GPU support v) Setting up Jupyter hub. For those of you who would just want to install Tensorflow 2.0 without GPU support, you can skip the **GPU setup** part below.

## Revision History of this post
* 2023-10-19 -- Added recommendations to create per-user conda environment that shows GPU devices in jupyter notebooks.
* 2023-10-16 -- separated the sections by user types (Administrator or a regular user). I tested most of the `Admin@Server+Desktop` on both Ubuntu 22.04 Server and Ubuntu 22.04 Desktop computers. And, the mentioned `regular user` is just a user without any privileged access.
* 2023-08-30 -- initial version of the steps.

## (Admin@Server+Desktop) Install Miniconda Python
  * `curl https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -o Miniconda3-latest-Linux-x86_64.sh`
  * Then, run it to complete the installation and make sure to install it in a non-user home directory. For example, install it in `/opt/miniconda3/`:
      * `bash Miniconda3-latest-Linux-x86_64.sh`
      * Make sure `conda` sets the `$PATH` variables.
  * Close and re-open the current shell.
  * If you don’t want conda `(base)` environment to be activated every time, do the following:
      * `conda config —-set auto_activate_base false`
* `conda update conda`

## (Regular user@Server+Desktop) Working with virtual environments 
* Create a conda environment
  * `conda create -n venv-p39-tf2 python=3.9`
  * This will create a python virtual environment in the user home directory. (E.g, `/home/ashis/.conda/`)
* To activate this environment, use `conda activate venv-p39-tf2`
* To deactivate this environment, use `conda deactivate`
* You can list all virtual environments and their absolute paths by: `conda env list` or `conda info --envs`
* To remove this environment, use `conda remove -n venv-p39-tf2 --all`

## (Admin@Server) Preparing NVIDIA graphics card
### (Admin@Server) Setting up NVIDIA graphics driver.
* Check first which Nvidia card(s) is (are) installed in your system running: `$ lspci | grep VGA` . This will list the name(s) of the card.
* Disable (`nouveau`) driver that is loaded by default as a generic graphics driver by running the following commands: 
  * List out every loaded kernel module on the system:
    * `$ sudo lsmod | grep nouveau`
  * Create (if not present) a file: `/etc/modprobe.d/blacklist-nouveau.conf` and add the following lines in the file:

```bash
blacklist nouveau
options nouveau modset=0
```
  * Update the existing `initramfs` image with `$ sudo update-initramfs -u`
  * Reboot the system.
* Install the `nvidia-driver-*` with the command: `$ sudo apt install nvidia-driver-535-server`
  * Please check for the latest beyond `535` (if available)
  * Reboot the system.
* Check if the `nvidia-driver` is loaded by running `$ nvidia-smi`
  * This will list all the graphics card as well as their configurations/statuses and cuda version number. In my case it is currently showing `CUDA version 12.2`.

### (Admin@Server+Desktop) Installing NVIDIA Container toolkit
* Install docker to be able to use nvidia within docker containers:
  * `$sudo apt install docker.io`
  * Verify: `$docker version`
    * In my case the version is `24.0.5`
* Following commands are to be run using `root` user by `sudo -s`
  * `root@gpux4:~# curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | apt-key add -`
  * `root@gpux4:~# curl -s -L https://nvidia.github.io/nvidia-docker/ubuntu22.04/nvidia-docker.list > /etc/apt/sources.list.d/nvidia-docker.list`
* `$ sudo apt update`
* `$ sudo apt install nvidia-container-toolkit`
* `$ sudo systemctl restart docker`
* This is how to use `nvidia-smi` from docker container:
  * `$ sudo docker run --gpus all nvidia/cuda:12.2.0-base-ubuntu22.04 nvidia-smi`
* This is how to connect to the interactive session `cuda-11.5.2`:
  * `$ sudo docker run -it --gpus all nvidia/cuda:11.5.2-base-ubuntu20.04 bash`
  * This will create the interactive session with:

```bash
root@94afe49836a4:/# nvidia-smi
root@94afe49836a4:/# exit

```
* List all docker images pulled so far:
  * `sudo docker images`

### (Admin@Server) Install NVIDIA HPC SDK
* Following commands are to be run using `root` user by `sudo -s`
  * `echo 'deb [trusted=yes] https://developer.download.nvidia.com/hpc-sdk/ubuntu/amd64 /' | tee /etc/apt/sources.list.d/nvhpc.list`
* `$ sudo apt update`
* `$ sudo apt install nvhpc-23-9 environment-modules`
  * Check for updated `nvhpc-xx-x` version number
* Edit file `/etc/environment-modules/modulespath`

```bash
# add to the end
/opt/nvidia/hpc_sdk/modulefiles
```
* `$ source /etc/profile.d/modules.sh`
* `$ module avail`
* `$ module load nvhpc/23.9`
* `$ nvc --version`
  * In my case it shows: `23-9-0`
* Let's print the installed Accelerator Information: `$ nvaccelinfo`

## (Admin@Server+Desktop) Installing CUDA
* Run: `$ sudo apt install nvidia-cuda-toolkit`
* Check if it is installed by checking `nvcc` version: `$ nvcc --version`
  * In my case the version was `12.2.91`

## (Regular user) Setting up CUDNN and LD LIBRARY PATH for Tensorflow2 to work properly
* Make sure `$ nvidia-smi` works.
* Make sure `miniconda3` python is accessible. You can check it by running `$ conda` at the terminal. It must not return error. That is, the `$ echo $PATH` should list `miniconda3` path.
* Run: `$ conda create -n venv-p39-tf2 python=3.9`
  * This will create a conda virtual environment at the user's home directory `~/.conda/`
* Activate the environment by: `$ conda activate venv-p39-tf2`
  *  `$ echo $CONDA_PREFIX` should return the complete path of the this activated virtual environment.
  *  You can also check available conda environments by `$ conda env list`
* Update conda (if needed): `$ conda update conda`
* Install `conda-forge` and `cudatoolkit=11.8.0` by running the following command
  * `$ conda install -c conda-forge cudatoolkit=11.8.0`
* Install `nvidia-cudnn-cu11==8.6.0.163` through `pip`
  * `$ pip install nvidia-cudnn-cu11==8.6.0.163`
* Create directories and scripts for `conda` environment variables to set when environment activates/deactivates:
  * `$ mkdir -p $CONDA_PREFIX/etc/conda/activate.d/`
  * `$ mkdir -p $CONDA_PREFIX/etc/conda/deactivate.d/`
  * `$ touch $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh`
  * `$ chmod +x $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh`
  * `$ touch $CONDA_PREFIX/etc/conda/deactivate.d/env_vars.sh`
  * `$ chmod +x $CONDA_PREFIX/etc/conda/deactivate.d/env_vars.sh`
* `LD_LIBRARY_PATH` needs to be set by:

```bash
echo 'CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn; print(nvidia.cudnn.__file__)"))' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
echo 'export LD_LIBRARY_PATH=$CONDA_PREFIX/lib/:$CUDNN_PATH/lib/:$LD_LIBRARY_PATH' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
```
* Now, deactivate and activate the conda virtual environment. Alternatively, restart the shell session to check if `$ echo $LD_LIBRARY_PATH` works or not.

## (Regular user) Install Tensorflow
  * Again, do the following the `$conda activate venv-p39-tf2` environment.
  * `pip install --upgrade pip`
  * Do not install tensorflow with conda (as of 8/11/2023).
  * `pip install tensorflow==2.13.*`
* Verify installation:
  * In the activated python shell:
  * `import tensorflow as tf`
  * `print(tf.config.list_physical_devices('GPU'))`
    * or, `print(tf.config.list_physical_devices())`
  * This should list a GPU instead of an empty list, if you have a GPU configured above.
  * **Issue 1**: In Ubuntu 22.04, you may encounter an Internal error: `libdevice not found at ./libdevice.10.bc [Op:__some_op]`. In my case, I did not get it luckily!
      * Solution: 
          * Install NVCC (as regular user): 
            * `conda install -c nvidia cuda-nvcc=11.3.58`
          * Configure the XLA cuda directory:

            ```bash
            mkdir -p $CONDA_PREFIX/etc/conda/activate.d
            printf 'export XLA_FLAGS=--xla_gpu_cuda_data_dir=$CONDA_PREFIX/lib/' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
            ```

          * Reactivate conda environment.
          * Copy libdevice file to the required path
            * `mkdir -p $CONDA_PREFIX/lib/nvvm/libdevice`
            * `cp $CONDA_PREFIX/lib/libdevice.10.bc $CONDA_PREFIX/lib/nvvm/libdevice/`
  * **Issue 2**: Jupyter notebook can't use GPU although your python shell can. Basically, something was 'wrong' with jupyter in that it could not read my `LD_LIBRARY_PATH` variable, although you may have already put everything correctly inside `.bashrc`.
    * Solution: 
      * Switch to the command line (terminal). Switch into your virtual environment if you have one, then type in: `jupyter notebook --generate-config`
      * It will tell you the directory in which your jupyter configuration file is stored/overwritten if you already have one. If you want to list it again type: `jupyter --config-dir`
      * At the very top of this file, `jupyter_notebook_config.py`, add in the following code based on your `LD_LIBRARY_PATH` configurations:

```
# Configuration file for notebook.

import os
c = get_config()  #noqa

os.environ['LD_LIBRARY_PATH'] = '/opt/miniconda3/envs/venv-p39-tf2/lib/:/home/opt/miniconda3/envs/venv-p39-tf2/lib/python3.9/site-packages/nvidia/cudnn/lib:$LD_LIBRARY_PATH'
c.Spawner.env.update('LD_LIBRARY_PATH')

```
      * Then restart jupyterhub or jupyter notebook.
      * Make the changes in `jupyter`, not `jupyterhub`. (Each user of `jupyterhub` will have their own `jupyter` process, so make the changes not at the 'hub' level, but the `jupyter notebook` level.


## (Regular user) Configure VSCode
* In VSCode, hit <CTRL><SHIFT><p> to open the command prompt, where type:
  * `python: select interpreter`
      * By now, the conda environments should all the listed. Choose the one you like.

## (Admin@Server+Desktop) Setting up JupyterHub
The combination of JupyterHub and JupyterLab is a great way to make shared computing resources available to a group.

These instructions are a guide for a manual, ‘bare metal’ install of JupyterHub and JupyterLab. This is ideal for running on a single server: build a beast of a machine and share it within your lab, or use a virtual machine from any VPS or cloud provider.

This guide has similar goals to The Littlest JupyterHub setup script. However, instead of bundling all these step for you into one installer, we will perform every step manually. This makes it easy to customize any part (e.g. if you want to run other services on the same system and need to make them work together), as well as giving you full control and understanding of your setup.


### (Admin@Server + Desktop) Part 1: JupyterHub and JupyterLab
#### (Admin@Server + Desktop) Setup the JupyterHub and JupyterLab in a virtual environment

* First we create a virtual environment under `/opt/venv-jupyterhub`. The `/opt` directory is where apps not belonging to the operating system are commonly installed. Both `jupyterlab` and `jupyterhub` will be installed into this virtualenv: `venv-jupyterhub`. Create it with the commands: 
  * `$sudo apt install python3.10-venv`
  * `$ sudo python3 -m venv /opt/venv-jupyterhub/`
* Now we use `pip` to install the required Python packages into the new virtual environment. Be sure to install `wheel` first. Since we are separating the user interface from the computing kernels, we don’t install any Python scientific packages here. The only exception is `ipywidgets` because this is needed to allow connection between interactive tools running in the kernel and the user interface. Note that we use `/opt/venv-jupyterhub/bin/python3 -m pip install` each time - this makes sure that the packages are installed to the correct virtual environment. To accomplish this, perform the install using the following commands: `sudo /opt/jupyterhub/bin/python3 -m pip install wheel jupyterhub jupyterlab ipywidgets`
* JupyterHub also currently defaults to requiring `configurable-http-proxy`, which needs `nodejs` and `npm`. The versions of these available in Ubuntu therefore need to be installed first (they are a bit old but this is ok for our needs): `sudo apt install nodejs npm`
  * Then install configurable-http-proxy: `sudo npm install -g configurable-http-proxy`

#### (Admin@Server + Desktop) Create the configuration for JupyterHub
* Now we start creating configuration files. To keep everything together, we put all the configuration into the folder created for the virtualenv, under `/opt/venv-jupyterhub/etc/`. For each thing needing configuration, we will create a further subfolder and necessary files.
* First create the folder for the JupyterHub configuration and navigate to it:

```
sudo mkdir -p /opt/venv-jupyterhub/etc/jupyterhub/
cd /opt/venv-jupyterhub/etc/jupyterhub/
```

* Then generate the default configuration file: `sudo /opt/venv-jupyterhub/bin/jupyterhub --generate-config`. 
  * This will produce the default configuration file `/opt/venv-jupyterhub/etc/jupyterhub/jupyterhub_config.py`
  * You will need to edit the configuration file to make the JupyterLab interface by the default. Set the following configuration option in your `jupyterhub_config.py` file (Find the line and uncomment if needed):

```
c.Spawner.default_url = '/lab'
```

#### (Admin@Server + Desktop) Setup Systemd service

* We will setup JupyterHub to run as a system service using Systemd (which is responsible for managing all services and servers that run on startup in Ubuntu). We will create a service file in a suitable location in the virtualenv folder and then link it to the system services. 
* First create the folder for the service file: `sudo mkdir -p /opt/venv-jupyterhub/etc/systemd/`
* Then create the following text file using your favorite editor at `/opt/venv-jupyterhub/etc/systemd/jupyterhub.service` by:
  * `$ sudo touch /opt/venv-jupyterhub/etc/systemd/jupyterhub.service`
* Paste the following service unit definition into the file:

```bash
[Unit]
Description=JupyterHub
After=syslog.target network.target

[Service]
User=root
Environment="PATH=/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/opt/venv-jupyterhub/bin"
ExecStart=/opt/venv-jupyterhub/bin/jupyterhub -f /opt/venv-jupyterhub/etc/jupyterhub/jupyterhub_config.py

[Install]
WantedBy=multi-user.target
```

  * This sets up the environment to use the virtual environment we created, tells Systemd how to start jupyterhub using the configuration file we created, specifies that jupyterhub will be started as the root user (needed so that it can start jupyter on behalf of other logged in users), and specifies that jupyterhub should start on boot after the network is enabled.
* Finally, we need to make `systemd` aware of our service file. First we symlink our file into systemd’s directory:

```
sudo ln -s /opt/venv-jupyterhub/etc/systemd/jupyterhub.service /etc/systemd/system/jupyterhub.service
```

* Then tell systemd to reload its configuration files: `sudo systemctl daemon-reload`
* And finally enable the service: `sudo systemctl enable jupyterhub.service`
* The service will start on reboot, but we can start it straight away using: `sudo systemctl start jupyterhub.service`
* and check that it’s running using: `sudo systemctl status jupyterhub.service`
* You should now be already be able to access jupyterhub using <your servers ip>:8000 (assuming you haven’t already set up a firewall or something). However, when you log in the jupyter notebooks will be trying to use the Python virtualenv that was created to install JupyterHub, this is not what we want. So on to part 2.

### (Regular User) Part 2: Setting up Conda environment for Tensorflow 2 (with GPU)

For user `userx`:
1. Create a conda environment:
  * `$ conda create -n venv-p39-tf2-userx python=3.9`
2. Activate the environment:
  * `$ conda activate venv-p39-tf2-userx`
  * Run `$ conda env list` and note the absolute path of the virtual environment. In my case it was: `/home/abx4kb/.conda/envs/venv-p39-tf2-userx`
  * You could also get the path set at the `$CONDA_PREFIX` variable.
3. Install cudnn and cudatoolkit package in the activated environment using the following two command:
  * `conda install -c conda-forge cudatoolkit=11.8.0`
  * `pip install nvidia-cudnn-cu11==8.6.0.163`
4. Install the `ipykernel` package to add the functionality so that jupyter notebook can access the created jupyter kernels:
  * `$ pip install ipykernel`
5. Create a jupyter kernel. Please note the `--user` flag as this kernel is meant to be used and accessed only user `userx`:
  * `$ python -m ipykernel install --user --name=venv-p39-tf2-userx`
  * This will say that it installed kernelspec `venv-p39-tf2-userx` in `/home/userx/.local/share/jupyter/kernels/venv-p39-tf2-userx`
  * Note, this will create a file named `kernel.json` at `/home/userx/.local/share/jupyter/kernels/venv-p39-tf2-userx` directory.
* Edit the `kernel.json` to add the environments variables (e.g., PATH etc): 

```bash
{
 "argv": [
  "/home/userx/.conda/envs/venv-p39-tf2-userx/bin/python",
  "-m",
  "ipykernel_launcher",
  "-f",
  "{connection_file}"
 ],
 "display_name": "venv-p39-tf2-userx",
 "env": {
  "CONDA_PREFIX":"/home/userx/.conda/envs/venv-p39-tf2-userx/",
  "CONDA_DEFAULT_ENV":"venv-p39-tf2-userx",
  "PATH":"/opt/conda/condabin:/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/opt/venv-jupyterhub/bin:/snap/bin",
  "LD_LIBRARY_PATH":"/home/userx/.conda/envs/venv-p39-tf2-userx/lib/:/home/userx/.conda/envs/venv-p310-torch210-abx4kb/lib/python3.10/site-packages/nvidia/cudnn/lib"
 },
 "language": "python",
 "metadata": {
  "debugger": true
 }
}
```
  * The key insight here is the `PATH` and `LD_LIBRARY_PATH` environment variables. Because the `kernel.json` allows you to set your path, you can update binaries to point to the binaries in your `pip` environment. There are likely some other variables specific to pip (like CONDA_PREFIX is for conda), but hopefully this can get you over the line.
6. Upgrade `pip`:
   * `pip install --upgrade pip`
7. Install `tensorflow 2.13`:
   * `pip install tensorflow==2.13.*`
   * You can verify if GPU devices were detected by going in `python` shell in the activated virtual environment in a jupyter notebook the following commands:
     * `import tensorflow as tf`
     * `print(tf.config.list_physical_devices())`
       * You may want to see available `GPU` device(s) to be listed like below:

```bash
[PhysicalDevice(name='/physical_device:CPU:0', device_type='CPU'),
 PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU'),
 PhysicalDevice(name='/physical_device:GPU:1', device_type='GPU'),
 PhysicalDevice(name='/physical_device:GPU:2', device_type='GPU'),
 PhysicalDevice(name='/physical_device:GPU:3', device_type='GPU')]
```
    * You can pass `GPU` or `CPU` as argument to the `list_physical_devices` function to get only the list of devices of that type.

#### (Optional) Jupyter configuration:
* Generate jupyter configuration file by running the command: `$ jupyter notebook --generate-config`
  * `vi /home/userx/.jupyter/jupyter_notebook_config.py`, and at the top add the following:

```
# Configuration file for notebook.

import os
c = get_config()  #noqa

os.environ['YOUR_PATH'] = 'your_specific_path'
c.Spawner.env.update('YOUR_PATH')
```

### (Regular User) Part 3: Setting up Conda environment for PyTorch (with GPU)
1. Create another conda environment  `venv-p310-torch210-userx` :
  * `$ conda create -n venv-p310-torch210-userx python=3.10`
2. Activate the environment: `conda activate venv-p310-torch210-userx`
3. (Optional as pytorch configures the cuda lib paths during setup) Now, follow steps 3--6 listed in section **(Regular User) Part 2: Setting up Conda environment for Tensorflow 2 (with GPU)** above to configure an usable jupyter kernel. Please note the `cuda/lib` paths on the activated conda environment using the following two commands:

```bash
echo 'CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn; print(nvidia.cudnn.__file__)"))' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
echo 'export LD_LIBRARY_PATH=$CONDA_PREFIX/lib/:$CUDNN_PATH/lib/:$LD_LIBRARY_PATH' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
```

4. Visit [https://pytorch.org/get-started/locally/](https://pytorch.org/get-started/locally/) and choose the configuration matrix to find the command to properly install `pytorch-2.1.0 (stable)` (as of 10/19/2023). In my case, my choices were:
   * PyTorch build: Stable (2.1.0)
   * Your OS: Linux
   * Package: Pip
   * Language: Python
   * Compute Platform: CUDA 12.1
   * Run this command: `pip3 install torch torchvision torchaudio`
   * And, I ran this command.
5. Verify install by running the following commands in the python shell:

```python
# Check for GPU
import torch
torch.cuda.is_available()
```

It is expected to return `true` if cuda/GPU is configured correctly.

```python
# Set device type
device = "cuda" if torch.cuda.is_available() else "cpu"
device
```
It sets `device` to `cuda` if cuda/GPU is configured correctly.

```python
# Count number of devices
torch.cuda.device_count()
```

It returns number of cuda/GPU devices configured currently in the environment.


### (Admin@Server + Desktop) Setting up a reverse proxy
The guide so far results in JupyterHub running on port 8000. It is not generally advisable to run open web services in this way - instead, use a reverse proxy running on standard HTTP/HTTPS ports.

Important: Be aware of the security implications especially if you are running a server that is accessible from the open internet i.e. not protected within an institutional intranet or private home/office network. You should set up a firewall and HTTPS encryption, which is outside of the scope of this guide. For HTTPS consider using LetsEncrypt or setting up a self-signed certificate. Firewalls may be set up using ufw or firewalld and combined with fail2ban.

### (Admin@Server + Desktop) Using Nginx

* Nginx is a mature and established web server and reverse proxy and is easy to install using `sudo apt install nginx`. Details on using Nginx as a reverse proxy can be found elsewhere. Here, we will only outline the additional steps needed to setup JupyterHub with Nginx and host it at a given URL e.g. `<your-server-ip-or-url>/jupyter`. This could be useful for example if you are running several services or web pages on the same server.

* To achieve this needs a few tweaks to both the JupyterHub configuration and the Nginx config. First, edit the configuration file `/opt/jupyterhub/etc/jupyterhub/jupyterhub_config.py` and add the line:

```
c.JupyterHub.bind_url = 'http://:8000/jupyter'
```

where `/jupyter` will be the relative URL of the JupyterHub.

* Now Nginx must be configured with a to pass all traffic from `/jupyter` to the the local address `127.0.0.1:8000` Add the following snippet to your nginx configuration file (e.g. `/etc/nginx/sites-available/default`).

```
  location /jupyter/ {
    # NOTE important to also set base url of jupyterhub to /jupyter in its config
    proxy_pass http://127.0.0.1:8000;

    proxy_redirect   off;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;

    # websocket headers
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;

  }
```

* Also add this snippet before the server block:

```
map $http_upgrade $connection_upgrade {
        default upgrade;
        '' close;
    }
```

* Nginx will not run if there are errors in the configuration, check your configuration using: `nginx -t`
* If there are no errors, you can restart the Nginx service for the new configuration to take effect. `sudo systemctl restart nginx.service`

### (Regular user) Getting started using your new JupyterHub
Once you have setup JupyterHub and Nginx proxy as described, you can browse to your JupyterHub IP or URL (e.g. if your server IP address is 123.456.789.1 and you decided to host JupyterHub at the /jupyter URL, browse to 123.456.789.1/jupyter). You will find a login page where you enter your Linux username and password. On login you will be presented with the JupyterLab interface, with the file browser pane showing the contents of your users’ home directory on the server.

## (Admin@Desktop) Alternate GPU setup (mostly deprecated as of 10/18/2023)
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
  * Install cuda-toolkits:
    * `sudo apt install nvidia-cuda-toolkit nvidia-cuda-toolkit-gcc`

## Reference (as of 10/19/2023):
* [https://www.tensorflow.org/install/pip#linux](https://www.tensorflow.org/install/pip#linux)
* [Official documentation: Install JupyterHub and JupyterLab from the ground up](https://jupyterhub.readthedocs.io/en/1.2.1/installation-guide-hard.html)
* Stackoverflow post [link](https://stackoverflow.com/questions/67914989/creating-virtual-environments-for-jupyterhub)
