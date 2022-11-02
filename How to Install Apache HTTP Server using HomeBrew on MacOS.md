---

title: How to Install Apache HTTP Server using HomeBrew on MacOS
menu_order: 1
post_status: publish
post_excerpt: How to Install Apache with Virtual hosts on MacOS.
taxonomy:
    category:
        - Computer
        - Apache
        - HomeBrew
    post_tag:
        - Computer
        - Apache
        - HomeBrew
comment_status: open
---

# Installation

[How to Install Apache HTTP Server on MacOS – TecAdmin](https://tecadmin.net/install-apache-macos-homebrew/)

A much better description is [here](https://www.git-tower.com/blog/apache-on-macos/) from the folks at Git-Tower.

We need to install a local apache. This page shows how to install `http` doing this:

```bash
brew install httpd
sudo brew services start httpd
```

I found `apache2` as a module, so I am trying that instead:

```bash
brew install apache2
brew services start apache2
```

Note that using `sudo`, this fails telling me that Homebrew should not be started as root. So start the apache as normal user.

The Git-Tower folks say it's better to do this:

```bash
brew install httpd php
```

Note that we're using the fact that httpd refers to Apache; also note that we're installing php should you not have it.

# Starting, stopping, restarting

```bash
brew services start httpd
brew services restart httpd
brew services stop httpd
```

It has started an apache daemon on port `8080`.

# Configuration

The tutorial refers to `/usr/local/etc/httpd` and `/usr/local/var/www`, but in fact, at least for me, the configuration is in `/opt/homebrew/etc/httpd`, and the webroot is in `/opt/homebrew/var/www`.


# Install PHP with Apache

Open `/opt/homebrew/etc/httpd/httpd.conf` and do these changes:

```
Listen 8080
```

to

```
Listen 80
```

Comment in

```
Include /opt/homebrew/etc/httpd/extra/httpd-vhosts.conf
```

as well as

```
Include /opt/homebrew/etc/httpd/extra/httpd-vhosts.conf
```


Add these lines where the modules are loaded:

```
LoadModule php_module /opt/homebrew/opt/php/lib/httpd/modules/libphp.so
Include /opt/homebrew/etc/httpd/extra/httpd-php.conf
```

Edit / create a file `/opt/homebrew/etc/httpd/extra/httpd-php.conf`:

```
<IfModule php_module>
  <FilesMatch \.php$>
    SetHandler application/x-httpd-php
  </FilesMatch>

  <IfModule dir_module>
    DirectoryIndex index.html index.php
  </IfModule>
</IfModule>
```

# Virtual Host Setup

Edit `/opt/homebrew/etc/httpd/extra/httpd-vhosts.conf`.

I've e.g. added something like this:

```
<VirtualHost *:80>
    ServerName koolreport.test
    DocumentRoot /Users/i052341/Cloud/Development/php/koolreport/vendor/examples

    <Directory /Users/i052341/Cloud/Development/php/koolreport/vendor/examples>
        Require all granted
        AllowOverride All
    </Directory>
</VirtualHost>
```

After having added `koolreport.test` to `/etc/hosts` for `127.0.0.1`.

I'm adding `koolreport.test` as an virtual host; I'll blog in a separate post about the `koolreport` installation.



# Add a simple phpinfo.php

Create a file  `/opt/homebrew/var/www/phpinfo.php` with this content:

```php
<?php phpinfo();
```

# Restart the Server

```bash
brew services restart httpd
```






---
<mark style="margin-top: 100; background-color: #3B3836; color: #494942">Created: 1`$=dv.span(dv.current().file.ctime)`</mark>