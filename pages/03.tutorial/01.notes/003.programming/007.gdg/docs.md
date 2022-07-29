---
title: GDB
taxonomy:
    category: docs
---

## Tips

First make sure the symbols are in the execution. For GCC you should use `-g` flag. For letting compiler to generate more warnings, use `-Wall` flag.

```
g++ -Wall -g file.cpp
gdb a.out
gdb --args a.out arg1 arg2
```

## Setting the arguments inside GDB

```
set args arg1
```

### Seeing the source code (TUI - Text User Interface)

```
gdb -tui
layout src
layout asm
layout reg
```

### Enable/Disable TUI

```
tui enable
tui disable
```

By default it's empty. You need to run the program to see the code. You can press arrow up or down to scroll the source code.

### Run a program

```
run
```

If you enter `run` again, it restart the execution.

### Stack trace

```
backtrace full
```

### Next Statement

```
next
n
```

### Step into a function

```
step
```

### Finish current function

```
finish
```

### Set a breakpoint

```
break line_number
break function_name
```

### List breakpoints

```
info break
```

### Clear a breakpoint

```
clear line_number
clear function_name
```

### Watch a variable

```
watch variable
```

### Remove a watch

First run

```
info watchpoint
```

To see watch number, then

```
delete watch_number
```

### Refresh the screen

```
refresh
```

You can also use `CTRL+L` shortcut

### Continue to next breakpoint

```
continue
```

### Print variables

Let's assume we have:

```
vector<int> v{1, 2, 3};
int a1[3] = {1, 2, 3};
int* a2 = int[3]{1, 2, 3};
```

```
print variable
print v[0]
print v
print a1
print *a2@3
```

## References

* [GDB Tutorial](https://www.youtube.com/watch?v=bWH-nL7v5F4)
