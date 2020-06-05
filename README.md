# website
My Personal Website

## Run without Web Server

```
php -S localhost:8000 system/router.php
```

## Cleaning the Cache

```
bin/grav clean
```

## Sidebar in Blog

Copy `.dependencies` from `grav-skeleton-blog-site` into user directory and then run:

```
bin/grav install
```

Then add `grav-skeleton-blog-site/pages/modules`.


## Install these plugins

```
$ bin/gpm install mathjax
$ bin/gpm install langswitcher
```
