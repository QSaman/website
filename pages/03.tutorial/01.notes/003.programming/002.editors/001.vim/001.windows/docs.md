---
title: Windows
taxonomy:
    category: docs
---

In other editors **tabs** are proxies for files. In Vim they are collections of windows. In other editors each tab represent one file. This method usually is not scalable. It's difficult to find a tab when you open a lot of them. In Vim each file is represented by a buffer. You can use one window or more to show parts of it. You can organize windows in a tab. Each tab may have its own working directory (using `:tcd` command). For more information you can read these links:

* [The patient vimmer](https://romainl.github.io/the-patient-vimmer/1.html)

## Windows

For more information run `:h windows.txt`.

### Splitting Window

#### Horizontal Splitting

* By pressing `CTRL-w s` `in normal mode`
* By running `:split` in `command mode`

#### Vertical Splitting

* By pressing `CTRL-w v` in `normal mode`
* By running `:vsplit` in `command mode`

### Closing a Window

* By pressing `CTRL-w q` in `normal mode`
* By running `:q` in `command mode`

### New Window

You can create a new window using `:new` which will be opened horizontally or `:vnew` which will be opened vertically

### Moving to the top-left window

Press `CTRL-w t` in `normal mode`

### Moving to the bottom-right window

Press `CTRL-w b` in `normal mode`

### Changing the Layout of Windows

#### `CTRL-w K`

Note that `K` is uppercase. Move the current window to be at the very top, using the full width of screen. In other words if we have two vertically-splitted Windows, this command makes them horizontally splitted.

#### `CTRL-w J`

Note that `J` is uppercase. Move the current window to be at the very bottom, using the full width of screen. In other words if we have two vertically-splitted Windows, this command makes them horizontally splitted.

#### `CTRL-w H`

Move the current window to be at the far left, using the full height of the screen. In other words if we have two horizontally-splitted windows, this command makes them vertically splitted.

#### `CTRL-w L`

Move the current window to be at the far right, using the full height of the screen. In other words if we have two horizontally-splitted windows, this command makes them vertically splitted.

#### `CTRL-w T`

If we have more than one window in the current tab, moves the current window to a new tab.

#### `CTRL-w o`

Make the current window the only one on the screen. All other windows are closed.

## Tabs are not buffers

Unlike other editors, tabs are different in Vim:

* A buffer is a file in memory
* A window is a viewport of a buffer. It is possible multiple windows are showing different portions of the same buffer
* A tab is a collection of windows

For a good explanation you can read these Stack Overflow [post 1](https://stackoverflow.com/questions/26708822/why-do-vim-experts-prefer-buffers-over-tabs/26710166#26710166), [post 2](https://stackoverflow.com/a/21338192) and this Wiki [page](https://vim.fandom.com/wiki/Category:Tabs).

## Buffers

### Switching to previous buffer

You can use `CTRL-6` or `CTRL-^` or `:e #` to switch to alternate buffer. Mostly the alternate buffer is the previously edited file.

### Listing buffers

Use `:ls` or `:buffers` or `:files` to see the list of buffers. You can use `:ls!` to include unlisted buffers (e.g. `:h windows` opens `windows.txt` and puts the corresponding buffer in the unlisted).

### Delete buffers

You can use `:bdelete` or `:bd` to delete a buffer and move it to unlisted (you can still see it using `:ls!`). The argument is buffer number or buffer name. You can also use a range (e.g. `:5-10bd` deletes buffers in the range `[5, 10]` or `:%bd` deletes all buffers).

### Split the window with a new unnamed buffer

You can use `:new` to split the current window horizontally with a new buffer. If you want to split it vertically, you should use `:vnew`.

## Use Tabs efficiently

You can read this Wiki [page](https://vim.fandom.com/wiki/Quick_tips_for_using_tab_pages) for more tips.

### Open help page in a new tab

Let's say you want to read about windows but you don't want to pollute the current tab:

```
:tab help windows
:tab h windows
```

### Closing a tab

You can use `:tabclose` to close a tab page with all its windows.


### Differences between current buffer and the corresponding file before loading

You can use `:tab split` to open a new window on a new tab that shows the current buffer. Note that a diff view of two or more buffers is local to the tab page. Now you can use `:DiffOrig` to see the differences.

### Mini-sessions

If you remove 'tabpages' from `sessionoptions` (`:set sessionoptions-=tabpages`), then you can save the current buffer list as well as other settings using `:mksession ~/my_project.vim`. When you start Vim later, you can use `:source ~/my_project.vim` to restore it.

### Useful Links for buffers
* [Buffers](https://vim.fandom.com/wiki/Buffers)
