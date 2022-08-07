# Academic website of Dr. B


This is repository for [academic site](https://cse.ucdenver.edu/~biswasa/) of Dr. Ashis Kumer Biswas. I used [Jekyll](https://jekyllrb.com) and [academic website template](https://github.com/academicpages/academicpages.github.io) to design and build the pages. I welcome everyone to contribute to the site. Feel free to fork and pull-request!


## Run the page locally

To run locally, follow instruction [here](https://jekyllrb.com) to install Jekyll then run `jekyll serve` to see in `localhost:4000`. Here is a brief install guidelines.

### On MacOS
You must not use the Ruby that comes with the system. Please follow the guidelines [here](https://jekyllrb.com/docs/installation/macos/) to install Ruby using `brew`. Also, make sure your `$PATH` environment variable is set accordingly (in `zsh`) so that newly installed `Ruby` can be recognized. Here are sample `~/.zshrc` entries:

```zsh
if [ -d "/usr/local/opt/ruby/bin" ]; then
  export PATH=/usr/local/opt/ruby/bin:$PATH
  export PATH=`gem environment gemdir`/bin:$PATH
fi

export LDFLAGS="-L/usr/local/opt/ruby/lib"
export CPPFLAGS="-I/usr/local/opt/ruby/include"
```

Also, make sure to run `ruby -v` command to check if the version matches with your installation. Once installed the `ruby` correctly, run the followng to install `jekyll`:

```zsh
gem install jekyll bundler
```

When the jekyll is installed successfully, navigate to the project folder (assuming you have jekyll project created already) and run the following to build/serve:

### Build a Jekyll project
Before you build, make sure to install the following two gems: `github-pages`, `hawkins`. And, remove all system gems. Finally, to fix `webrick` related dependencies, `bundle add webrick`:
```bash
gem install github-pages
gem install hawkins
bundle clean --force
bundle add webrick
```

Since Jekyll is a static site generator, it has to build the site before we can view it. Run either of the following commands to build your site:
* `jekyll build` - Builds the site and outputs a static site to a directory called `_site`.
* `jekyll serve` - Does jekyll build and runs it on a local web server at [http://localhost:4000](http://localhost:4000), rebuilding the site any time you make a change.

## Update the homepage

It's easy. Just update the `_pages/about.md`. It's all markdown. Please don't remove the header section in the file.

## A piece of advice on the `permalink`
Please append a trailing `/` forward-slash after each permalink to encourage Jekyll to create a directory rather than referring to files. Trust me, it helps.

## Another piece of advice on `_site/`
After the `jekyll build`, please investigate the structure of the files, and folders, and how all the pieces are stitch together to work as the whole website. And, please do remember, the pages under `_site/` are all static, meaning the entire website can very well be understood by a kid!
## Add publications
It is better if I list the steps so to understand the order of activities around it:
1. If you want to modify the schema of the `publications` page, you may need to update the `_pages/publications.md` file. For instance, having a brief introduction, a journal section, a conference section, you may want to look at the current configurations placed in the page. To be creative, you may also want to play with the `_includes/archive-single-journal.html` and `_includes/archive-single-conference.html` files to understand how the schema is formatted.
2. If you are happy with the publication page schema, just add 2 files for each publication entry that you want to appear on the publications page: one `*.md` file in the `_publications/` directory and one `*.bib` file in the `files/bib_files/` directory (optional).

### About the `_publications/` directory
Add to `_publications/` directory an `*.md` file, say `_publications/2023-01-31-ABCD-biswas.md`, denoting that you are talking about a publication from 31st January, 2023, at venue/publisher `ABCD`, and the first author's last name `biswas`. This is the convention used to store all publication data in the `_publications/` directory. In case, you do not know the specific date, put `2023-01-01`, or a rather arbitrary date. Also, the format is important as the date is used to sort (in reverse order) to appear on the page. Please don't forget the `dashes -`, that are not `underscores`.

The content of such an `*.md` file needs to have at least a header section (currently) with the following:

```
---
title: "title of the paper"
collection: publications
pub_type: conference
permalink: /publication/2022-BCB-pastorino
date: 2022-06-23
venue: 'venue/publisher'
paperurl: 'the_url'
link: 'doi url'
code: 'link to your code'
github: 'link to the github repo'
citation: 'APA citation'
bib_file: '/files/bib_files/2022-BCB-pastorino.bib'
---

```
Please note that, currently the `publications.md` is designed in a way (according to the schema) so that the value of the `citation` field is used to populate the publication page, not the `title`, or `date`, or `venue`. These 3 fields might be used in future versions! 

The `paperurl`, `link`, `bib_file` are optional. But, please try to be consistent about the `bib_file`. Grab it from an easier resource like [Google Scholar](https://scholar.google.com) and put it in `files/bib_files/` directory, and mention it here.  `paperurl` is supposed to point at the publication webpage, and `link` to have the `DOI` link that may eventually point to the `paperurl`, but `DOI` links are a bit more reliable than the `paperurl`. Also, you can share the link to your source codes in the `code` field. Additionally, if you would want to share the `github` repository, if different from the link in the `code`, share it in the `github` field. All these links will appear below the `citation` entry with their own little icons: `fa-link`, `fa-file-pdf`, `fa-code`, `fa-github`, `fa-download` from the [Font Awesome library](https://aksakalli.github.io/jekyll-doc-theme/docs/font-awesome/).

Currently, only the header information is used. In the future version, there is a possibility to read and use the body section of the `*.md` file in the `_publicatinos` directory.
### About the `files/bib_files/` directory
Please have a physical `*.bib` file in this directory for each entry of publication that you put in the `_publications` directory. It's also a good idea to have a file naming convention, although not as strict as in the `_publications/` as you would be using the path information in the corresponding `*.md` file. Currently, I used `2023-ABCD-biswas.bib` style formatting. The year goes first, then the venue/publisher, and finally the lastname. You may use any tool to obtain the `bib` files. 

## Update the Navigation bar
Add/subtract/revise items on the navigation bar by simply updating the `_data/navigation.yml` file.

## Update about Teaching
You taught a course, or planning to teach a course in the future, this is the section for you. You need to add one file (`*.md`) for each entry in the `_teaching/` directory. Entries will be listed in reverse chronological order. Please adhere to the file naming convention as in `2017-fall-OOP-2312.md`, where `OOP` is the acronym for the course `Objective Oriented Programming`, annd `2312` might be the course number for you to identify easily later. Make sure it has the following header section, and the rest of it is description of the course in markdown -- use your imagination what you would want to tell your audience about the course. That might even be your course-website. Trust me, it's very possible. Just update just a single `*.md` file.

Make sure to assign `teaching` to the `collection` key in the header. If you put a future `date` in the `date` key, the course will be listed in the teaching page, but the content won't be accessible. This might be a desirable feature to a few.

```
---
title: "CSCI-2312: Object Oriented Programming"
collection: teaching
type: "Undergraduate course"
permalink: /teaching/2017-fall-OOP-2312
venue: "PLAZA-112"
date: 2017-08-21
location: "University of Colorado Denver"
---
```
Like other `pages`, the teaching page schema could also be fine-tuned as you desire. Everything you need is in the `_pages/teaching.html`, and hard-core configurations can be found in the `_includes/archive-single.html` file.

## Adding a Talk
Just add a talk page (`*.md` file) in the `_talks/` directory. The naming convention of the file was used for consistency: `yyyy-mm-dd-talk-XXX.md`, where `yyyy`, `mm`, `dd` are year, month and day of the talk. `XXX` is the acronym/representation of the venue of the talk. 

The header section of the talk file has couple of important fields used to populate the talks page:

```
---
title: "Data Science Competitions: A know-how to participate"
collection: talks
type: "Talk"
permalink: /talks/2021-08-27-talk-DSSymposium
venue: "CU Denver, Data Science Symposium"
date: 2021-08-27
location: "Denver, Colorado, USA."
---
```
Please make sure the `collection: talks` are set. Following the header section, it is all markdown: write whatever you want to let your audience read about the talk.


## How to deploy in your own web-server
* Update the `url` field of the `_config.dev.yml` to appropriate setting.
* Do not build with `jekyll serve`. Only use `jekyll build`. And upload the content of the `_site/` directory.

## How to add posts


- **Directly edit on Github**, you can simply go to `_posts` and click `New file` then put some markdown file e.g. `2022-08-02-post-name.md` and start writing blog post. Github also allows you to preview it so it's nice for people who don't want to clone the repo. 

- **Clone the repository**, kind of the same as directly add post on Github. You just have to clone the repository. Then add new post file, commit and push to the repo.

The changes will take approximately half a minute to render. You can see the new posts or changes on [ML Lab page](http://ml.cse.ucdenver.edu/)!


## Add yourself

You can add yourself to the page in `_people` folder just create file name `<firstname>_<lastname>.md` in the folder. We require few line of header before you start writing your own page. See the following for the header

```
---
name: Ashis Kumer Biswas 
position: pi
avatar: ashis_biswas1.jpg
twitter: mlcse_cudenver
---
```

If you don't have information, just leave it blank. The avatar will bring photo from `images/people` folder and display it on people page. 
For lab position, you can choose position from 8 classes including `pi`, `msstudent`, `phdstudent`, `postdoc`,`alumni`, `ugradstudent`, `visiting`, annd `others` (i.e, Principal Investigator, MS student, PhD student, Post Doc, Alumni, Undergraduate Student, Visting Scientist, and Others respectively). Position will put you into section that you choose.

## Add new publications

All publications from the lab are located in `research.md`. Please upload information about your new publication in the `_research/` directory, in the format `yyyy_publisher_lastname.md`. Each of the `*.md` file will at least contain a header as below:

```
---
title: TexAnASD - Text Analytics for ASD Risk Gene Predictions 
category: reliability
photo: texanasd.png
year: 2019
publisher: IEEE BIBM
---
```

Then, you write detail information of this publication, including abstract, codes, collaborators, where published, etc. Please note this particular commit only support 6 research categories in the `category` field when you load the `research.md` page: `fairness`,`inclusiveness`, `reliability`, `explainability`, `privacy`, `security`, `accountability`. If you want to change these, you need to work in the `research.md` file.


## Acknowledgements

Thanks to Prof. Konrad Kording for the [Jekyll site template](http://kordinglab.com).

## Frequently Asked Questions
* **Q** How can I add something like `This page was last modified at xxx` at the footer of all Jekyll pages?
* **A**: 
    * *Step 1*: Please make sure to install the plugin with `gem` before listing the plugin below at the command line at the current working directory: `gem install jekyll-last-modified-at`
    * *Step 2*: List the plugin in the `Gemfile` in the following group:
  
    ```bash
    group :jekyll_plugins do
      gem "jekyll-last-modified-at"
    end
    ```
    * *Step 3*: Also, add the following configurations in the `_config.yml` file:
    ```bash
    plugins:
      - jekyll-last-modified-at

    # Enable safe mode
    safe: true

    # Whitelist plugins under safe mode.
    whitelist:
      - jekyll-last-modified-at
    ```
    * *Step 3*: Add `_include/footer.html` file designing how you want your footer to look like:
    ```html
        <footer class="site-footer h-card" style="position:fixed;padding:10px 10px 0px 10px;bottom:0;width:100%;height:40px;">
          <data class="u-url" href="{{ "/" | relative_url }}"></data>
          <div class="wrapper">
            <div class="footer-col-wrapper">
              <div class="footer-col">
              {%- if site.author %}
                <ul class="contact-list">
                  {% if site.author.name -%}
                    <li class="p-name">{{ site.author.name | escape }}</li>
                  {% endif -%}
                  {% if site.author.email -%}
                    <li><a class="u-email" href="mailto:{{ site.author.email }}">{{ site.author.email }}</a></li>
                  {%- endif %}
                </ul>
              {%- endif %}
              </div>
              <hr style="width:10%;margin:auto;margin-left:0;">
              <div class="footer-col">
                  <p style="text-align:left;font-size:75%">This page was last modified at {{ page.last_modified_at | date: '%-d %B %Y %T%:z' | escape }}</p>
              </div>
            </div>
          </div>
        </footer>
    ```
    * *Step 4*: Modify the `_layouts/default.html` file to include the `_includes/footer.html` file so that it gets included in all Jekyll pages:
  
    ```html
        ...
        ...
        ...
        {%- include footer.html -%}
      </body>
    </html>
    ```
    * *Step 5*: Done. The plugin `jekyll-last-modified-at` determines a page's last modified date by checking the last Git `commit` date of the source files. In the event Git is not available, the file's `mtime` is used.
* **Q** How can I add a new `collection` so that Jekyll would scan through all its contents and populate a page similar to how `publications`, `talks` pages work?
  * **A** These are typical steps. Be sure to check how I added the `funding` collection in the repo.
    * Step 1: add the collection entry in the `_config.yml` file. In that file you will find a section something like below. Look how I added the `funding` collection there. It tells Jekyll to look for a `_funding/` directory and the files within.
    * Step 2: add a page in the `_pages/` directory for the particular type of collection. Please look how I added the `_pages/funding.md` file. It pretty much is a decorator of the page that you would like to see, including a schema file to use.
    * Step 3: The detail schema that you are including in the `_pages/funding.md` file, needs to be created in the `_includes/archive-single-funding.html`. I borrowed a lot of the decorations from other schema files.
    * Step 4: now, add one `*.md` file for each type of the new `collection` that you desire.
* **Q** My `git-remote-https` process freezes in MacOSX during syncing. What should I do?
  * **A** Try `git config http.postBuffer 16384`. Then, manually push to remote.
* **Q** I am having the following error: `You have already activated i18n 1.12.0, but your Gemfile requires i18n 0.9.5. Prepending bundle exec to your command may solve this. (Gem::LoadError)`, when I do `jekyll build`. What shall I do?
  * **A**: Here are the steps. Please note, the following action may impose side-effects to other projects using gems of different versions. You may need to repeat the same steps in those projects. I hope there is a better way to tackle this. But, for the sake of this particular commit, let's call it a day!
  * Step 1: Clean previously installed gems with `bundle clean --force`
  * Step 2: Run `bundle install` to install ruby dependencies. If you get errors, delete Gemfile.lock and try again.
  * Step 3: Run `bundle exec jekyll liveserve` to generate the HTML and serve it from `localhost:4000` the local server will automatically rebuild and refresh the pages on change.
