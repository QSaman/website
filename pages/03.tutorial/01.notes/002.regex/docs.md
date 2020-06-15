---
title: Regex
taxonomy:
    category: docs
---

# [^pattern]

It matches everything but the ones in the brackets. Compare the output of the following commands:

```
$ echo "38=2;150=Grep me if you can;40=1;" | grep -oE "150=[^;]+"
150=Grep me if you can

$ echo "38=2;150=Grep me if you can;40=1;" | grep -oE "150=.+;"
150=Grep me if you can;40=1;
```
# Lookaround

lookaround actually matches characters, but then gives up the match, returning only the result: match or no match.They do not consume characters in the string, but only assert whether a match is possible or not. For more information visit this [website](https://www.regular-expressions.info/lookaround.html).

If you want to use lookaround with `grep` you need to enable Perl regex by using `grep -P`.


## Lookahead


### Negative Lookahead

If you want to match something not followed by something else. The syntax is like `(?!pattern)` or `(?!(regex))`:

```
$ echo "int i = 2; int GrepMeIfYouCan; int j = 2;" | grep -oP "; int.+(?!= 2)"
; int GrepMeIfYouCan; int j = 2;
```

### Positive Lookahead

The syntax is like `(?=pattern)`or `(?=(regex))`

```
$ echo "192.168.0.0/16 127.0.0.0/8" | grep -oP ".+(?=/[0-9]+)"
192.168.0.0/16 127.0.0.0

$ echo "192.168.0.0/16 127.0.0.0/8" | grep -oP "[0-9.]+(?=/[0-9]+)"
192.168.0.0
127.0.0.0
```

## Lookbehind

Lookbehind has the same effect, but works backwards

### Negative Lookbehind

The syntax is `(?<!pattern)` or `(?<!(regex))`.

### Positive Lookbehind

The syntax is `(?<=pattern)` or `(?<=(regex))`.

## Lookaround Examples

```
$ echo "<h1>IP Information</h1><ISP>Bell Canada</ISP></p><City>Montreal</City>" | grep -oP "(?<=(<ISP>)).*(?=(</ISP>))"
Bell Canada
```
