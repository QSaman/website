# website
My Personal Website

## Bootstrapping

First run Grav without empty config so it initialize everything. Then you can run the following commands:

```
cd user
rm config/site.yaml config/system.yaml
rm -rf pages/*
git init
git remote add origin git@github.com:QSaman/website.git
git pull origin master
git branch --set-upstream-to=origin/master master
git submodule update --init
```

## Run without Web Server

```
php -S localhost:8000 system/router.php
```

## Update Grav

```
bin/gpm selfupgrade -f
```

## Cleaning the Cache

```
bin/grav clean
```

## Sidebar in Blog

```
bin/grav install
```
## Install these plugins

```
$ bin/gpm install mathjax
$ bin/gpm install langswitcher
$ bin/gpm install language-selector
$ bin/gpm install themer 
$ bin/gpm install anchors
$ bin/gpm install highlight
$ bin/gpm install devtools
$ bin/gpm install markdown-notices
```

## Install these themes

```
$ bin/gpm install learn2
```
