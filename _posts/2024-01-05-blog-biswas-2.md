---
title: 'Github commands at the command line: a typical workflow with github'
date: 2024-01-05
toc: true
permalink: /posts/2024/01/biswas/blog-github-commands/
tags:
  - linux
  - ubuntu
  - ubutun22.04
  - github
---

Make sure to authenticate your github pass token at the command line. You may find my this blog post useful: [Github command line with new authentication](/posts/2024/01/biswas/blog-github-command-line-with-new-authentication/).

## Clone a repository
* In your local computer, change current working directory to the directory where you want to clone a github repository. Say, `cd _myGITs/`
* `git clone https://github.com/xxx/my-repo.git`
  * The `git clone` command copies your repository from the Github into your local directory.
* `cd my-repo`
  * And, inspect `ls -a`, and you will find `.git`, `.gitignore` files besides the repository contents.

## Make changes to your repository
* Anything you'd like with editors you have in your local system. You can add/edit/remove files as you go.

## Track and add changes to the local git repo
* `git status`
  * This will display a list of modifications made.
*  `git add README.md a_file.txt`
  * This will add one or more files which are listed changes into the `staging area`
  * You may use `git add --all`, but it is strongly discouraged as you may accidentally add things like credential files. If you really want to do it regardless, move sensitive data out of the repository.
* `git commit -m "a message meaningful enough for the people you work with`
  * It takes everything from the staging area and makes a permanent snapshot of the current state of the repository that is associated with a unique id.
  * First attemt of `git commit` command may ask you to set the `user.email` and `user.name` values:
    * `git config --global user.email "myemail@domain.com"`
    * `git config --global user.email "my name is xxx"`

## Push changes to Github (i.e., push to remote)
* So far we have only modified our local (revised) copy of the repository we were working in.
* To add the changes to your remote git repo, you need to `git push` them to Github.
* You can push your changes to Github with `git push`
  * You will then be prompted for Github authentication (if not set already with `gh auth login`, or the stored access token not having correct privileges to make those changes).

## Undoing unstaged changes (i.e., prior to calling `git add`)
* If a file has been changed, but the changes have not yet been staged with `git add`, then the changes can be undone using `git checkout -- myFile.txt`
  * This will discard the changes made on the `myFile.txt`. The command `git checkout --` will revert back to the last committed version.
  * Check the status with `git status` to see if the undoing was successful.

## Undoing staged changes (i.e., after changes are added with `git add`)
* Once you add a set of changes to git using `git add`, the file is then staged.
* If a file has been changed and then staged via `git add`, then use `git reset` to pull the most recently committed version of the file and undo the changes that you've made.
  * `git reset HEAD myFile.txt`
    * This command unstages our changes. The `HEAD` refers to the most recently committed version of the file `myFile.txt`. You can also revert to an older version with `HEAD-1`, `HEAD-2`, etc.
* When you use `git reset`, your changes still exist in the file, but the file has been `unstaged`, that is the changes are not added to git, yet.
  * You can confirm this with `git status` suggesting you to use `git checkout` to discard the changes in working directory.
    * `git checkout -- myFile.txt`

## Undoing a commit
* If you have modified, added and committed changes to a file, and want to undo the changes, then you can use `git reset HEAD~` to undo your commit.
  * Confirm the status with `git status` . You'd notice that your file is no longer being tracked.
  * If you inspect the output of `git log`, you will notice that your previous commit is no longer part of the repo history.
  * Similar to previous example, when you use `git reset` the modifications will be unstaged. You need to `git checkout --` to discard the changes.

## Ignore sentitive files
* If you do have sensitive files in a repo directory that you never want to track with git, you can add those file names to a file called `.gitignore`.

## References
* [Earth Data Analytics Online Certificate -- Lesson 1. Version control intro and setup Intro version control git Workshop](https://www.earthdatascience.org/workshops/intro-version-control-git/)
