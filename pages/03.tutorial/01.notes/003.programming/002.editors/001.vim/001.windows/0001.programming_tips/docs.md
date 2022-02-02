---
title: Programming Tips
taxonomy:
    category: docs
---

## Special windows

You can use special windows in Vim to improve your experience during programming.

### Quick fix window

It's a global window. So all tabs and windows sharing the same quick fix window. For more information run `:h quickfix.txt`. You can put data in it using command like `:make`, `:grep` and `:vimgrep`. You can use [The Silver Searcher (ag)](https://github.com/ggreer/the_silver_searcher) as a back-end for `:grep` by adding the following lines into your `~/.vimrc`:

```
if executable("ag")
    set grepprg=ag\ --vimgrep\ $*
    set grepformat=%f:%l:%c:%m
endif
```
Now you can use `ag` inside vim. For example you can search for "vector" in all C++ header and source files using `:grep vector --cpp`. You can use the following commands to use quick fix window:

* `:cwindow`: open quick fix window
* `:cfirst`: jump to the first entry
* `:clast`: jump to the last entry
* `:cnext`: jump to the next entry
* `:cprevious`: jump to the previous entry

when quick fix window is opened you can open the file in a **new window** using `CTRL+ENTER`.

### Location window

It's a window-local quick fix window. Let's say you have two projects `~/src/foo` and `~/src/bar` and you are working on `foo`. But you need to look for something on `bar`. You open `bar` on a new tab in Vim. You can even change the working directory for this tab using `:tcd` command. Now you want to search for something in `bar` but you don't want to modify quick fix that is used by `foo`. You can use location list using `:lgrep` command. Now you can open location window using `:lwindow`. The list of some commands:

* `:lgrep`
* `:lvimgrep`
* `:lmake`
* `:lwindow`
* `:lfirst`
* `:llast`
* `:lnext`
* `:lprevious`

For more information run `:h location-list-window`

### Preview window

If you are using a plugin like [YouCompleteMe](https://github.com/ycm-core/YouCompleteMe) you've seen it when you are looking at a function signature. You've noticed it doesn't gain focus automatically. Also we only have one preview window per tab. Some useful commands:

* `:pclose` or `CTRL-w+z`: close preview window
* `CTRL-w+P` (note that `P` is a capital letter): go to preview window

For more information run `:h preview-window`
