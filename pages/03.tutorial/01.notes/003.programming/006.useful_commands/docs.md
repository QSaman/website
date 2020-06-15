---
title: Useful Commands
taxonomy:
    category: docs
---

* To find the number of lines in a project, run the following command:

```
$ find . -name '*.cpp' | xargs wc -l
```

* If you want to run a cpu-intensive task you can run it with `nice` command to give it a lower priority.
