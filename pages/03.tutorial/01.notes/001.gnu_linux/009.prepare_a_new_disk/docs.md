---
title: Prepare a New Disk
taxonomy:
    category: docs
---

### Creating a Partition table

We are using `parted` to do it. If you prefer a GUI, you can use `GParted`. First we need to find the disk:

```
$ sudo parted -l
```

Let's assume it's `/dev/sda`, then we can create a partition table `gpt` in interactive mode:

```
$ sudo parted /dev/sda
(parted) mklabel gpt
```

for more other options you can run `help mklable` in interactive mode.

### Create a Partition

We want to create just one partition that covers the entire disk. Enter into interactive mode:

```
$ sudo parted /dev/sda
(parted) mkpart primary ext4 0% 100%
```
When we use `0%` as the begin, it takes care of alignment for more information visit [Arch Wiki](https://wiki.archlinux.org/title/Parted#Alignment). You can run `help mkpart` in interactive mode to see other options.

Note that `mkpart` does not create a new file system. By specifying `ext4`, we help it to specify a partition ID.

Now we have a new partition name `/dev/sda1` (Note that `dev/sda` is the name of drive). To make sure the partition is fully aligned, you can run `align-check optimal 1` in interactive mode. Here `1` is partition number.

### Create a File System on New External Drive Partition

We use `ext4` as a new file system.:

```
$ sudo mkfs.ext4 -T largefile4 -m 0 -L movies /dev/sda1
```

Note that if you use `/dev/sda` (the name of disk, instead of partition), `mkfs.ext4` issues a warning.

We assume we are using this drive for large files like movies. `largefile4` is defined in `/etc/mke2fs.conf`. It has `4194304` [inode](https://en.wikipedia.org/wiki/Inode) ratio. In other words it creates one [inode](https://en.wikipedia.org/wiki/Inode) for every `4 MiB` (`419304` bytes). As a reminder every file, directory or link occupies one [inode](https://en.wikipedia.org/wiki/Inode). You cannot add new files if there is no empty inode, even if there are empty spaces. By specifying`4 Mib` inode ration,  we assumed the average file size is above `4 MiB`. Fore more information visit [Arch Wiki](https://wiki.archlinux.org/title/Ext4#Bytes-per-inode_ratio).

By default `ext4` occupy `5%` of the disk for super user. We can set it to `0%` by `-m 0`. Since it's for movie archive, we won't have a problem in future. We can change it later. For more information visit [Arch Wiki](https://wiki.archlinux.org/title/Ext4#Reserved_blocks). 

### Creating a File System on a New Flash Drive Partition

Since the size of a flash drive is not large, we can use `-i` which defines "bytes-per-inode". If we assume the average file size is `128 KiB` (`131072 bytes`), we can specify it explicitly:

```
sudo mkfs.ext4 -i 131072  -m 0 -L NixData /dev/sda3
```

### Useful Links

* [Ext4 on Arch Wiki](https://wiki.archlinux.org/title/Ext4)
* [Parted on Arch Wiki](https://wiki.archlinux.org/title/Parted)
