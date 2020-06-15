---
title: Samba
taxonomy:
    category: docs
---

# Mounting a Samba directory
 
```
mount -t cifs //SERVER/sharename /mnt/mountpoint -o username=username
```

Since you need to mount using `sudo`, it's likely you don't have write permission as a normal user. You can use 

```
mount -t cifs //SERVER/sharename /mnt/mountpoint -o uid=$(id -u),gid=$(id -g),username=username
```

For more information run `man mount.cifs`. Add the following lines into your sudoer file. Replace user_group with a suitable one:

```
%user_group ALL=(ALL) NOPASSWD: /usr/sbin/mount.cifs
%user_group ALL=(ALL) NOPASSWD: /usr/bin/mount -t cifs *
%user_group ALL=(ALL) NOPASSWD: /usr/bin/umount -t cifs *
```
