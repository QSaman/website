---
title: Comparing two binary files
taxonomy:
    category: docs
---

## Method 1

Suppose we want to compare two binary files `image.png` and `image-win.png`. First run the following command to convert them to hex files:

```
$ xxd image.png > image.hex
$ xxd image-win.png > image-win.hex
``` 

Then you can use your favorite editor to compare them. For example `kdiff3`:

```
$ kdiff3 image.hex image-win.hex
```

You can use `hexdump` instead of `xxd`.

## Method 2

Use `DHEX` program:

```
$ dhex image.png image-win.png

```

## Method 3

Use `VBINDIFF` program:

```
$ vbindiff image.png image-win.png
```
