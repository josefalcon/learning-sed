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

Recall that `!` negates our address. So this new command reads,
"Delete every line whose line-number is not 1". We also so how to
print only the last line.

```sh
sed -n '$ p' ../lesson2/example.txt
```
```
But if you're a worm, sleep late.
```

```sh
sed '$! d' ../lesson2/example.txt
```
```
But if you're a worm, sleep late.
```

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

Deletion also supports address ranges. The following is equivalent to
the command above.

```sh
sed '1,10! d' /etc/passwd
```

## Next line, please!

One interesting command in `sed` is the `n` command. The `man` entry
for `n` reads:

> Write the pattern space to the standard output if the default output
> has not been suppressed, and replace the pattern space with the next
> line of input.

The `example.txt` file for this example is organized with words
containing 'even' appearing on even lines, and words containing 'odd'
appearing on odd lines. We'll use `sed` and the new `n` command to
display its power.

```sh
sed -e 'n' -e 'd' example.txt
```
```
odd
oddish
oddness
Odds
odds
...
```

Let's break this down. The first three lines are:

```
odd
antevenient
oddish
```

When `sed` begins processing it reads `odd` into the pattern
space. `n` is the first function to operate on the pattern
space which says, "print the pattern space, then replace the
pattern space with the next line." After `odd` is printed,
`antevenient` is put into the pattern space, and `sed` moves
on to the next command, `d`. From this lesson, we know that
`d` deletes the pattern space and starts the next cycle. Thus,
`oddish` is read into the pattern space and the cycle repeats.

Let's invert some commands.

```sh
sed -n -e 'n' -e 'p' example.txt
```
```
antevenient
Beneventan
Beneventana
bevenom
Cevennian
...
```

Unlike our previous command, this command uses the `-n` option
to turn off default printing. We've also replaced the `d` function
with the `p` function. So what is happening in this example. Like
before, `odd` is read into the pattern space, and the first function
encountered is `n`. `n` only prints if default printing is enabled,
but the existence of the `-n` option disables that behaviour, so no
printing occurs. So `odd` is removed the pattern space without
printing, and `antevenient` is placed in the pattern space. The next
command `p` forces printing. The cycle continues after that.

## Recap

- `d` is used to delete the contents of the pattern space. It
immediately begins the next cycle.
- `n` prints the pattern space unless default printing is disabled. It
replaces the contents of the pattern space with the next line.
- Don't get confused with `n` and `-n`. `-n` is an option passed to
`sed`, while `n` is a function that operates on the pattern space.