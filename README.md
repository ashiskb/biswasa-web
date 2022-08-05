# Academic website of Dr. B


This is repository for [academic site](http://cse.ucdenver.edu/~biswasa/) of Dr. Ashis Kumer Biswas. I used [Jekyll](https://jekyllrb.com) and [academic website template](https://github.com/academicpages/academicpages.github.io) to design and build the pages. I welcome everyone to contribute to the site. Feel free to fork and pull-request!


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

## Add posts

It's very easy to add post. All the posts are located in `_posts` folder. It arrangement is based on date. Each post can be written in markdown format. You just have to state headers before writing: `title`, `description` and `categories`. `description` will be shown when you share on social media like Facebook or twitter. See the following headers:

```
---
title: Summer School in Computational Sensory-Motor Neuroscience (CoSMo)
description: all links to CoSMo summer school in computational neuroscience materials
categories: scientists
---
```

We have 4 categories: `scientists`, `students`, `discussion`, `blog` you can choose and this will be rendered to different location.


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
* How can I add something like `This page was last modified at xxx` at the footer of all Jekyll pages?
  * **Fix**: 
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
 