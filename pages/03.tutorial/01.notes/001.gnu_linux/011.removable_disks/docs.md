---
title: Removable Disks
taxonomy:
    category: docs
---

### Customizing mount point

There are two options:

#### Using `fstab`

First we need to find UUID of the disk. Assuming we know our disk is mapped to `/dev/sdx`, we can find UUID by running the following command:

```
$ blkid /dev/sdx
```

Note that if the disk has multiple partitions, we can get UUID for each partition. For example for second partition we can use:

```
$ blkid /dev/sdx2
```

Now we can edit `/etc/fstab`:

```
/dev/sdx2 /mnt/backup ext4 nofail,x-systemd.device-timeout=1ms 0 2

```

For more information visit this wiki [page](https://wiki.archlinux.org/title/Fstab#External_devices).

The problem with this approach is if you disconnect the disk and then connect it again, it won't be mounted automatically. You need to run the following command to mount all enteries in `/etc/fstab`:

```
$ sudo mount --all
```

#### Using `udisks2`

There aren't a lot of customization here. By default `udisks` mounts under `/run/media/username`. Let's assume there are two users "foo" and "bar", then we have:

```
ls -l /run/media
drwxr-x---+ 2 root root 40 Nov 21 18:00 foo
drwxr-x---+ 2 root root 40 Nov 21 18:00 bar
```
The last "+" means there are [ACL](https://en.wikipedia.org/wiki/Access-control_list) rules. We can get the rules for user "foo" by running the following command:

```
$ getfacl /run/media/foo
getfacl: Removing leading '/' from absolute path names
# file: run/media/foo/
# owner: root
# group: root
user::rwx
user:foo:r-x
group::---
mask::r-x
other::---
```

So everything under `/run/media/foo` is accessible only to user "foo". To share removable disk among users, we can create/edit `/etc/udev/rules.d/99-udisks2.rules` like this:

```
# UDISKS_FILESYSTEM_SHARED
# ==1: mount filesystem to a shared directory (/media/VolumeName)
# ==0: mount filesystem to a private directory (/run/media/$USER/VolumeName)
# See udisks(8)
ENV{ID_FS_USAGE}=="filesystem|other|crypto", ENV{UDISKS_FILESYSTEM_SHARED}="1"
```

For more information visit this [page](https://wiki.archlinux.org/title/Udisks#Mount_to_/media_(udisks2)).

##### Ubuntu Patch

Modern versions of Ubuntu are using different mount points. Instead of `/run/media/[username]`, they are using `/media/[username]`. You can use `udisksctl` to see mount points.

### `udisksctl` Tips

Most GNU/Linux distros, don't automatically mount the disk, if you want to mount `/dev/sdx1`, you can use:

```
$ udisksctl mount -b /dev/sdx1
```
For more information you can run `udisksctl mount --help`. You can find useful information, including mount points, by using:

```
$ udisksctl info -b /dev/sdx1
```
