---
title: find
taxonomy:
    category: docs
---

# Examples
## Run all mp4 files in a directory recursively:

```
find -type f -iname '*.mp4' -exec mpv --shuffle '{}' +
```

Alternatively you can use `xargs`:

```
find -type f -iname '*.mp4' -print0 | xargs -0 mpv --shuffle
```

## Run selected file extensions

Suppose that you want to play all files with extension `mp4` and `mkv`:

```
find -type f \( -iname '*.mp4' -or -iname '*.mkv' \) -exec mpv --shuffle '{}' +
```
Note that in find `expr expr` is equivalent to `expr -and expr` so the parentheses are important.

## Play all video files

There isn't an easy one line to run only video files. You can use `file -i` to extract mime types. If we suppose all video files are at least 5MB:

```
find -type f -size +5M -exec mpv --shuffle '{}' +
```
