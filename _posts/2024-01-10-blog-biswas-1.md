---
title: 'Jupyter Notebook to pdf/html file'
date: 2024-01-10
toc: true
permalink: /posts/2024/01/biswas/blog-jupyter-nbconvert/
tags:
  - jupyter
  - notebook
  - nbconvert
  - pdf
---

If you would like to create a pdf/html from your jupyter notebook, you can use the handy `nbconvert` tool of `jupyter`. Here is how you can use it.

### Setup
* Make sure you have `jupyter` and `nbconvert` installed in your activated python virtual environment. You can install `nbconvert` by just this command: `pip install nbconvert`

### Converting a notebook into PDF

`jupyter nbconvert --to pdf yourNotebook.ipynb`
* This generates a PDF via your system's `latex` (or more specifically `xelatex`) command. So, make sure you have these installed in your system otherwise the `nbconvert` will fail.



### Converting a notebook into HTML
`jupyter nbconvert --to html yourNotebook.ipynb`
* This exports a html version of the jupyter notebook.

### Converting a notebook into WebPDF

`jupyter nbconvert --to webpdf yourNotebook.ipynb`
* This generates a PDF by first rendering to HTMMLM, and rendering the HTML chromium headless, and exporting to PDF. The exporter supports the same template as `--to html`. The `webpdf` exporter requires the `playwright` Chromium automation library, which can be installed via `pip install nbconvert[webpdf]` command.







