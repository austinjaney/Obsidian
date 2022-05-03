# How to Install DokuWiki on CentOS 7
#sysadmin #linux #centOS7 

Published on: Fri, Jun 22, 2018 at 11:15 am EST

This article is a port of "How to Install DokuWiki on Ubuntu 16.04 LTS" for CentOS 7.
DokuWiki is an open source wiki software written in PHP that doesn't require a database. It stores data in text files. DokuWiki source code is publicly hosted on GitHub. This guide will show you how to install DokuWiki on a fresh CentOS 7 Vultr instance.
Requirements
Make sure your server meets the following requirements.

* Web server software that supports PHP (Apache, Nginx, IIS, Lighttpd, LiteSpeed)
* PHP version 5.6 or later

Before you Begin
Check the CentOS version.

```bash
cat /etc/centos-release
# CentOS Linux release 7.4.1708 (Core)
```

Create a new non-root user account with sudo access and switch to it.

```bash
useradd -c "John Doe" johndoe && passwd johndoe
usermod -aG wheel johndoe
su - johndoe
```

NOTE: Replace johndoe with your username.
Set up the timezone.

```bash
timedatectl list-timezones
sudo timedatectl set-timezone 'Region/City'
```

Ensure that your system is up to date.

```bash
sudo yum update -y
 
```

Install required and useful packages.

```bash
sudo yum install -y wget vim bash-completion
```

Disable SELinux.

```bash
sudo setenforce 0
```

Step 1 - Install PHP and PHP extensions
In this tutorial, we will use PHP 7.1 which is not available in the default CentOS repositories, so you will need to use a third-party repository, such as Webtatic. Follow this Vultr article to install PHP 7 before continuing with this article.
Install PHP 7.1 and required PHP extensions.
sudo yum install -y php71w php71w-cli php71w-fpm php71w-gd php71w-xml php71w-zip

Check the version.

```bash
php --version

# PHP 7.1.14 (cli) (built: Feb  4 2018 09:05:29) ( NTS )
# Copyright (c) 1997-2018 The PHP Group
# Zend Engine v3.1.0, Copyright (c) 1998-2018 Zend Technologies
```

Start and enable PHP-FPM.

```bash
sudo systemctl start php-fpm.service
sudo systemctl enable php-fpm.service
```

Step 2 - Install and configure Nginx
If you prefer Apache or another popular web server, you can use one of those instead of Nginx.
Install Nginx.

```bash
sudo yum install -y nginx
```

Check the version.

```bash
nginx -v
# nginx version: nginx/1.12.2
```

Start and enable Nginx.

```bash
sudo systemctl start nginx.service
sudo systemctl enable nginx.service
```

Configure Nginx.

```bash
sudo vim /etc/nginx/conf.d/dokuwiki.conf
```

Copy/paste the following Nginx cofiguration and save it.

```bash
server {
    listen [::]:80;
    listen 80;

    server_name wiki.example.com; # Replace with your hostname
    root /var/www/dokuwiki;
    index index.html index.htm index.php doku.php;

    client_max_body_size 15M;
    client_body_buffer_size 128K;

    location / {
        try_files $uri $uri/ @dokuwiki;
    }

    location ^~ /conf/ { return 403; }
    location ^~ /data/ { return 403; }
    location ~ /\.ht { deny all; }

    location @dokuwiki {
        rewrite ^/_media/(.*) /lib/exe/fetch.php?media=$1 last;
        rewrite ^/_detail/(.*) /lib/exe/detail.php?media=$1 last;
        rewrite ^/_export/([^/]+)/(.*) /doku.php?do=export_$1&id=$2 last;
        rewrite ^/(.*) /doku.php?id=$1 last;
    }
    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index  index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
}
```

Check the configuration.

```bash
sudo nginx -t
```

Reload Nginx.

```bash
sudo systemctl reload nginx.service
```

Step 3 - Install DokuWiki
Create a document root directory.

```bash
sudo mkdir -p /var/www/dokuwiki
```

Change ownership of the /var/www/dokuwiki directory to johndoe.

```bash
sudo chown -R johndoe:johndoe /var/www/dokuwiki
```

Navigate to the document root.

```bash
cd /var/www/dokuwiki
```

Download the newest stable release of DokuWiki from the DokuWiki download page.

```bash
wget https://download.dokuwiki.org/src/dokuwiki/dokuwiki-stable.tgz
```

Unpack the DokuWiki tarball.

```bash
tar xvf dokuwiki-stable.tgz
rm dokuwiki-stable.tgz
mv dokuwiki-2017-02-19e/* . && mv dokuwiki-2017-02-19e/.* .
rmdir dokuwiki-2017-02-19e/
```

Change ownership of the /var/www/dokuwiki directory to nginx.

```bash
sudo chown -R nginx:nginx /var/www/dokuwiki
```

Run sudo vim /etc/php-fpm.d/www.conf and set the user and group to nginx.

```bash
sudo vim /etc/php-fpm.d/www.conf
# user = nginx
# group = nginx
```

Restart php-fpm.service.

```bash
sudo systemctl restart php-fpm.service
```

As the last step, run the DokuWiki setup script install.php in your browser and setup DokuWiki. The script checks for the availability of required PHP functions and checks for needed file permissions. It also creates an initial administrator account and an initial ACL policy. To run the installer, open http://wiki.example.com/install.php in the browser and follow the instructions.
Upon successful configuration, delete the install.php file from the DokuWiki root directory.

```bash
sudo rm /var/www/dokuwiki/install.php
```

Congratulations, DokuWiki is installed and you will be able to access and edit a functional wiki at http://wiki.example.com/. Enjoy your new DokuWiki installation.