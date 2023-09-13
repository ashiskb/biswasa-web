---
title: 'Basics of Laravel -- A PHP Framework'
date: 2023-09-08
permalink: /posts/2023/09/biswas/blog-laravel/
tags:
  - laravel
---
Laravel is an open-source PHP web framework that follows the `model-view-controller (MVC)` design pattern. It hosts two major tools: i) composer and ii) artisan. The `Composer` includes all the dependencies and libraries. It allows a user to create a project with respect to the mentioned framework (for example, those used in Laravel installation). Third party libraries can be installed easily with help of composer. All the dependencies are noted in composer.json file which is placed in the source folder. `Artisan` includes a set of commands which assists in building a web application. These commands are incorporated from Symphony framework, resulting in add-on features in Laravel 5.1+.

## Prerequisites
* Assuming, you have PHP and `VSCode` installed in your system.

## Setting up Laravel
### Setting up Composer
* Get and install composer from [https://getcomposer.org/download/](https://getcomposer.org/download/) by following the instructions.

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```
* This installer script will simply check some `php.ini` settings, warn you if they are set incorrectly, and then download the latest `composer.phar` in the current directory. The 4 lines above will, in order:
  * Download the installer to the current directory
  * Verify the installer SHA-384, which you can also cross-check here
  * Run the installer
  * Remove the installer
* Most likely, you want to put the composer.phar into a directory on your `PATH`, so you can simply call `composer` from any directory (Global install), using for example:
  * `mkdir /Users/ashis/_Applications`
  * `mv composer.phar _Applications`
  * `ln -s /Users/ashis/_Applications/composer.phar /Users/ashis/_Applications/composer`
  * `vi ~/.zshrc`
    * `export PATH="/Users/ashis/_Applications/:$PATH"`
    * `source ~/.zshrc`
  * Test if `composer` command works from the command line.

### Setting up Node.js and NPM
* `Node.js` has installer for all OS at this site [https://nodejs.org/en](https://nodejs.org/en). It also installs `npm`, a package manager for javascript.
  * Follow the step by running the installer and make a note on the install path. In my case it was installed at:

```bash
This package has installed:
	•	Node.js v18.17.1 to /usr/local/bin/node
	•	npm v9.6.7 to /usr/local/bin/npm
```
* Check the installation of node.js and npm by typing the following:

```bash
ashis@Ashiss-MacBook-Pro ~ % node -v
v18.17.1
ashis@Ashiss-MacBook-Pro ~ % npm -v
9.6.7
```
* Upgrade `npm` with `sudo npm install -g npm@10.0.0`
* Also run `npm install`

### Now, install and configure Laravel
* Install Laravel with `composer`:

```bash
ashis@Ashiss-MacBook-Pro ~ % composer global require laravel/installer
```
* Add the laravel executable in `PATH`:
  * in MacOS: `export PATH="$HOME/.composer/vendor/bin/:$PATH"`
  * in Ubuntu: `export PATH="$HOME/.config/composer/vendor/bin/:$PATH"`
* Check `laravel` installation with

```bash
ashis@Ashiss-MacBook-Pro ~ % laravel   
Laravel Installer 5.1.0

Usage:
  command [options] [arguments]

Options:
  -h, --help            Display help for the given command. When no command is given display help for the list command
  -q, --quiet           Do not output any message
  -V, --version         Display this application version
      --ansi|--no-ansi  Force (or disable --no-ansi) ANSI output
  -n, --no-interaction  Do not ask any interactive question
  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Available commands:
  completion  Dump the shell completion script
  help        Display help for a command
  list        List commands
  new         Create a new Laravel application
```

## Create new laravel project
* `laravel new mlcompetition`
  * It may ask for 3 options: i) No starter kit, ii) Laravel breeze, iii) Laravel jetstream. Details can be found [here](https://laravel.com/docs/10.x/starter-kits). Please use either of `breeze` or `jetstream` template to integrate the `authentication/login/register` template/options. 

```bash
┌ Would you like to install a starter kit? ────────────────────┐
 │ Laravel Breeze                                               │
 └──────────────────────────────────────────────────────────────┘

 ┌ Which Breeze stack would you like to install? ───────────────┐
 │ Blade                                                        │
 └──────────────────────────────────────────────────────────────┘

 ┌ Would you like dark mode support? ───────────────────────────┐
 │ Yes                                                          │
 └──────────────────────────────────────────────────────────────┘

 ┌ Which testing framework do you prefer? ──────────────────────┐
 │ PHPUnit                                                      │
 └──────────────────────────────────────────────────────────────┘

 ┌ Would you like to initialize a Git repository? ──────────────┐
 │ Yes                                                          │
 └──────────────────────────────────────────────────────────────┘

┌ Which database will your application use? ───────────────────┐
 │ MySQL                                                        │
 └──────────────────────────────────────────────────────────────┘
```
* Open the project folder with `VSCode`.
  * `./composer.json` : contains the project's assets.
  * `./resources/views/welcome.blade.php` is the default welcome page. `blade` is a rendering template.
  * `./resources/views/dashboard.blade.php` is a view file that gets loaded when a user successfully logs in.
* Type `php artisan` to see all different options/commands. Get familiar with all the available commands. For instance,
  * Serve the project into built-in web server: `php artisan serve`

### Front-end
* In the project directory run: `npm run build`. Explanations for this command are follow:
  * `npm`: One could say that it is a command native to the system, for calling Node Package Manager program. 
  * `run`: It is a command native to npm. Keep in mind this is an aliases to the original command `run-script`.
  * `dev`/`build` and `prod`: They're user defined.
      * `dev`/`build`: Used for running the specific commands for serving the project, to any server, to live development. In the case of a web page, you'll see your web page in the browser, and any change you make to the HTML code, for example, will be reflected immediately in the page you see in your browser.
      * `prod`: Compiles all the necessary files for production. Final product. In the case of a web page, for example, the HTML, CSS, and JS files you'll handle to the client. The result of running this command, it is expected to be one single folder with all the afore mentioned content.
  * It creates the following files in `./public` directory:

```bash
public/build/manifest.json             0.26 kB │ gzip:  0.13 kB
public/build/assets/app-e3a33e56.css  34.34 kB │ gzip:  6.26 kB
public/build/assets/app-6e0eadfb.js   71.31 kB │ gzip: 26.54 kB
```

    * You will not be directly editing any of these files, instead edit the `./resources/css/app.css` and `./resources/js/app.js` files.

## References (as of 9/6/2023)
* 