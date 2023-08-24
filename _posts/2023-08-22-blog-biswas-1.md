---
title: 'LaTeX template for CU Denver CSE Graduate project report or thesis or dissertation'
date: 2023-08-22
permalink: /posts/2023/08/biswas/blog-latex-template/
tags:
  - latex
  - template
  - overleaf
  - project report
  - doctoral dissertation
  - thesis
---
I have created this template to help graduate students in Computer Science @ University of Colorado Denver students to prepare their project report / thesis / proposal / dissertation and become successful. Please note, this template is a good start, but there is no guarantee that using this template will produce a document passing the graduate school format review. Please consult with your advisor and share the PDF with her/him to confirm the template is following the committee prescribed template.


* **Here is the link to the Overleaf Template** : [**https://www.overleaf.com/latex/templates/cu-denver-cse-graduate-project-report-or-thesis-template/pnnqwqnznnsx**](https://www.overleaf.com/latex/templates/cu-denver-cse-graduate-project-report-or-thesis-template/pnnqwqnznnsx)

* **Directory Structure of the template**: 

  ```
  .
  ├── README.md
  ├── abstract.tex
  ├── acknowledgements.tex
  ├── appendices.tex
  ├── approvalpage.tex
  ├── chapter01.tex
  ├── chapter02.tex
  ├── chapter03.tex
  ├── chapter04.tex
  ├── chapter05.tex
  ├── copyrightpage.tex
  ├── cse-ucdenver-thesis-report.cls
  ├── dedicationpage.tex
  ├── figures
  │   └── graph1.png
  ├── header.tex
  ├── main.tex
  ├── metainfo.tex
  ├── references.tex
  └── yourLibrary.bib
  ```
  Let's discuss about the important files to begin with:
  * `cse-ucdenver-thesis-report.cls` -- This is the thesis report class definition. It was written to comply with the graduate committee report template. But, any format change that you desire, this is the file you will be looking for.
  * `main.tex` -- Document entry point. This is the LaTeX root document that you will compile. I tried to make it concise and self explanatory.
    * It uses the `cse-ucdenver-thesis-report.cls` to define format of the report.
    * It then inputs the `header.tex` file which contains the `usepackage` statements and command definitions. It is a good idea to put your own `usepackage` statements in `header.tex`. You may want to check if the package is already imported, or may impose conflicts with other imported packages. Please try not to break the project by removing listed `usepackage` statement(s).
    * Then, the `metainfo.tex` is input. This is an important file for you where you define title of the report, your name, education, report type, degree name, submission date, advisor name, committee info.
    * Then, it inputs `abstract.tex` where you write your abstract after the `\ucdabstract` command. Please remove any text written in red color.
    * Then, it inputs `approvalpage.tex`. Please check with committee if it is desired. If not, comment the `\input` line.
    * Then, it inputs `copyrightpage.tex`. This page must be included for MS thesis and doctoral disserations.
    *  It then inputs `acknowledgements.tex`. Although I did not find this page in the original graduate committee provided template, but I highly recommend to have one. Please read carefully what I wrote in red there, and remove it and replace with your own text of gratitude.
    *  Then, it inputs `dedicationpage.tex`. This is completely optional, and I could not find it in the original template. Please consult with graduate committee if you really want to have one.
    *  Then, within the `\begin{document}...\end{document}` block, you will have atleast the following chapters:
       *  Chapter 1: Introduction
       *  Chapter 2: Literature Review
       *  Chapter 3: Methods
       *  Chapter 4: Results and Discussions
       *  Chapter 5: Conclusions
       *  References
       *  You may also include appendix (or appendices), but are optional.
       *  Finally, according to the original template, a mandatory blank page as a last page of your report.
  * `references.tex` refers to a bibliography (bibtex) entries you add into `yourLibrary.bib` file. This particular template utilizes `IEEEtran` bibliography style, as this complies with the graduate committee provided template.
    * Whenever you have a `bib` entry of an article, paste into `yourLibrary.bib` file so that you can `\cite` it from anywhere in your report and have it listed in the `References` page.

## Some Suggestions

* Get into the habit of using [Google Scholar at https://scholar.google.com](https://scholar.google.com) to pull the `bib` entries of articles you read from scholar and put into `yourLibrary.bib` so that you can cite whenever you want during the development of the document.
* Bookmark [LaTeX wikibook at https://en.wikibooks.org/wiki/LaTeX](https://en.wikibooks.org/wiki/LaTeX) and use it as a reference for all your LaTeX needs to document processing. It's a comprehensive LaTeX document processing resource.


## Acknowledgements
  * The initial version of this template was based on the work by [Sarah Kreidler and Peter DeWitt](https://github.com/dewittpe/ucd-thesis-template.git) back in 2018! Thank you two for the attempt!