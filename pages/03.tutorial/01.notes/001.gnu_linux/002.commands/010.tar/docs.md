---
title: tar
taxonomy:
    category: docs
---

## List the content of a tar file

```
tar -tvf /path/to/tar
```

## Extract a tar file

```
tar -xvf /path/to/tar
```

## Extract a tar file to `/usr/local`

Let's assume `foo.tar` has the following structure:

```
$ tar -tvf foo.tar
foo0.1/
foo0.1/bin/
foo0.1/bin/foo
foo0.1/include/
foo0.1/include/foo.h
foo0.1/lib/
foo0.1/lib/libfoo.so
foo0.1/share/
foo0.1/share/man/
foo0.1/share/man1/
foo0.1/share/man/man1/foo.1
```

We want to copy its content in such a way that we have:

```
usr/local/bin/foo
usr/local/include/foo.h
usr/local/lib/libfoo.so
usr/local/share/man/man1/foo.1
```

We can achieve it by running the following command:

```
$ sudo tar -xvf foo.tar --strip-components=1 -C /usr/local/
```

## Tar Files On the Fly over the Network

We can tar/untar on the fly using `-f -` switch. First see the following example:

```
$ tar -cvf - foo.txt > foo.txt.tar
$ tar -cvf - bar_dir/ > bar_dir.tar
```

We can extend the above logic to tar/untar on the fly:

```
$ tar -cvf - ../foo | tar -xv
```

We can also use netcat to send files between different hosts. On the receiver we use

```
ncat -l -p 1234 | pv | tar -xv
```
and on the sender we use:

```
tar -cvf - my_movie.mp4 | pv | ncat -v receiver_ip 1234
```

Note that `pv` shows us the progress. We can also send a directory. The receiver command is the same but for sender:

```
tar -cvf - foo/ | pv | ncat -v receiver_ip 1234
```
