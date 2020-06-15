---
title: rpm
taxonomy:
    category: docs
---

# Package Install (Update) Date

If you want to know when you installed or update a package, run the following command:

```
$ rpm -qi [package_name]
``` 

You can use the following command also:

```
$ rpm --last -q [package_name]
```

# Package Changelog

If you want to see an `installed` package changelog, run the following command:

```
$ rpm --changelog -q [package_name] | less
```

If it's not installed you need to use your distro package manager. For example in Fedora 26:

```
$ dnf updateinfo summary [package_name]
$ dnf updateinfo list [package_name]
$ dnf updateinfo info [package_name]
```
