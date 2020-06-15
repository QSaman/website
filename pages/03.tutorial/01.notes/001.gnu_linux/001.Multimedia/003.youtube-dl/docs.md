---
title: youtube-dl
taxonomy:
    category: docs
---

 
```
youtube-dl -f"137+140" -o '%(uploader)s/%(playlist)s/%(playlist_index)03d - %(title)s-%(id)s.%(ext)s' "PLldXmXAqI3VPwit6k_-7AJU7aHSJzlo-8"
```

If there is at least one private video in the playlist, use `-i` flag to ignore them.
