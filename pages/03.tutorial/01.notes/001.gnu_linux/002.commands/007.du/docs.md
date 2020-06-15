---
title: du
taxonomy:
    category: docs
---

# Sort directories by their size

for listing all directories (even hidden ones) and sort them based on their size, you can run one of the following commands.

```
$ find -maxdepth 1 -type d -exec du -hs '{}' \; | sort -hr | less
$ ls -A | xargs -d '\n' du -hs | sort -hr | less
```
