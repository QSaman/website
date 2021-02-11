---
title: Vim
taxonomy:
    category: docs
---

# Plugin Tips

## NERDTree

If you want to synchronize the current working directory of NERDTree with open file/buffer, the following command.

```
:NERDTreeFind
```

# Tips

* Add the following line to your `~/.vimrc` to access `man` in Vim. For example `:Man mpv`

```
runtime ftplugin/man.vim
```

* When you are in `command mode` (using `:`) enter `CTRL-f` to open command-line window.
* You can yank in `command mode`. For example `:%y 0` copy the entire buffer into `0` register
* You can open a terminal in Vim by running `:terminal`.

# Search tips

If you want to disable highlighted search text, run `:nohls` or `:nohlsearch`. If you try to search for a new word, it will not be highlighted so you should  enable it by running `:hls` or `:hlsearch`. A better approach is to empty search register by running `:let @/=''` without touching the highlight variable.

## Searching in multiple files

You can use [ack](https://github.com/beyondgrep/ack2) or [ag](https://github.com/ggreer/the_silver_searcher) without a plugin. The author of the latter claims that it's faster. To run `ag` and fetching its output in `QuickFix` list you can run:

```
:cexpr system('ag search_keyword')
```

After that you can run `:copen` to see the output in `QuickFix` list. If you press `enter` key on any file, it will be opened in a new window. If you want to add more result to the same `QuickFix` list, you can run `:caddexpr system('ag search_keyword_2`)`. If you are searching for special characters you can use the following command (note that the dot means concatenation):

```
:cexpr system('ag ' . shellescape("1'000'000"))
```

# Moving around

For more information in Vim type `:h motion.txt`

## `e` and `E`

* `e` move to the end of a word
* `E` move to the end of a word (any non-whitespace characters)

Suppose the cursor is at the beginning of `std::cout <<`. `e` stops at `d` and `E` stops at `t`.

## `w` and `W`

* `w` move forward to the beginning of a word
* `W` move forward a word (any non-whitespace characters)

Suppose the cursor is at the beginning of `std::cout <<`. `w` stops at `:` and `W` stops at `<`.

## `b` and `B`

* `b` move backward to the beginning of a word
* `B` move backward to the beginning of a word (any non-whitespace characters)

## `ge` and `gE`

* `ge` move backward to the end of a word
* `gE` move backward to the end of a word (any non-whitespace characters)

## `0` and `^` and `$`

* `0` Move to the beginning o a line
* `^` Move to the first non-blank character of the line
* `$` Move to the end of the line

## `H` (Home)  and `M` (Middle) and `L` (Last)
* `H` jump to the top of screen
* `M` jump to the middle of screen 
* `L` jump to the bottom of screen

## `CTRL+u` and `CTRL+d` and `CTRL+f` and `CTRL+b`

* `CTRL+d` move 16 lines down
* `CTRL+u` move 16 lines up
* `CTRL+f` move one page down
* `CTRL+b` move one page up

## `z+enter` and `z+.` and `z+-`

* `z+enter` move the current line to the top of screen
* `z+.` move the current line to the middle of screen
* `z+-` move the current line to the bottom of screen
* `50z+enter` makes the top of screen starts at line 50

# Clipboard Access in Terminal

You can run the following command to figure out if it's available:

```
vim --version | grep clipboard
```

Inside Vim you can run `:echo has('clipboard')` to see if it's available. Most Linux distros ship with a "minimal" Vim build by default which doesn't have `+clipboard`. but you can usually install it:

## Fedora

Install `vim-x11` and run `vimx` instead of `vim`. You can add the following line to your `~/.bashrc`:

```
alias vim='vimx'
```
## Debian and Ubuntu

Install `vim-gtk` or `vim-gnome`.


# Copy and pasting

## Pasting from OS Clipboard

When you are in `insert mode` or `command mode` press `CTRL-r *` or `CTRL-r +` for pasting from OS clipboard. Of course you can also use Vim registers (e.g. `CTRL-r a`)

## Fast Paste

You can quickly exit `insert mode` for a single `normal mode` operation with `CTRL-o`. For example if you are in `insert mode` and want to quickly paste, you can press `CTRL-o p`

# Vim Registers

For more information type `:h registers` in Vim.

## See the content of all registers

Type `:reg` in Vim. If you only want to see the content of registers a, b and c you must run `:reg a b c` in Vim.

## Numbered Registers

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

## Modifying the content of a register

Suppose you yanked a text (as you know it stored in `0` register). You can modify `0` register before pasting by running the following command: `:let @0='CTRL-r 0'`. Then the content of `0` register appears and you can modify it.

## Read-only Registers

### `.` register

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

### `%` register

It has the current file address. Suppose we want to copy the current file path to OS clipboard. We need to run `:let @+=@%`.


### `:` register

It has the content of last executed command. In command mode you can run `:@:` to rerun the last command.

### `=` register

The expression register is used to deal with result of expressions. For example if you are in `insert mode` and you type `CTRL-r =` you will see a `=` sign in the command line. Then you type `2+4*3<enter>`, 14 will be inserted

### `/` register

The search register has the content of the last search.


# Macros

Type `:recording` for more information.

## Run a macro from clipboard

Suppose you copied the content of a macro in OS clipboard. You can run it by `@+`. For example copy "iVim is awesome" into your OS clipboard and then in `normal mode` enter `@+`.

# Windows

For more information run `:h windows.txt`.

## Splitting Window

### Horizontal Splitting

* By pressing `CTRL-w s` `in normal mode`
* By running `:split` in `command mode`

### Vertical Splitting

* By pressing `CTRL-w v` in `normal mode`
* By running `:vsplit` in `command mode`

## Closing a Window

* By pressing `CTRL-w q` in `normal mode`
* By running `:q` in `command mode`

## New Window

You can create a new window using `:new` which will be opened horizontally or `:vnew` which will be opened vertically

## Moving to the top-left window

Press `CTRL-w t` in `normal mode`

## Moving to the bottom-right window

Press `CTRL-w b` in `normal mode`

## Changing the Layout of Windows

### `CTRL-w K`

Note that `K` is uppercase. Move the current window to be at the very top, using the full width of screen. In other words if we have two vertically-splitted Windows, this command makes them horizontally splitted.

### `CTRL-w J`

Note that `J` is uppercase. Move the current window to be at the very bottom, using the full width of screen. In other words if we have two vertically-splitted Windows, this command makes them horizontally splitted.

### `CTRL-w H`

Move the current window to be at the far left, using the full height of the screen. In other words if we have two horizontally-splitted windows, this command makes them vertically splitted.

### `CTRL-w L`

Move the current window to be at the far right, using the full height of the screen. In other words if we have two horizontally-splitted windows, this command makes them vertically splitted.

### `CTRL-w T`

If we have more than one window in the current tab, moves the current window to a new tab.

## `CTRL-w o`

Make the current window the only one on the screen. All other windows are closed.


# Buffers

Use `:buffers` or `:ls` to see the list of buffers. For example if you have multiple windows and you press `CTRL-w o` to close all windows except the current one, Vim add all the closed ones to buffer list. You can reopen them using `:buffer`. The argument is buffer number or buffer name. You can also open a buffer using `N CTRL-^` which `N` is buffer number.

# Search in files

For more information type `:h vimgrep`. 

For example you can type `:vimgrep /while/ **/*.cpp` to search for `while` recursively (`**` means search recursively). To see the file list type `:copen`.


# Book Review

## Group 1:
```
i, I
a, A
s, S
```        
## Group 2:
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

 ## Deleting an HTML tag  

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


# Programming Tips (visit this [site](http://www.moolenaar.net/habits.html))

* Use `%` to jump from an open brace to its matching closing brace. Or from a `"#if"` to the matching `"#endif"`. Actually, `%` can jump to many different matching items. It is very useful to check `if ()` and `{}` constructs are balanced properly.
* Use `[{` to jump back to the `"{"` at the start of the current code block.
* Use `gd` to jump from the use of a variable to its local declaration.
* Vim has a completion mechanism that makes this a whole lot easier. It looks up words in the file you are editing, and also in #include'd files. You can type `"XpmCr"`, then hit `CTRL-N` and Vim will expand it to
* ":abbr Lunix Linux" The words will be automatically corrected just after you typed them.
# Tips

* If you are using [Ack](https://github.com/mileszs/ack.vim) plugin, you can use the following command:

```
:Ack -w -i --ignore-dir=build -w  \"gui\"
```
`-w` means search for the whole word, `-i` means ignore case sensitivity.

# Plugins

## YouCompleteMe

* Press `\d` to see error detail
* If you see literal strings are highlighted. It is likely that you ran `set spell`. The solution is `set nospell`.

# Book Review

## Group 1:
```
i, I
a, A
s, S
```        
## Group 2:
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

 ## Deleting an HTML tag  

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


# Programming Tips (visit this [site](http://www.moolenaar.net/habits.html))

* Use `%` to jump from an open brace to its matching closing brace. Or from a `"#if"` to the matching `"#endif"`. Actually, `%` can jump to many different matching items. It is very useful to check `if ()` and `{}` constructs are balanced properly.
* Use `[{` to jump back to the `"{"` at the start of the current code block.
* Use `gd` to jump from the use of a variable to its local declaration.
* Vim has a completion mechanism that makes this a whole lot easier. It looks up words in the file you are editing, and also in #include'd files. You can type `"XpmCr"`, then hit `CTRL-N` and Vim will expand it to
* ":abbr Lunix Linux" The words will be automatically corrected just after you typed them.
# Tips

* If you are using [Ack](https://github.com/mileszs/ack.vim) plugin, you can use the following command:

```
:Ack -w -i --ignore-dir=build -w  \"gui\"
```
`-w` means search for the whole word, `-i` means ignore case sensitivity.

# Plugins

## YouCompleteMe

* Press `\d` to see error detail
* If you see literal strings are highlighted. It is likely that you ran `set spell`. The solution is `set nospell`.
