---
title: 'Markdown (md/txt) file to PDF conversion (in Ubuntu 22.04)'
date: 2023-10-18
toc: true
permalink: /posts/2023/10/biswas/blog-markdown-to-pdf/
tags:
  - ubuntu
  - ubuntu 22.04
  - linux
  - markdown
  - pdf
---

If you are writing in markdown, at times you may want to convert those into PDF file. Here are few steps you need:

* Download few dependencies (if not present in your system):
  * `sudo apt-get install pandoc texlive-latex-base texlive-fonts-recommended texlive-extra-utils texlive-latex-extra`
* Now convert using:
  * `pandoc my-markdown-file.md -o my-markdown-file.pdf`
  * `pandoc my-markdown-file.txt -o my-markdown-file.pdf`