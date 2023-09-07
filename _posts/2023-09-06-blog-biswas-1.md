---
title: 'Install and Configure PHP, MySQL, Apache2 on MacOS Ventura 13.5.1'
date: 2023-09-06
permalink: /posts/2023/09/biswas/blog-apache2-macos/
tags:
  - Apache
  - Apache2
  - PHP
  - MySQL
  - MacOS
  - Ventura
---
If you would want to use your Macbook with MacOS (Ventura 13.5.1) for web development and are looking for installation and configuration instructions all over the Internet, this blog post is an effort to summarize the basic steps you may follow.

## Prerequisites
* Make sure you have `homebrew` installed in your system. It is a software package management system that simplifies the installation of software on Apple's operating system, macOS (and in other Linux distros which I did not know. For linux you may rely on `apt`/`snap` installers, or if you are looking for a GUI, walk with `synaptic`).
  * If `homebrew` is not installed in your system, here is their official page [https://brew.sh](https://brew.sh) where you will find insructions on how to install it. Install [homebrew](https://brew.sh/):

## Setting up Apache/httpd
* Open the terminal and run the following:
  * `brew update`
  * Install the apache2/httpd package: `brew install httpd`
* Make a backup of the default Apache configuration (`/usr/local/etc/httpd/httpd.conf`). This will help you to cross check later what you changed or in case you want to restore the configuration to default.
  * Change `Listen 8080` to `Listen 80` to use the default HTTP port.
  * Uncomment the line `LoadModule rewrite_module lib/httpd/modules/mod_rewrite.so` to enable the URL rewrite module.
  * Update the line `DocumentRoot "/usr/local/var/www"` to the desired document root directory.
* Start the service: `sudo brew services start httpd`
* Stop the service: `sudo brew services stop httpd`
* Open your browser and access [http://localhost](http://localhost). You should see the Apache default page.

## Note on File Permissions
* You may recieve **403 forbidden** when you visit your local site. 
  * You may want to change permissions like this: `chmod 755 directory/ -R`

## Setting up MySQL
* To install MySQL, get MySQL through official page: [https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/)
  * MySQL Community Server download page:
    * Select version: 8.0.34
    * Select Operating System: MacOS
  * Download the `.dmg` file and install.
  * After a successful new MySQL Server installation, complete the configuration steps by choosing the default encryption type for passwords, define the root password, and also enable (or disable) MySQL server at startup.
  * The default MySQL 8.0 password mechanism is caching_sha2_password (Strong), and this step allows you to change it to mysql_native_password (Legacy).
  * Define a password for the `root` user, and also toggle whether MySQL Server should start after the configuration step is complete.
* Now stop/start the MySQL service go to: 'System Preferences' >> 'MySQL' and stop MySQL
* After finishing this up, test MySQL: `mysql -u root -p` and type provide the root password.
* It will ask you write the password you set for mysql before. Enter password and then something like this appear:

```mysql-Login
ashis@Ashiss-MacBook-Pro ~ % mysql -u root -p
Enter password: xxxxxx
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 21
Server version: 8.0.34 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```
* In case you forget the MySQL `root` password down the road, please refer to this page [https://dev.mysql.com/doc/refman/8.0/en/resetting-permissions.html](https://dev.mysql.com/doc/refman/8.0/en/resetting-permissions.html)


## Setting up PHP for Apache
* Install PHP with `brew install php`
* Make a backup of the default Apache configuration (`/usr/local/etc/httpd/httpd.conf`). This will help you to cross check later what you changed or in case you want to restore the configuration to default.
* Now edit the **httpd.conf** with `vi` or any other text editor: `sudo vi /usr/local/etc/httpd/httpd.conf`
* Now add/uncomment the line : `LoadModule php_module /usr/local/opt/php/lib/httpd/modules/libphp.so`
* Replace the line DirectoryIndex index.html with:

```
DirectoryIndex index.php index.html
```
* Add the following lines at the end of the file to enable PHP processing:

```
<FilesMatch \.php$>
    SetHandler application/x-httpd-php
</FilesMatch>
```
* Now Restart apache: `sudo brew services restart httpd`
* To test your PHP installation, create a new file called `info.php` in your document root directory:

```info.php
<?php
  phpinfo();
?>
```
* Save and close the file. Open your web browser and visit [http://localhost/info.php](http://localhost/info.php). You should see the PHP information page, displaying the PHP version and configuration details.
  * Be sure to remove this `info.php` file from production server as it discloses a lot of server configurations and may put your system as risk.

## Setting up PHPMyadmin
* You can use MySQL through command line but this is a good way to administer MySQL. Download phpmyadmin from [https://www.phpmyadmin.net](https://www.phpmyadmin.net).

```phpmyadmin-installation
7z x ~/Downloads/phpMyAdmin-5.2.1-all-languages.zip
mv phpMyAdmin-5.2.1-all-languages/ phpmyadmin
cd phpmyadmin
mv config.sample.inc.php config.inc.php
mv phpMyAdmin-5.2.1-all-languages /your/DocumentRoot/
```
* You can access phpmyadmin via [http://localhost/phpmyadmin](http://localhost/phpmyadmin)

## References (as of 9/6/2023)
* [How to Set Up Apache, MySQL, and PHP on macOS: A Comprehensive Guide](https://tecadmin.net/how-to-setup-apache-mysql-and-php-on-macos/)
* [https://gist.github.com/dreamsparkx/45fba1a25d6b175283a456b19f935ca3](https://gist.github.com/dreamsparkx/45fba1a25d6b175283a456b19f935ca3)