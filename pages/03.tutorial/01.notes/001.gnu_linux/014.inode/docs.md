---
title: inode
taxonomy:
    category: docs
---

According to [Wikipedia](https://en.wikipedia.org/wiki/Inode), The inode (index node) is a data structure in a Unix-style file system that describes a file-system object such as a file or a directory. Each inode stores the attributes and disk block locations of the object's data.

### Mounting a directory

Let's create a directory as a [mount point](https://en.wikipedia.org/wiki/Mount_(computing)#MOUNT-POINT):

```
$ sudo mkdir /mnt/my_directory
$ ls -id /mnt/my_directory/
934805 /mnt/my_directory/
```

As you can see `my_directory` has inode `934805`. `-d` tells `ls` to not show directory content. `i` tells `ls` to show corresponding inodes. Now consider this:

```
$ cd /mnt/my_directory/

$ ls -id .
934805 .

$ sudo mount /dev/sdc1 /mnt/my_directory/

$ ls
$ ls -id
934805 .

$ ls -id /mnt/my_directory/
2 /mnt/my_directory/

$ cd ../my_directory/
$ ls -id .
2 .

$ ls
Movies
```

So when we mount `my_directory`, its inode changes to `2`. Since we are in `my_directory`, it still shows the old inode. So we need to re-enter that directory to see the changes. If we unmount `my_directory`, the old inode will be assigned:

```
$ sudo umount /mnt/my_directory
$ ls -id /mnt/my_directory/
934805 /mnt/my_directory/
```
