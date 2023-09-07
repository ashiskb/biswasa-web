---
title: 'Install and Configure PHP, MySQL, Apache2 on Ubuntu 22.04 LTS'
date: 2023-09-06
permalink: /posts/2023/09/biswas/blog-apache2-ubuntu/
tags:
  - Apache
  - Apache2
  - PHP
  - MySQL
  - Ubuntu
  - 22.04
---
If you would want to use your Ubuntu 22.04 LTS system for web development and are looking for installation and configuration instructions all over the Internet, this blog post is an effort to summarize the basic steps you may follow.

## Setting up Apache2
* Open the terminal and run the following:
  * `sudo apt update`
  * Install the apache2/httpd package: `sudo apt install apache2`
* Once the installation is finished, you’ll need to adjust your firewall settings to allow HTTP traffic. Ubuntu’s default firewall configuration tool is called `Uncomplicated Firewall (UFW)`. It has different application profiles that you can readily use. To list all currently available UFW application profiles, execute this command: `sudo ufw app list` . This will show something like this:

```bash
Available applications:
  Apache
  Apache Full
  Apache Secure
  CUPS
  OpenSSH
```
  * Here are the meanings of the Apache apps:
    * **Apache**: This profile opens only port 80 (normal, unencrypted web traffic).
    * **Apache Full**: This profile opens both port 80 (normal, unencrypted web traffic) and port 443 (TLS/SSL encrypted traffic).
    * **Apache Secure**: This profile opens only port 443 (TLS/SSL encrypted traffic).
  * To only allow traffic on port 80, use the Apache profile: `sudo ufw allow in "Apache"`
  * Verify the change: `sudo ufw status`
* Open your browser and access [http://localhost](http://localhost) or [http://your_server_ip](http://your_server_ip). You should see the Apache default page.

## Note on File Permissions
* You may recieve **403 forbidden** when you visit your local site. 
  * You may want to change permissions like this: `chmod 755 directory/ -R`

## Setting up MySQL
* `sudo apt install mysql-server`
* When the installation is finished, it’s recommended that you run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system.
  * Run: `sudo mysql_secure_installation`
* Check the setup from the terminal: `mysql -u root -p`
* In case you forget the MySQL `root` password down the road, do the following:
  * Stop the mysql servide: `sudo systemctl stop mysql.service`
  * Temporarily add the following line at the end of the mysql config file, `/etc/mysql/my.cnf` to allow temporary "passwordless" authentication to the mysql service.

```bash
[mysqld]
skip-grant-tables
```
  * Then, start the service: `sudo systemctl start mysql.service`
  * Now, root login will work with `mysql -u root`
    * At the mysql prompt do the following:
      * `FLUSH PRIVILEGES;`
      * `ALTER USER 'root'@'localhost' IDENTIFIED BY 'YOUR_NEW_STRONG_PASSWORD';`
      * `quit;`
  * Stop the service: `sudo systemctl stop mysql.service`
  * Remove the passwordless authentication by removing the `[mysqld]` block you added above in the `/etc/mysql/my.cnf` configuration file.
  * Then, start the service: `sudo systemctl start mysql.service`

## Setting up PHP for Apache
* `sudo apt install php libapache2-mod-php php-mysql`
* Check your `php` version with `php -v`

```bash
ashiskb@ashiskb-office:~$ php -v
PHP 8.1.2-1ubuntu2.14 (cli) (built: Aug 18 2023 11:41:11) (NTS)
Copyright (c) The PHP Group
Zend Engine v4.1.2, Copyright (c) Zend Technologies
    with Zend OPcache v8.1.2-1ubuntu2.14, Copyright (c), by Zend Technologies
```

## Creating a VirtualHost for your website
* When using the Apache web server, you can create virtual hosts (similar to server blocks in Nginx) to encapsulate configuration details and host more than one domain from a single server.
* Apache on Ubuntu 22.04 has one virtual host enabled by default that is configured to serve documents from the `/var/www/html` directory. While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. Instead of modifying `/var/www/html`, we’ll create a directory structure within `/var/www` for the `your_domain` site/virtual host, leaving `/var/www/html` in place as the default directory to be served if a client request doesn’t match any other sites.
* Create a directory for one virtual host named `your_domain`: `sudo mkdir /var/www/your_domain`
* `sudo chown -R $USER:$USER /var/www/your_domain`
* Then, open a new configuration file in Apache’s `sites-available` directory by creating a file named after `your_domain`: `sudo nano /etc/apache2/sites-available/your_domain.conf`
  * This will create a blank file. Now add the following and edit accordingly:


```bash
<VirtualHost *:80>
    ServerName your_domain
    ServerAlias www.your_domain 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/your_domain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

  * With this VirtualHost configuration, we’re telling Apache to serve `your_domain` using `/var/www/your_domain` as the web root directory. If you’d like to test Apache without a domain name, you can remove or comment out the options `ServerName` and `ServerAlias` by adding a pound sign (#) the beginning of each option’s lines.
* Enable the virtual host by: `sudo a2ensite your_domain`
  * You can also disable a virtual host by: `sudo a2dissite your_domain`
* You may want to disable the Apache default site: `sudo a2dissite 000-default`
* Check with apache configtest if the `your_domain.conf` is free from systax error: `sudo apache2ctl configtest`
* Reload apache2 service: `sudo systemctl reload apache2`
  * Your new website, `your_domain` is now active.

## About DirectoryIndex
* With the default `DirectoryIndex` settings on Apache, a file named `index.html` will always take precedence over an `index.php` file
  * This is useful for setting up maintenance pages in PHP applications, by creating a temporary `index.html` file containing an informative message to visitors. Because this page will take precedence over the `index.php` page, it will then become the landing page for the application. 
  * Once maintenance is over, the `index.html` is renamed or removed from the document root, bringing back the regular application page.
  * Edit this config file to change the behavior: `sudo nano /etc/apache2/mods-enabled/dir.conf`

```bash
<IfModule mod_dir.c>
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

* Reload the apache2 service: `sudo systemctl reload apache2`
* To test your PHP installation, create a new file called `info.php` in your document root directory:

```info.php
<?php
  phpinfo();
?>
```
* Save and close the file. Open your web browser and visit [http://localhost/your_domain/info.php](http://localhost/your_domain/info.php). You should see the PHP information page, displaying the PHP version and configuration details.
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
* [How To Install Linux, Apache, MySQL, PHP (LAMP) Stack on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-22-04)