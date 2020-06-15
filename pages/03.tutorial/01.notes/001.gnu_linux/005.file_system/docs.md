---
title: File System
taxonomy:
    category: docs
---

## Finding device name
To find device name of a mounted directory:

```
findmnt /path/to/directory
```
For example in a Fedora machine:

```
findmnt /run/media/[username]/[external_hdd_label]
```

## Finding inode usage

The first command show disk usage by files and the second ones shows only inode disk usage

```
df -h .
df -hi .
```

# ext4

To create an ext4 partition:

```
mkfs.ext4 /dev/sdx
```
To see the default parameters see `/etc/mke2fs.conf`. For more information read this [article](https://wiki.archlinux.org/index.php/ext4).

## ext4 partition attributes

You can see ext4 attributes using the following command:

```
tune2fs -l /dev/sdxx
```
### reserved space

Usually %5 of disk is reserved. You can check it using `tune2fs`. For example:

```
# tune2fs -l /dev/sdc2 | grep -i block
Block count:              976702976
Reserved block count:     48835148
Free blocks:              377076996
First block:              0
Block size:               4096
```

As you can see in the above example we are using `[Reserved block count] * [Block size]` bytes as reserved. As you can see `[Reserved block count] / [Block count] = 0.05`. You can reduce the reserved space from %5 to %1 using the following command:

```
# tune2fs -m 1 /dev/device
```

If you want to set the reserved space absolutely, you can use `-r` instead of `-m`. You need to specify reserved block count for `-r`.
