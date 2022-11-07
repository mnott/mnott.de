---
title: How to Install Apache HTTP Server on Windows
menu_order: 1
post_status: publish
post_excerpt: How to Install Apache with Virtual hosts on Windows.
taxonomy:
    category:
        - Computer
        - Apache
    post_tag:
        - Computer
        - Apache
comment_status: open
---

# Installation

Similar to [this description](https://www.mnott.de/how-to-install-apache-http-server-using-homebrew-on-macos/), we're doing an installation of Apache and PHP, this time on Windows 11. [Here](https://www.youtube.com/watch?v=3EAj9tsXLFU) is a nice tutorial. 

We're also going to install [KoolReport](https://koolreport.com/) in a virtual host setup.

## Download

### Download Apache

Go [here](https://www.apachelounge.com/download) and download the most recent version of `Apache 2.5.x Win64`.

Create a directory `c:\\www` and extract the content of the Apache zip file into it so that you'll get a directory `c:\\www\\Apache24\\conf`.
 
### Download PHP

Go [here](https://windows.php.net/download#php-8.1) and download PHP. Look for the `VS16 x64 Thread Safe` Zip. 

Create a directory `c:\\www\\php` and extract the content of the PHP zip file into it so that you'll get a file `c:\\www\\php\\php8apache2_4.dll`.


### Download KoolReport

Download KoolReport from their web site so that you'll get a file `koolreport_pro_6.0.6.zip`.

Create a directory `c:\\www\\koolreport` and extract the content of the KoolReport zip file into it so that you'll get a directory `c:\\www\\koolreport\\koolreport` and a directory `c:\\www\\koolreport\\examples`.


### Download VC Libraries

Go [here](https://visualstudio.microsoft.com/downloads/#microsoft-visual-c-redistributable-for-visual-studio-2019) and download `Microsoft Visual C++ Redistributable for Visual Studio 2022` for your platform (`x64` if you are running on a native machine; `ARM64` if you are running under Parallels for Mac)  this is under "Other Tools, Frameworks, and Redistributables" at the bottom of the page.

## Installation

### Installation of the VC Libraries

Go to where you downloaded the VC Libraries, and run the installer.

### Installation and Configuration of Apache

#### Basic configuration of Apache via httpd.conf

You need to configure Apache first. To do so, open `c:\\www\\Apache24\\conf\\httpd.conf` and change the line

```
Define SRVROOT "c:/Apache24"
```

to read

```
Define SRVROOT "c:/www/Apache24"
```

Also, search for the line

```
#Include conf/extra/httpd-vhosts.conf
```

and comment it in so that it reads

```
Include conf/extra/httpd-vhosts.conf
```

Do the same for the line

```
#LoadModule rewrite_module modules/mod_rewrite.so
```

so that it reads

```
LoadModule rewrite_module modules/mod_rewrite.so
```
 
and add

```
Include c:/www/Apache24/conf/extra/httpd-php.conf
```

right after it.

Add the following code just below the `LoadModule` section:

```
AddHandler application/x-httpd-php .php
AddType application/x-httpd-php .php .html
LoadModule php_module "c:/www/php/php8apache2_4.dll"
PHPIniDir "C:/www/php"
DirectoryIndex index.php
```


#### Configuration for PHP via of http-php.conf

Edit / create a file `c:\\www\\Apache24\\conf\\extra\\httpd-php.conf`:

```
<IfModule php_module>
  <FilesMatch \\.php$>
    SetHandler application/x-httpd-php
  </FilesMatch>

  <IfModule dir_module>
    DirectoryIndex index.html index.php
  </IfModule>
</IfModule>
```


#### Configuration of Virtual Hosts via httpd-vhosts.conf

Edit the file `c:\\www\\Apache24\\conf\\extra\\httpd-vhosts.conf` adding this section to it (if you had now content of your own in it, you can replace the file content, as there are some dummy definitions inside):

```
<VirtualHost *:80>
    ServerName koolreport-examples.test
    DocumentRoot c:/www/koolreport/examples

    <Directory c:/www/koolreport/examples>
        Require all granted
        AllowOverride All
    </Directory>
</VirtualHost>

<VirtualHost *:80>
    ServerName koolreport.test
    DocumentRoot c:/www/koolreport/www

    <Directory c:/www/koolreport/www>
        Require all granted
        AllowOverride All
    </Directory>
</VirtualHost>
```


Add to / modify the localhosts line in `c:\\windows\\system32\\drivers\\etc\\hosts` adding two domains for `koolreport` (you'll need an administrative command line for this):

```
127.0.0.1 	localhost koolreport-examples.test koolreport.test
```


#### Installation of Apache Service

Using an administrative command line, go into `c:\\www\\Apache24\\bin` and execute this command:

```
httpd.exe -k install -n "Apache24"
```

#### Start the Apache Service

Do this inside the administrative command line:

```
net start Apache24
```

In case of failure, look at the event viewer.



#### Prepare the Working Directory

Create a directory `c:\\www\\koolreport\\www` and place a file `phpinfo.php` into it with the following content:

```php
<?php phpinfo(); ?>
```


## Test the Installation

Go to [here](http://koolreport.test/phpinfo.php).

You should see the PHP Info Page.


---
Related: [[Computer/Other/- -|Other]]


<mark style="margin-top: 100; background-color: #3B3836; color: #494942">Created: 1`$=dv.span(dv.current().file.ctime)`</mark>