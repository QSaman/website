---
title: dd
taxonomy:
    category: docs
---

For some interesting examples like creating ISO image or cloning a partition or disk wipe, read this [article](https://en.wikipedia.org/wiki/Dd_(Unix)#Uses).

## Test Disk I/O Performance with dd command

For more information visit these sites:
* [cyberciti](https://www.cyberciti.biz/faq/howto-linux-unix-test-disk-performance-with-dd-command/)
* [ArchLinux Wiki](https://wiki.archlinux.org/index.php/Benchmarking#dd)
* [romanrm](https://romanrm.net/dd-benchmark)

### Writing Speed

To test throuput (write speed):

```
dd if=/dev/zero of=/tmp/test1.img bs=1G count=1 oflag=dsync
```

To test latency:

```
dd if=/dev/zero of=/tmp/test2.img bs=512 count=1000 oflag=dsync
```

### Reading Speed

For an accurate test you must clear the cache which requires root access:

```
echo 3 > /proc/sys/vm/drop_caches
```

then you can use `dd` to measure read speed:

```
time dd if=/path/to/bigfile of=/dev/null bs=8k
```
