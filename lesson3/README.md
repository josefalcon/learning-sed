# Substitution

All `sed` functions are denoted by single characters. Substitution is
one of the most frequently used functions and is marked by `s`.

The format for substitution is

```
s<delimiter><pattern><delimiter><replacement><delimiter><flags>
```

Every substitution command has exactly 3 occurences of a delimiter.
Typically, the delimiter used is `/`, but it can be any character
except space or newline. For readability, it is best to use a
delimiter not found in `<pattern>`.

```sh
sed 's///' example.txt
```
```
```

Suppose our pattern contains `/` as a character. We can use a different
delimiter to improve readability.

```sh
sed 's:/home/path:/something/else/' paths.txt
```
```
```

Or

```sh
sed 's_/home/path_/something/else_' paths.txt
```
```
```

By default, substitute only substitues the first match of
`<pattern>` per line.

```sh
TODO: example where the first occurence happens on multiple lines.
```
```
```

We'll learn how to replace all occurences in the Flags section.

## Special Characters

### &

The `<replacement>` text is not a pattern, though it does support
a few special characters. An `&` character in the replacement text
is replaced by the entire string matching the pattern.

```sh
TODO: example of &
```
```
```

Like all special replacement characters, this behaviour can be escaped
with a backslash, `\&`.

```sh
TODO: example of \&
```
```
```

### \n

An occurence of `\n`, where `n` is a digit, is replaced by the <i>n</i>th
substring match of the given pattern. This is called a 'backreference
expression'.

```sh
TODO: example of \n
```
```
```

## Flags

There are four possible flags for substitute.

The global flag, `g`, substitutes for **all** matches of pattern. As
the previous section mentioned, by default, `sed` only substitutes
the first occurence of the match.

```sh
TODO: example of g
```
```
```

If there is a specific occurence of the pattern we'd like to replace, use
a single digit.

```sh
TODO: example of 1/2/3
```
```
```

Recall our print function `p`. We can use a `p` flag with substitute to
print the pattern space if and only if a replacement was made.

```sh
TODO: example of p
```
```
```

The last flag, `w`, functions like `p`, but writes to a file. If the
given file exists, it is overwritten, otherwise it is created.

```sh
TODO: example of w
```
```
```

## Addresses

TODO