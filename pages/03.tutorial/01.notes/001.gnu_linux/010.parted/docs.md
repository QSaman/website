---
title: Parted
taxonomy:
    category: docs
---

### Partition Table

For a brief introduction you can read official [manual](https://www.gnu.org/software/parted/manual/html_node/Partitioning.html). Basically each storage device will have a structure like this:

```
+--+---------------+----------------+------------------------+
|PT|  Partition 1  |  Partition 2   |  Partition 3           |
+--+---------------+----------------+------------------------+
0 start                                                    end
```

At the beginning we have a partition table that specify how many partitions we have and the location of them. It can be `MBR` or `GPT` or other partition tables.

### Extended and Logical Partitions

Unlike "MBR", "GPT" doesn't require extend and logical partitions if you need more than 4 partitions. Out of the box, "GPT" supports 128 partitions. Fore more information refer to "advantages of GPT over MBR" in this [wiki](https://wiki.archlinux.org/title/Partitioning).

### Tips

* If you want to specify exact location you must use the sector unit "s". With parted 2.4 and newer IEC [binary units](https://en.wikipedia.org/wiki/Binary_prefix) like MiB, GiB, TiB, etc., specify exact locations as well. For more information refer to official [manual](https://www.gnu.org/software/parted/manual/html_node/Running-Parted.html)
* when creating a partition, you should prefer to specify units of bytes (“B”), sectors (“s”), or IEC binary units like “MiB”, but not “MB”, “GB”, etc. Read this [page](https://www.gnu.org/software/parted/manual/html_node/unit.html) for more information
* In `mkpart` command, start and end are the offset from the beginning of the disk, that is, the “distance” from the start of the disk. For more information read the [manual](https://www.gnu.org/software/parted/manual/html_node/mkpart.html)
* Note that "1KiB" is 1024 bytes but "1KB" is 1000 bytes. The should be no space between number and unit. For more information read the [manual](https://www.gnu.org/software/parted/manual/html_node/unit.html)
* The last sector is "-1s"

### Useful Links

* [Official manual](https://www.gnu.org/software/parted/manual/)
* [Commands](https://www.gnu.org/software/parted/manual/html_node/Command-explanations.html)
* [Unit](https://www.gnu.org/software/parted/manual/html_node/unit.html)
* [Arch Wiki Partitioning](https://wiki.archlinux.org/title/Partitioning)

