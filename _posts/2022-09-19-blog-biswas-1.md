---
title: 'Setting up multiple python versions and virtual environments in Mac, Windows, Ubuntu'
date: 2022-09-19
permalink: /posts/2022/09/biswas/blog-1-python-venv/
tags:
  - python programming
  - venv
  - macos 
  - win10
  - ubuntu
---
Say, someone told you to work on a python project build on `python 3.7.x` and gave you all the project files, and a `requirements.txt` file listing package versions used. In this case, it's a good idea to have the specific python interpreter version and a virtual environment using that specific interpreter setup in your own workstation. Here below are the steps. Please understand the steps may need to be changed based on your current system configurations -- lots of unknown do exist when I was writing this blog post.

# On MacOS Monterey, v 12.5.1
1. Please use package called `pyenv` that will help you switch between different versions of Python (in case you need to run Python 2.x for some reason, and in anticipation of Python 4.0 coming).

    * To install `pyenv` run `brew install pyenv` at the terminal. However, if you do not have `brew`/`homebrew` in your system already, please follow the simple step mentioned [here](https://brew.sh).
    * Assuming the installation of `pyenv` was a success in the previous step. Now use `pyenv` to install specific python version or update your python version. Be sure to run the following command to list what versions of Python you could install in your system `currently` using `pyenv`.
      * `pyenv install --list`
      * If you can find the latest 3.7.x there, go for it. In my case I found version `3.7.13`. Yay!! The command I then executed in the terminal was the following:
        * `pyenv install 3.7.13`
        * I assume the specific version of python install was successful in the previous step. Please note the path where it was installed. In my case int was installed in `/Users/ashiskb/.pyenv/versions/3.7.13`. 
2. Now, you need to create a virtual environment with it so that you can start using the venv from `vscode`.
      * `/Users/ashiskb/.pyenv/versions/3.7.13/bin/python -m venv ~/my-venvs/venv-p3.7.13`
3. Configure `vscode` to locate the path to your venv directories. Hint: From `Code > Preferences > Settings`. Search for `venv` and put the path. In my case I put `Users/ashiskb/my-venvs/`. Save it. Reopen `vscode` and for a notebook, select the `venv-p3.7.13` virtual environment.
4. Create a cell at the top of a jupyter notebook, and type in, execute the following to install all the specific package versions listed in the provided `requirements.txt` in your virtual environment:
  `!pip install -r requirements.txt`

# On Windows 10
1. Follow the steps mentioned in [this article](https://levelup.gitconnected.com/how-to-install-and-manage-multiple-python-versions-on-windows-10-c90098d7ba5a) to install a version of python3.7.x
2. If you are reading this step, please make sure you did not skip any steps. Double check!
3. Now, follow steps `3` and `4` above under the section `On MacOS Monterey, v 12.5.1` to configure and prepare your `vscode` workspace.

# On Ubuntu 22.04 LTS
1. Please update `apt` repo with `$ sudo apt update`
2. Install a software called `software-properties-common`. This software provides an abstraction of the used apt repositories. It allows you to easily manage your distribution and independent software vendor software sources. The command to install it is: `$ sudo apt install software-properties-common`
3. Add a PPA to your apt-source list with: `sudo add-apt-repository ppa:deadsnakes/ppa`
4. Just to be on the safe side update the repo again: `sudo apt update`
5. Install python: `sudo apt install python3.7`
6. Create virtual environment with this interpret: `python3.7 -m venv ~/my-venvs/venv-p3.7.13`
7. Now, follow steps `3` and `4` above under the section `On MacOS Monterey, v 12.5.1` to configure and prepare your `vscode` workspace.