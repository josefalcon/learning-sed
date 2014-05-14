# Deletion

If you've made it this far, you've got the basics of `sed`.  From here
on out, we'll learn more `sed` functions by way of example.

Deletion is denoted by `d` and is used in a similar fashion to `p`. In
our first two lesson we approached deletion by passing the `-n` option
to `sed`. Let's see how we can use `d` in lieu of the `-n` option.

One of the first examples we saw was printing only the first line, or
deleting everything but the first line. Here was our code from
[lesson2](https://github.com/josefalcon/learning-sed/tree/master/lesson2)

```sh
sed -n '1 p' ../lesson2/example.txt
```
```
Oh, if you're a bird, be an early bird
```

If `d` is the inverse of `p`, then we should see how to easily mimic
this functionality without the `-n` flag.

```sh
sed '1! d' ../lesson2/example.txt
```
```
Oh, if you're a bird, be an early bird
```

Recall that `!` negates our address. So this new command reads like,
"Delete every line whose line-number is not 1".

What if we want to delete only the first line? That's easy; just drop
the `!`.

```sh
sed '1 d' ../lesson2/example.txt
```
```
And catch the worm for your breakfast plate.
If you're a bird, be an early bird-
But if you're a worm, sleep late.
```

A typical usage of `d` is to delete every line matching (or not) a
specific pattern. Both are easy with context addresses. The general
form is:

```sh
sed '/pattern/ d'
```

or,

```sh
sed '/pattern/! d'
```

For example, if we want remove comments from `/etc/passwd` for 
further processing, we could use `^#` as a context address. The
address finds all lines that begin with `#`.

```sh
sed '/^#/ d' /etc/passwd
```

To read about `/etc/passwd`, let's negate the address and read
only the comments.

```sh
sed '/^#/! d' /etc/passwd
```
```
##
# User Database
#
# Note that this file is consulted directly only when the system is running
# in single-user mode.  At other times this information is provided by
# Open Directory.
...
```