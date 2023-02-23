---
title: split
taxonomy:
    category: docs
---

### Splitting a large file

We can use the following command to split a large file into smaller ones:

```
split Win11_22H2_English_x64v1.iso Win11_22H2_English_x64v1.iso_ -a 2 -d -C 1024MB
```

`-a` means the length of suffix should be 2 (it's the default one). `-d` uses numerical suffixes starting at 0. `-C` put at most `1024 MB` of data per output file. The above command generates the following files:

```
$ ls -lh
total 5.2G
-rw-r--r--. 1 saman saman 977M Feb 22 20:42 Win11_22H2_English_x64v1.iso_00
-rw-r--r--. 1 saman saman 977M Feb 22 20:42 Win11_22H2_English_x64v1.iso_01
-rw-r--r--. 1 saman saman 977M Feb 22 20:42 Win11_22H2_English_x64v1.iso_02
-rw-r--r--. 1 saman saman 977M Feb 22 20:42 Win11_22H2_English_x64v1.iso_03
-rw-r--r--. 1 saman saman 977M Feb 22 20:42 Win11_22H2_English_x64v1.iso_04
-rw-r--r--. 1 saman saman 418M Feb 22 20:42 Win11_22H2_English_x64v1.iso_05
```

We can use `sha256sum` to check file integrity later:

```
sha256sum Win11_22H2_English_x64v1.iso > Win11_22H2_English_x64v1.iso.sha256sum
```

### Concatenating files

We can merge all the generated files again using the following command:

```
cat Win11_22H2_English_x64v1.iso_0* > Win11_22H2_English_x64v1.iso
```

To check file integrity:

```
sha256sum -c Win11_22H2_English_x64v1.iso.sha256sum
```

