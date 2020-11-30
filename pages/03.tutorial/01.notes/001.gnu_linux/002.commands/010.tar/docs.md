---
title: tar
taxonomy:
    category: docs
---


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
