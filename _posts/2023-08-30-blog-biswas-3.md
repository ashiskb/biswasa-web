---
title: 'Install and Configure Julia'
date: 2023-08-30
permalink: /posts/2023/08/biswas/blog-julia/
tags:
  - julia
  - VSCode
---
In this blog post (*in progress*) I will show you few steps to install and configure Julia so that you can start programming in it, and in Microsoft Visual Studio Code (VSCode).


## Julia install
  * Download exact julia installer from https://julialang.org/downloads/ . In my case, I downloaded `Generic Linux on x86: 64-bit (glibc)` for my Ubuntu 22.04 workstation, and `Windows 64-bit (installer)` for my Windows 10 workstation, and `MacOS x86 (Intel chip) 64-bit dmg` for my Macbook.
    * **Configuration in Ubuntu 22.04**
      * Extracted the tar.gz file into `~/Applications/`
      * Added the following line in my ~/.bashrc 
          * export PATH="/home/ashiskb/Applications/julia-1.9.2/bin:$PATH"
    * **Configuration in Windows 10**
      * @TODO -- will add later. *It should be easy and self-explanatory.*
    * **Configuration in MacOS Ventura**
      * @TODO -- will add later. *It should be easy and self-explanatory.*
  * Reopen terminal, and type $`julia`
      * Make sure this opens julia prompt successfully.
  * Julia source file extension ends with `jl`
  * At the julia> prompt type the following to install the “IJulia” package to work with jupyter notebook:
      * >`using Pkg`
      * >`Pkg.add("IJulia")`
      * >`exit()`

## Configuring Julia for VSCode:
  * Install the `julia` extension from the market place, author: `julialang`
  * Create a jupyter notebook at VSCode: `test.ipynb`
  * Select Kernel, and choose “`Julia 1.9.2`” (this is listed here because of the “IJulia” package.

