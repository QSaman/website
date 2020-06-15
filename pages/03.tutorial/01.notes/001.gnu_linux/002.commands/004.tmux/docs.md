---
title: tmux
taxonomy:
    category: docs
---

First exit all tmux sessions. Then create `~/.tmux.conf` and add these lines to have VIM-like shortcuts to change panes:

```
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R
``` 
# Using tmux in multiple monitors

You can connect to the same session but it acts as a mirror. A better approach is to group sessions. Run the first command in left monitor and the other on the right one:

```
tmux new -s left_monitor
tmux new -t left_monitor -s right_monitor
```
Now you have two different sessions (in this case `left_monitor` and `right_monitor`) that shares windows together.
