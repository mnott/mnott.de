---
title: How to make KoolReport run in Virtual Directories
menu_order: 1
post_status: publish
post_excerpt: KoolReport does not look up its root path correctly. Here's a fix.
taxonomy:
    category:
        - KoolReport
        - PHP
    post_tag:
        - KoolReport
        - PHP
comment_status: open
---

I was wondering why the path lookup in KoolReport (see also [this](https://www.mnott.de/how-to-install-apache-http-server-on-windows/)) doesn't work. Turns out, in `vendor/examples/helpers/common.php`, you need to change

```php
	return $root_url;
```

into this:

```php
    $protocol   = empty($_SERVER['HTTPS'])? 'http' : 'https';
    $servername = $_SERVER['SERVER_NAME'];
    $serverport = $_SERVER['SERVER_PORT']=='80'? '' : ':' . $_SERVER['SERVER_PORT'];
    $path       = str_replace('\\', '/', substr(dirname(__FILE__), strlen($_SERVER['DOCUMENT_ROOT'])));

    $root_url = $protocol . '://' . $servername . $serverport . $root_url;

    // error_log(print_r("Root Url: " . $root_url, TRUE));

    return $root_url;
```


---
Related: [[Computer/Other/- -|Other]]


<mark style="margin-top: 100; background-color: #3B3836; color: #494942">Created: 1`$=dv.span(dv.current().file.ctime)`</mark>