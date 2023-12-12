---
title: 'Acccessing Windows 10 Shared folders (samba shares) from Ubuntu 22.04'
date: 2023-12-12
toc: true
permalink: /posts/2023/12/biswas/blog-windows-samba-folders-from-ubuntu/
tags:
  - linux
  - ubuntu
  - ubutun22.04
  - windows
  - windows10
  - samba
  - shared-folder
---

Say, you have some files/folder, say `D:\my_box_folder\` you would want to access from Ubuntu (terminal). Here are few steps you can follow:

## Step 1: Share the folder in Windows 10
* Right click on `D:\my_box_folder\`, click `Properties`. Then, on the `Sharing` tab, click `Share` button.

## Step 2: (Admin only) Access the folder from Ubuntu 22.04 terminal
* Make sure you have the `cifs-utils` package installed. Or, install it by `sudo apt install cifs-utils`.
  * Please note: CIFS stands for Common Internet File System. Also note, CIFS is now obsolete. It's replaced with SAMBA.
* Mount the shared folder into `/mnt/cifs-my-box-folder`. If the folder is not there, create it.
  * `sudo mount -t cifs //10.11.12.13/my_box_folder/ /mnt/cifs-my_box_folder -o username=XXX`
    * Here, replace `XXX` with Windows 10  username. Also, change the IP of the Windows 10 system.
    * It will definitely you ask you for the password for the user `XXX`
* That's it. If successful, you can get access to the `D:\my_box_folder` from your ubuntu system..


## References
* How to mount a SMB share in Ubuntu by Howard Young [ Link ](https://support.zadarastorage.com/hc/en-us/articles/213024986-How-to-Mount-a-SMB-Share-in-Ubuntu)