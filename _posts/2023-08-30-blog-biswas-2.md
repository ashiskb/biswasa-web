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
      * Make sure `conda` sets the `$PATH` variables.
  * Close and re-open the current shell.
  * If you don’t want conda `(base)` environment to be activated every time, do the following:
      * `conda config —-set auto_activate_base false`
* `conda update conda`
* Create a conda environment
  * `conda create -n venv-tf2 python=3.9`
  * To activate this environment, use `conda activate venv-tf2`
  * To deactivate this environment, use `conda deactivate`
  * You can list all virtual environments and their absolute paths by: `conda info --envs`
  * To remove this environment, use `conda remove -n venv-tf2 --all`

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
  * Install cuda-toolkits:
    * `sudo apt install nvidia-cuda-toolkit nvidia-cuda-toolkit-gcc`
## Install CUDA and cuDNN with conda and pip**:
  * Do the following with `$conda activate venv-tf2` environment.
  * `conda install -c conda-forge cudatoolkit=11.8.0`
  * `pip install nvidia-cudnn-cu11==8.6.0.163`
  * Now, configure system path and cuDNN paths:
      * Do the following steps to automatically configure so that these get set when you activate the conda environment:
      * `mkdir -p $CONDA_PREFIX/etc/conda/activate.d`
      * `echo 'CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn; print(nvidia.cudnn.__file__)"))' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh`
      * `echo 'export LD_LIBRARY_PATH=$CONDA_PREFIX/lib/:$CUDNN_PATH/lib:\$LD_LIBRARY_PATH' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh`
  * Reopen your shell.
  
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
  * **Issue 2**: Jupyter notebook can't use GPU although your python shell can. Basically, something was 'wrong' with jupyter in that it could not read my `LD_LIBRARY_PATH` variable, although you may have already put everything correctly inside `.bashrc`.
    * Solution: 
      * Switch to the command line (terminal). Switch into your virtual environment if you have one, then type in: `jupyter notebook --generate-config`
      * It will tell you the directory in which your jupyter configuration file is stored/overwritten if you already have one. If you want to list it again type: `jupyter --config-dir`
      * At the very top of this file, `jupyter_notebook_config.py`, add in the following code based on your `LD_LIBRARY_PATH` configurations:

```
# Configuration file for notebook.

import os
c = get_config()  #noqa

os.environ['LD_LIBRARY_PATH'] = '/home/ashiskb/miniconda3/envs/venv-p39-tf213user/lib/:/home/ashiskb/miniconda3/envs/venv-p39-tf213user/lib/python3.9/site-packages/nvidia/cudnn/lib:$LD_LIBRARY_PATH'
c.Spawner.env.update('LD_LIBRARY_PATH')

```
      * Then restart jupyterhub or jupyter notebook.
      * Make the changes in `jupyter`, not `jupyterhub`. (Each user of `jupyterhub` will have their own `jupyter` process, so make the changes not at the 'hub' level, but the `jupyter notebook` level.


## Configure VSCode
* In VSCode, hit <CTRL><SHIFT><p> to open the command prompt, where type:
  * `python: select interpreter`
      * By now, the conda environments should all the listed. Choose the one you like.


## Setting up JupyterHub
The combination of JupyterHub and JupyterLab is a great way to make shared computing resources available to a group.

These instructions are a guide for a manual, ‘bare metal’ install of JupyterHub and JupyterLab. This is ideal for running on a single server: build a beast of a machine and share it within your lab, or use a virtual machine from any VPS or cloud provider.

This guide has similar goals to The Littlest JupyterHub setup script. However, instead of bundling all these step for you into one installer, we will perform every step manually. This makes it easy to customize any part (e.g. if you want to run other services on the same system and need to make them work together), as well as giving you full control and understanding of your setup.


### Part 1: JupyterHub and JupyterLab
#### Setup the JupyterHub and JupyterLab in a virtual environment

* First we create a virtual environment under `/opt/venv-jupyterhub`. The `/opt` directory is where apps not belonging to the operating system are commonly installed. Both `jupyterlab` and `jupyterhub` will be installed into this virtualenv: `venv-jupyterhub`. Create it with the command: `sudo python3 -m venv /opt/venv-jupyterhub/`
* Now we use `pip` to install the required Python packages into the new virtual environment. Be sure to install `wheel` first. Since we are separating the user interface from the computing kernels, we don’t install any Python scientific packages here. The only exception is `ipywidgets` because this is needed to allow connection between interactive tools running in the kernel and the user interface. Note that we use `/opt/venv-jupyterhub/bin/python3 -m pip install` each time - this makes sure that the packages are installed to the correct virtual environment. To accomplish this, perform the install using the following commands:

```
sudo /opt/jupyterhub/bin/python3 -m pip install wheel
sudo /opt/jupyterhub/bin/python3 -m pip install jupyterhub jupyterlab
sudo /opt/jupyterhub/bin/python3 -m pip install ipywidgets
```

* JupyterHub also currently defaults to requiring `configurable-http-proxy`, which needs `nodejs` and `npm`. The versions of these available in Ubuntu therefore need to be installed first (they are a bit old but this is ok for our needs): `sudo apt install nodejs npm`
  * Then install configurable-http-proxy: `sudo npm install -g configurable-http-proxy`

#### Create the configuration for JupyterHub
* Now we start creating configuration files. To keep everything together, we put all the configuration into the folder created for the virtualenv, under `/opt/venv-jupyterhub/etc/`. For each thing needing configuration, we will create a further subfolder and necessary files.
* First create the folder for the JupyterHub configuration and navigate to it:

```
sudo mkdir -p /opt/venv-jupyterhub/etc/jupyterhub/
cd /opt/venv-jupyterhub/etc/jupyterhub/
```

* Then generate the default configuration file: `sudo /opt/jupyterhub/bin/jupyterhub --generate-config`. 
  * This will produce the default configuration file `/opt/venv-jupyterhub/etc/jupyterhub/jupyterhub_config.py`
  * You will need to edit the configuration file to make the JupyterLab interface by the default. Set the following configuration option in your `jupyterhub_config.py` file:

```
c.Spawner.default_url = '/lab'
```

#### Setup Systemd service

* We will setup JupyterHub to run as a system service using Systemd (which is responsible for managing all services and servers that run on startup in Ubuntu). We will create a service file in a suitable location in the virtualenv folder and then link it to the system services. 
* First create the folder for the service file: `sudo mkdir -p /opt/jupyterhub/etc/systemd`
* Then create the following text file using your favorite editor at `/opt/jupyterhub/etc/systemd/jupyterhub2.service`
* Paste the following service unit definition into the file:

```
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
* Finally, we need to make systemd aware of our service file. First we symlink our file into systemd’s directory:

```
sudo ln -s /opt/venv-jupyterhub/etc/systemd/jupyterhub2.service /etc/systemd/system/jupyterhub2.service
```

* Then tell systemd to reload its configuration files: `sudo systemctl daemon-reload`
* And finally enable the service: `sudo systemctl enable jupyterhub2.service`
* The service will start on reboot, but we can start it straight away using: `sudo systemctl start jupyterhub2.service`
* and check that it’s running using: `sudo systemctl status jupyterhub.service`
* You should now be already be able to access jupyterhub using <your servers ip>:8000 (assuming you haven’t already set up a firewall or something). However, when you log in the jupyter notebooks will be trying to use the Python virtualenv that was created to install JupyterHub, this is not what we want. So on to part 2.

### Part 2: Conda environments
#### Install conda for the whole system
* We will use conda to manage Python environments. We will install the officially maintained conda packages for Ubuntu, this means they will get automatic updates with the rest of the system. Setup repo for the official Conda debian packages, instructions are copied from here:
* Install Anacononda public gpg key to trusted store:

```
curl https://repo.anaconda.com/pkgs/misc/gpgkeys/anaconda.asc | gpg --dearmor > conda.gpg
sudo install -o root -g root -m 644 conda.gpg /etc/apt/trusted.gpg.d/
```

* Add Debian repo

```
echo "deb [arch=amd64] https://repo.anaconda.com/pkgs/misc/debrepo/conda stable main" | sudo tee /etc/apt/sources.list.d/conda.list
```

* Install conda

```
sudo apt update
sudo apt install conda
```

  * This will install conda into the folder `/opt/conda/`, with the conda command available at `/opt/conda/bin/conda`.

* Finally, we can make conda more easily available to users by symlinking the conda shell setup script to the profile ‘drop in’ folder so that it gets run on login

```
sudo ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh
```

#### Install a default conda environment for all users

* First create a folder for conda envs (might exist already): `sudo mkdir /opt/conda/envs/`
* Then create a conda environment to your liking within that folder. Here we have called it ‘python’ because it will be the obvious default - call it whatever you like. You can install whatever you like into this environment, but you MUST at least install ipykernel.

```
sudo /opt/conda/bin/conda create --prefix /opt/conda/envs/python python=3.9 ipykernel
```

Once your env is set up as desired, make it visible to Jupyter by installing the kernel spec. There are two options here:

1 ) Install into the JupyterHub virtualenv - this ensures it overrides the default python version. It will only be visible to the JupyterHub installation we have just created. This is useful to avoid conda environments appearing where they are not expected.

```
sudo /opt/conda/envs/python/bin/python -m ipykernel install --prefix=/opt/venv-jupyterhub/ --name 'python' --display-name "Python (default.x)"
```

2 ) Install it system-wide by putting it into /usr/local. It will be visible to any parallel install of JupyterHub or JupyterLab, and will persist even if you later delete or modify the JupyterHub installation. This is useful if the kernels might be used by other services, or if you want to modify the JupyterHub installation independently from the conda environments.

```
sudo /opt/conda/envs/python/bin/python -m ipykernel install --prefix /usr/local/ --name 'python' --display-name "Python (default)"
```

#### Setting up users’ own conda environments
* There is relatively little for the administrator to do here, as users will have to set up their own environments using the shell. On login they should run conda init or /opt/conda/bin/conda. The can then use conda to set up their environment, although they must also install ipykernel. Once done, they can enable their kernel using:
* `conda create -n venv-p39-tf213user python=3.9`
* Get the absolute path of the environment: `conda info --envs`
* `conda activate venv-p39-tf213user`
*  `sudo /home/ashiskb/miniconda3/envs/venv-p39-tf213user/bin/python -m ipykernel install --name "venv-p39-tf213user" --display-name "Python My Env"`
* `conda update -n base -c defaults conda`
* `conda install -c conda-forge cudatoolkit=11.8.0`
* `pip install nvidia-cudnn-cu11==8.6.0.163`
* `mkdir -p $CONDA_PREFIX/etc/conda/activate.d`
* `echo 'CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn; print(nvidia.cudnn.__file__)"))' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh`
* `echo 'export LD_LIBRARY_PATH=$CONDA_PREFIX/lib/:$CUDNN_PATH/lib:\$LD_LIBRARY_PATH' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh`
* `pip install --upgrade pip`
* `pip install tensorflow==2.13.*`
* `jupyter notebook --generate-config`
  * `vi /home/ashiskb/.jupyter/jupyter_notebook_config.py`, and at the top add the following:

```
# Configuration file for notebook.

import os
c = get_config()  #noqa

os.environ['LD_LIBRARY_PATH'] = '/home/ashiskb/miniconda3/envs/venv-p39-tf213user/lib/:/home/ashiskb/miniconda3/envs/v
env-p39-tf213user/lib/python3.9/site-packages/nvidia/cudnn/lib:$LD_LIBRARY_PATH'
c.Spawner.env.update('LD_LIBRARY_PATH')
```

* The above steps will place the kernel spec into their home folder, where Jupyter will look for it on startup.

### Setting up a reverse proxy
The guide so far results in JupyterHub running on port 8000. It is not generally advisable to run open web services in this way - instead, use a reverse proxy running on standard HTTP/HTTPS ports.

Important: Be aware of the security implications especially if you are running a server that is accessible from the open internet i.e. not protected within an institutional intranet or private home/office network. You should set up a firewall and HTTPS encryption, which is outside of the scope of this guide. For HTTPS consider using LetsEncrypt or setting up a self-signed certificate. Firewalls may be set up using ufw or firewalld and combined with fail2ban.

### Using Nginx

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

### Getting started using your new JupyterHub
Once you have setup JupyterHub and Nginx proxy as described, you can browse to your JupyterHub IP or URL (e.g. if your server IP address is 123.456.789.1 and you decided to host JupyterHub at the /jupyter URL, browse to 123.456.789.1/jupyter). You will find a login page where you enter your Linux username and password. On login you will be presented with the JupyterLab interface, with the file browser pane showing the contents of your users’ home directory on the server.

## Reference (08/12/2023):
* [https://www.tensorflow.org/install/pip#linux](https://www.tensorflow.org/install/pip#linux)
* [Official documentation: Install JupyterHub and JupyterLab from the ground up](https://jupyterhub.readthedocs.io/en/1.2.1/installation-guide-hard.html)