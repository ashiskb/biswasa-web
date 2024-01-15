---
title: 'Managing SSL Certificates with Amazon AWS Lightsail instance with Apache2 webservice'
date: 2024-01-14
toc: true
permalink: /posts/2024/01/biswas/blog-aws-lightsail-ssl-apache2/
tags:
  - https
  - ssl
  - cloudflare
  - aws
  - lightsail
  - apache2
  - ubuntu
  - domain-hosting
---

This post is for enabling a SSL certificate `for free` from [Cloudflare.com](https://www.cloudflare.com). Say, you purchased a domain name (e.g., `example.com` from [Godaddy](https://www.godaddy.com)). Now, you want an inexpensive way to host your website. Again, say you hosted the website at [Amazon AWS Lightsail (either $3.50/mo or $5.00/mo as of the time of writing)](https://aws.amazon.com/lightsail/) with configuration like: {`Platform`: `Linux/Unix`, `blueprint`:`Os only`, `OS`:`Ubuntu 22.04`}. You then bind a AWS provided static IP with the instance and create a `DNS zone` with couple of `DNS records` like: {`A records`:[`Record name`:`example.com`,`Route traffic to`:`1.2.3.4`],[`Record name`:`*.example.com`,`Route traffic to`:`1.2.3.4`]}. Also assign the created `DNS zone` to the instance. Finally, copy the AWS provided list of `Name servers` into Godaddy's `Nameservers` management page for your purchased domain while removing existing ones. And, you host your website files with Apache2 in your Lightsail instance. [Here is a post I wrote on how to configure a site with apache2](/posts/2023/09/biswas/blog-apache2-ubuntu/).

## Pre-requisites
* You have done everything I said in the paragraph above.

## Steps to enable SSL in your lightsail hosted website
1. Signup a free account at [Cloudflare.com](https://www.cloudflare.com), if you have no prior accounts there.
2. Login at Cloudflare.
3. `Add a site` into your Cloudflare dashboard and enter the site name: `example.com`
4. `Free Plan` is the plan you'd select, and click `Get started`. The free plan option can be anywhere in the `Select a plan` page. So, please be curious enough to find it.
   * Then, Cloudflare would perform a quick scan to show you current DNS records of your domain. Make a note of the summary, as you will have to come back to re-configure this at a later step.
   * Hit the `Continue` button.
5. Cloudflare will offer you few steps to `change your nameservers`. For instance,
   * Determine your registrar via [WHOIS](https://lookup.icann.org/en/lookup)
   * Login to Godaddy (in my case it was the registrar)
   * Remove the existing nameservers (i.e., the ones you got from AWS lightsail)
   * Add the Cloudflare provided nameservers.
   * Save your changes and wait (couple of minutes; atmost 24 hours). Check the WHOIS lookup again to see if the nameserver updates are completed or not.
6. Once Cloudflare determines that your domain's nameservers are updated accordingly, you'll receive a `Status active` email, and also see the expected changes in the WHOIS lookup.
7. At the Cloudflare dashboard do the following:
   * `DNS` pane:
     * Remove existing `DNS records`.
     * `Add record`: {`type`:`A`, `Name`:`*`, `IPv4 address`:`1.2.3.4`}
       * This specific `A record` is to enable domain name mapping for services like `ssh`, or `ping` etc.
     * `Add record`: {`type`:`A`, `Name`:`@`, `IPv4 address`:`1.2.3.4`}
     * `Add record`: {`type`:`CNAME`, `Name`:`www`, `IPv4 address`:`1.2.3.4`}
   * `SSL/TSL` pane:
     * At the `Overview` sub-pane: choose the option `Full (strict)`
     * At the `Origin Servers` sub-pane: `create a certificate`.
       * Choose option `Generate private key and CSR with Cloudflare`. Select `RSA (2048)` as the private key type. Make sure the hostnames are selected as: `*.example.com`, `example.com`. You have option choose even `15 years` as the validity period for the generated SSL certificate. Then, hit the `Create` button. 
       * Copy the content of `Origin certificate` content into a file named, `example.com.pem`
       * Copy the content of `Private key` content into a file named, `example.com.key`
   * `Edge Certificates` pane:
     * Configure status of the generated SSL certificate. Don't worry about the expiry date as it's going to be auto-renewed every 3 months for 15 years (if you chose that option in the previous step).
     * `Always Use HTTPS`: Turn this option to `ON` position.
     * `Automatic HTTPS Rewrites`: Turn this option to `ON` position.
   * `Overview` pane:
     * (Temporarily) turn on the `Development mode` to disable the caching.
8. At the Lightsail console:
   * `Instance` > `Networking` tab: at the `IPv4 Firewall` section add a new rule for the `HTTPS`: {`Application`:`HTTPS`, `Protocol`:`TCP`, `Port or range`: `443`}
   * Copy the `example.com.pem` and `example.com.key` files into `/etc/ssl/` directory.
   * Assuming `example.com.conf` is available at the `/etc/apache2/sites-available/` directory. If not, make sure the following is configured in the `/etc/apache2/sites-available/example.com.conf`:

```
<VirtualHost *:80>
    ServerName example.com
    ServerAlias www.exammple.com
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/example.com
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
  
   * Now, create the `example.com-ssl.conf` file with SSL configuration:

```
<VirtualHost *:443>
    ServerName example.com
    ServerAlias www.example.com
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/example.com
    SSLEngine on
    SSLCertificateFile /etc/ssl/example.com.pem
    SSLCertificateKeyFile /etc/ssl/example.com.key
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

   * Tell ubuntu system to use it by:
     * `sudo a2ensite example.com.conf`
     * `sudo a2ensite example.com-ssl.conf`
     * You may need to remmove the default ones, depending on your use case.
     * `sudo a2enmod ssl`
     * Reboot the `apache2` service: `sudo systemctl restart apache2`


## Testing if the SSL/HTTPS is active for your site
* Open a browser and go `example.com`
  * It should automatically go to `https://example.com`, and you'd see the `lock` icon at the browser's address bar noting the connection is secure. You can inspect the certificate to review the details.

## Possible Hiccups while going through the steps above
* **Issue 1**: 522 timeout received with the website host not responding.
  * **Solution**: Either `apache2` is not configured properly to serve any `https` requests, or your lightsail instance's `ipv4 firewall` is blocking any `443` port requests which is essential for `https`.
* **Issue 2**: website not loaded / recursive forward error.
  * **Solution**: Please do not add any additional page forwarding rules to switch from `http` to `https` as at `Cloudflare` dashboard we have already configured options like `always use https`, etc.



