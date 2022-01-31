---
title: Vim
taxonomy:
    category: docs
---

## Help

### Help suggestions

You can type `:h reg` and then press `CTRL+D` to see all subjects that contain "reg". for more information type `:h :h`.

### Navigating a help file

You can use `CTRL+]` to follow a tag. You can use `CTRL+o` or `CTRL+t` to return back. If you change your mind you can use `CTRL+i` to undo the return back.

## Plugins

### NERDTree

If you want to synchronize the current working directory of NERDTree with open file/buffer, the following command.

```
:NERDTreeFind
```

### YouCompleteMe

* Press `\d` to see error detail
* If you see literal strings are highlighted. It is likely that you ran `set spell`. The solution is `set nospell`.

## Reading Man pages in Vim

* Add the following line to your `~/.vimrc` to access `man` in Vim. For example `:Man mpv`

```
runtime ftplugin/man.vim
```

## Command-line window
 When you are in `command mode` (using `:`) enter `CTRL-f` to open command-line window.

## Yank the entire buffer into a register
 You can yank in `command mode`. For example `:%y 0` copy the entire buffer into `0` register

## Open a terminal in Vim
 You can open a terminal in Vim by running `:terminal`.

## Search tips

If you want to disable highlighted search text, run `:nohls` or `:nohlsearch`. If you try to search for a new word, it will not be highlighted so you should  enable it by running `:hls` or `:hlsearch`. A better approach is to empty search register by running `:let @/=''` without touching the highlight variable.

### Searching in multiple files

You can use [ack](https://github.com/beyondgrep/ack2) or [ag](https://github.com/ggreer/the_silver_searcher) without a plugin. The author of the latter claims that it's faster. To run `ag` and fetching its output in `QuickFix` list you can run:

```
:cexpr system('ag search_keyword')
```

After that you can run `:copen` to see the output in `QuickFix` list. If you press `enter` key on any file, it will be opened in a new window. If you want to add more result to the same `QuickFix` list, you can run `:caddexpr system('ag search_keyword_2`)`. If you are searching for special characters you can use the following command (note that the dot means concatenation):

```
:cexpr system('ag ' . shellescape("1'000'000"))
```

## Moving around

For more information in Vim type `:h motion.txt`

### `e` and `E`

* `e` move to the end of a word
* `E` move to the end of a word (any non-whitespace characters)

Suppose the cursor is at the beginning of `std::cout <<`. `e` stops at `d` and `E` stops at `t`.

### `w` and `W`

* `w` move forward to the beginning of a word
* `W` move forward a word (any non-whitespace characters)

Suppose the cursor is at the beginning of `std::cout <<`. `w` stops at `:` and `W` stops at `<`.

### `b` and `B`

* `b` move backward to the beginning of a word
* `B` move backward to the beginning of a word (any non-whitespace characters)

### `ge` and `gE`

* `ge` move backward to the end of a word
* `gE` move backward to the end of a word (any non-whitespace characters)

### `0` and `^` and `$`

* `0` Move to the beginning o a line
* `^` Move to the first non-blank character of the line
* `$` Move to the end of the line

### `H` (Home)  and `M` (Middle) and `L` (Last)
* `H` jump to the top of screen
* `M` jump to the middle of screen 
* `L` jump to the bottom of screen

### `CTRL+u` and `CTRL+d` and `CTRL+f` and `CTRL+b`

* `CTRL+d` move 16 lines down
* `CTRL+u` move 16 lines up
* `CTRL+f` move one page down
* `CTRL+b` move one page up

### `z+enter` and `z+.` and `z+-`

* `z+enter` move the current line to the top of screen
* `z+.` move the current line to the middle of screen
* `z+-` move the current line to the bottom of screen
* `50z+enter` makes the top of screen starts at line 50

## Clipboard Access in Terminal

You can run the following command to figure out if it's available:

```
vim --version | grep clipboard
```

Inside Vim you can run `:echo has('clipboard')` to see if it's available. Most Linux distros ship with a "minimal" Vim build by default which doesn't have `+clipboard`. but you can usually install it:

### Fedora

Install `vim-x11` and run `vimx` instead of `vim`. You can add the following line to your `~/.bashrc`:

```
alias vim='vimx'
```
### Debian and Ubuntu

Install `vim-gtk` or `vim-gnome`.


## Copy and pasting

### Pasting from OS Clipboard

When you are in `insert mode` or `command mode` press `CTRL-r *` or `CTRL-r +` for pasting from OS clipboard. Of course you can also use Vim registers (e.g. `CTRL-r a`)

### Fast Paste

You can quickly exit `insert mode` for a single `normal mode` operation with `CTRL-o`. For example if you are in `insert mode` and want to quickly paste, you can press `CTRL-o p`

## Vim Registers

For more information type `:h registers` in Vim.

### See the content of all registers

Type `:reg` in Vim. If you only want to see the content of registers a, b and c you must run `:reg a b c` in Vim.

### Unnamed register `""`

It has the content of the last **modified** register. If you don't specify register name in `yank` and `put` commands (e.g. `yy`) the unnamed register is used.

### Numbered Registers

Vim store the content of last yanked text (copied text) into `0` register (you can access it by `"0`). Vim uses registers `1` to `9` to store the last deleted operation. `1` have the most recent one and `9` has the oldest one. Suppose we have a file with the following content:

```
line 1
line 2
line 3
line 4
line 5
line 6
line 7
line 8
line 9
yanked text
```
We yanked the last line (yanked text) and then delete other lines starting with "line 1" (the last deleted line will be "line 9"). The content of registers are:

```
"" line 9
"0 yanked text
"1 line 9
"2 line 8
"3 line 7
"4 line 6
"5 line 5
"6 line 4
"7 line 3
"8 line 2
"9 line 1
```

### Small delete register `"-`

If you delete less than one line, Vim uses this register instead of numbered registers (e.g. `dw`).

### Named registers `"a` to `"z` or `"A` to `"Z`

Vim uses these registers if you specify them explicitly (e.g. `"ayy`). If you use lowercase letters, the previous content of named register is replaced. If you use uppercase letters, the new content is appended. If global variable `cpoptions` has character `>`, then a new line character is added before appending the new content. You can add it using `:set cpoptions+=>`.

### Modifying the content of a register

Suppose you yanked a text (as you know it stored in `0` register). You can modify `0` register before pasting by typing `:let @0=` then press `CTRL-r` and finally `0` to put the content of `0` register. Now you can modify it and then press enter to save the change.

### Read-only Registers

#### `.` register

It has the content of last inserted text. For example suppose we enter these two sentences and then delete the last sentence:

```
The Sith rely on their passion for their strength. They think inward only about themsevles.
```

So before exiting the `insert mode` the content is:

```
The Sith rely on their passion for their strength.
```
The content of `".` register is:

```
The Sith rely on their passion for their strength. They think inward only about themsevles.<80>kb<80>kb<80>kb<80>kb<80>kb<80>kb<80>kb<80>kb<80>kb<80>kb<80>kb<80>kb
```
As you can see we have two sentences plus backspace characters to delete the second one.

#### `%` register

It has the current file address. Suppose we want to copy the current file path to OS clipboard. We need to run `:let @+=@%`.


#### `:` register

It has the content of last executed command. In command mode you can run `:@:` to rerun the last command.

#### `=` register

The expression register is used to deal with result of expressions. For example if you are in `insert mode` and you type `CTRL-r =` you will see a `=` sign in the command line. Then you type `2+4*3<enter>`, 14 will be inserted

#### `/` register

The search register has the content of the last search.


## Macros

Type `:recording` for more information.

### Run a macro from clipboard

Suppose you copied the content of a macro in OS clipboard. You can run it by `@+`. For example copy "iVim is awesome" into your OS clipboard and then in `normal mode` enter `@+`.

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

## Search in files

For more information type `:h vimgrep`. 

For example you can type `:vimgrep /while/ **/*.cpp` to search for `while` recursively (`**` means search recursively). To see the file list type `:copen`.


## Book Review

### Group 1:
```
i, I
a, A
s, S
```        
### Group 2:
```
r, R
c, C, cc
d, dd, D
```
---

* `f[char], F[char], t[char], T[char]` page 1
* `2f, 20k`
* `CTRL-G` p19
* `CTRL-D, CTRL-U` p20
* `d3$, 3dd`
* `3d2w p21`
* `cc C` p22
* `D`
* p22 (The `.` command repeats the last change. A change, in this context, is inserting, deleting or replacing text)  

 ### Deleting an HTML tag  

You position the cursor on the first `<` and delete the `<B>` with the command `df>`.

---
* `J` p23
* `r` and `s`
* `5r*` p23
* `5r<ENTER>` p23
* `~, 2~` p24
## p24 Keyboard Macros
`qa <some actions> q`. For using macro three times: `3@a`.
* [Repeat macro recursively](http://vim.wikia.com/wiki/Record_a_recursive_macro):
```
qqq
qq
Commands you want to record
@q
q
@q
```     
* *digraphs:* for example type `CTRL-kCo` for copyright sign
* In search patterns, `"foobeep\&...beep"` matches `foobeep`. `foobeep\&..."` matches `"foo"` in `"foobeep`. see :h pattern
* `:set hlsearch`. `:nohlsearch`. :set `incsearch` p29
* `/` and `?` and `n` and `N` and `/<ENTER>` and `?<ENTER>`
* `/^$ p33`
* `/. p33`
24. **IMPORTANT** see `:h magic`. if you use `"/\V<pattern>"`, you only need to escape `'/'` and `'\'` character with `\` (e.g. `/\V\/` and `/\V\\`) in `<pattern>`; but if you use `"?\V<pattern>"` you only need to escape `'\'` with `'\'` (e.g. `?\V\\`) in `<pattern>`
* `:g/^#/d` Delete all lines that begin with `'#'` character. For more information see this [page](http://vim.wikia.com/wiki/Delete_all_lines_containing_a_pattern)
* `:for i in range(1, 12) | put = printf('%d.', i) | endfor`


## Programming Tips (visit this [site](http://www.moolenaar.net/habits.html))

* Use `%` to jump from an open brace to its matching closing brace. Or from a `"#if"` to the matching `"#endif"`. Actually, `%` can jump to many different matching items. It is very useful to check `if ()` and `{}` constructs are balanced properly.
* Use `[{` to jump back to the `"{"` at the start of the current code block.
* Use `gd` to jump from the use of a variable to its local declaration.
* Vim has a completion mechanism that makes this a whole lot easier. It looks up words in the file you are editing, and also in #include'd files. You can type `"XpmCr"`, then hit `CTRL-N` and Vim will expand it to
* ":abbr Lunix Linux" The words will be automatically corrected just after you typed them.
