---
title: 'Changing the Box drive sync directory (in Windows 10)'
date: 2023-12-12
toc: true
permalink: /posts/2023/12/biswas/blog-box-drive-path-update/
tags:
  - windows
  - windows10
  - box
  - cache
---

As of today, [Box drive](https://www.box.com) only provides support for Windows and Mac operating systems. Yet, some of its users (including me) may be experiencing issue in Windows 10 to not being able to change the directory where the files will be synced to. That means, the default box directory, which is the boot drive could not be changed through the installed Box application.

You may wonder why would you want to change the box sync directory location. Okay, that's a fair question: consider my situation. I have a very limited hard disk budget for a Windows10 operating system's boot partition, but I have an extensive storage that I want all kinds of data to be stored. Did you get my issue using Box?

Here is a temporary fix (hoping someday Box will add options in their application):
1. Exit the `Box` application from the system tray.
2. Check if where the box default cache path is. Most likely it is sitting in your `C:\Users\XXX\AppData\Local\Box\Box\cache`, where replace `XXX` with your username in your system.
3. You can take a backup of this directory for now, or want to do fresh start of the sync. Simply delete the `cache` directory.
4. Assuming, you would want `D:\my_box_directory\cache` to be your new sync path. You need to create this path first.
5. Create a directory synbolic link (in Windows it's called junction) with this command: `mklink /J "C:\Users\XXX\AppData\Local\Box\Box\cache" "D:\my_box_directory\cache"`
6. Start the box application.

## References
* Box drive change location of cache folder (Windows 10) by Dimitris Moutsatsos at Medium [ Link ](https://ugh82.medium.com/box-drive-change-location-of-cache-folder-windows-10-16c9e81b6193)