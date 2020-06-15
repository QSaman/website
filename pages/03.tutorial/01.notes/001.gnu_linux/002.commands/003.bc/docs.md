---
title: bc
taxonomy:
    category: docs
---

`bc` is a powerful calculator that you can run in terminal.

# Interactive mode
You can run `bc -i` or `bc` to run in interactive mode:

```
$ bc -i
2 * 3
6
quit

$ bc
2^3; 2^4
8
16
CTRL+D
```

# Using Previous Result

```
$ bc -i
2 + 2
4
. * 2
8
```
 
# Base conversion

## Converting from base 2 to 16

```
$ echo 'obase=16; ibase=2; 11110000' | bc
F0
```

Note that when we use `ibase=2` then everything after it is interpreted in base 2:

```
$ echo 'ibase=2; obase=10000; 11110000' | bc
F0
```

# Mathematical Operations

```
$ echo '10/3' | bc
3
$ echo '4%3' | bc
1
```

For fractional division use `scale` variable or run `bc -l`:

```
$ echo 'scale=2; 10/3' | bc
3.33
$ echo '10/3' | bc -l
3.33333333333333333333
```
For other operations:

```
$ echo '2^100' | bc
1267650600228229401496703205376
$ echo '3+4*2' | bc
11
$ echo '(3+4)*2' | bc
14
```

# Mathematical Functions

```
$ echo 'sqrt(100)' | bc
```

To call other mathematical functions run `bc -l` (`l()` is natural logarithm):

```
$ echo 'l(100)/l(10)' | bc -l
2.00000000000000000000
$ echo 'l(1024)/l(2)' | bc -l
10.00000000000000000010
```
To calculate Pi (`a()` is arctangent and `s()` is sine functions):

```
$ pi=$(echo '4*a(1)' | bc -l)
$ echo "s($pi/2)" | bc -l
1.0000000000
```
You can achieve the above result in interactive mode:

```
bc -il
pi=4*a(1)
s(pi/2)
1.00000000000000000000
```
