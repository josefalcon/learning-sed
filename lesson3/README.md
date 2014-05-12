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

Let's make a copy of the first 50 uuids from the `uuids.txt` file
from lesson 2. We can use `sed` to do that!

```sh
sed -n '1,50 p' ../lesson2/uuids.txt > uuids.txt
```

The uuids in our new file uses `-` to separate groups of characters in
the uuid. To replace `-` with `:` we use the `s` function.

```sh
sed 's/-/:/' uuids.txt
```
```
7BDB8954:BED2-4EE3-ACDE-62362CDFB205
2F413D1F:88AD-4C0A-A7E7-C286D7E49AD0
7D4F240B:D926-424D-89E0-0352FEBD91CA
...
```

Notice that only the first occurence of `-` on each line is
replaced. By default, `s` only replaces the first occurence of each
match. We can use the `g` flag to replace all matches on a line.

```sh
sed 's/-/:/g' uuids.txt
```
```
7BDB8954:BED2:4EE3:ACDE:62362CDFB205
2F413D1F:88AD:4C0A:A7E7:C286D7E49AD0
7D4F240B:D926:424D:89E0:0352FEBD91CA
```

Suppose we want to replace `-` with `/foo/bar/`. The use of slashes
in our replacement text makes the command difficult to read.

```sh
sed 's/-/\/foo\/bar\//g' uuids.txt
```

Forward slashes might appear in the pattern if we are working with
file paths.

TODO: would like something more practical here.

```sh
sed 's/\/User\/falcon\/projects/\/User\/guest\/projects/' 
```

When the pattern or replacement text includes an instance of the
delimiting character it must be escaped with a backslash, thus
polluting the command. To improve readability we can use a different
delimiting character.

```sh
sed 's:/User/falcon/projects:/User/guest/projects:' 
```

Or with underscores

```sh
sed 's_/User/falcon/projects_/User/guest/projects_' 
```


```sh
sed 's_-_/foo/bar/_g' uuids.txt
```
```
7BDB8954/foo/bar/BED2/foo/bar/4EE3/foo/bar/ACDE/foo/bar/62362CDFB205
2F413D1F/foo/bar/88AD/foo/bar/4C0A/foo/bar/A7E7/foo/bar/C286D7E49AD0
7D4F240B/foo/bar/D926/foo/bar/424D/foo/bar/89E0/foo/bar/0352FEBD91CA
...
```

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