# Multiline Editing

Up until this point we've used `sed` to operate on individual lines,
and we've equated the pattern space with an individual line in a
file. When `sed` reads a line into the pattern space, the trailing
newline character is removed. To see this, we'll try to substitute
a comma for newlines to create a single line.

```sh
sed 's/\n/,/' example.txt
```
```
asdf
```

Nothing happens. When the substitute command is applied to the pattern
space, there is no newline character to replace; `sed` has already
removed it.

Using `sed` to replace newline characters is one of the most frequently
asked questions on [stackoverflow](http://stackoverflow.com/questions/1251999/sed-how-can-i-replace-a-newline-n). In this lesson, we'll learn three new `sed`
functions for editing multiline operations. In lesson 6, we'll learn
about labels and branching which will gives us everything we need to
replace all newlines in a file.

## Line numbering with `=` and `N`.

Lesson 2 introduced the `=` command for printing the current value
of the line-number counter.

```sh
sed '=' example.txt
```
```
```

It would be nice to print the line-number on the same line as the
line itself. You might think the `n` command from the last lesson
could help us, but `n` *replaces* the current contents of the pattern
space with the next line.

The `sed` function `N` appends the next line of input to the pattern
space with a newline to separate the new text. The line-number counter
is also incremented. Notice that while similar to `n` the `N` function
does not attempt to print anything; it merely appends a line of input
with a newline character.

Before we continue, let's save the results of our last `sed` command
to a file.

```sh
sed '=' example.txt > numbered.txt
```

We'll use `numbered.txt` for simplicity. So what happens if we use
our new `N` command on our numbered file?

```sh
sed 'N' numbered.txt
```
```
```

Nothing! Again, let's try to reason like `sed` does. The first few
lines of the file are:

```
1
some line of text
2
another line of text
```

When `sed` begins processing it adds `1` to the pattern space (without
a newline), and then applies the `N` function. The effect is appending
the next line to the pattern space with a newline. The contents of the
pattern space are then:

```
1
some line of text
```

There are no further commands to execute, and because printing is not
surpressed with `-n`, the contents of the pattern space are written out.
The cycle will then repeat. Note that when the cycle repeats, and the
next line of input is read into the pattern space, the entire pattern
space is deleted.

But if the pattern space contains a newline character, we can use the
substitution command to remove it! Let's add another command to the
pipeline to replace the newline with a single space.

```sh
sed -e 'N' -e 's:\n: :' numbered.txt
```
```
some results
```

Our substitute command uses `:` for the delimiter for readability. Now
after the first two lines are in the pattern space, the substitute
command replaces the newline with a space, and then prints the pattern
space.

### Practice

Our work in the previous examples adds line numbers to empty lines.
Write a `sed` command to only write line numbers on non-empty lines.



## Recap

- The `N` command appends the next line of the input stream to the pattern
space with a newline. It does not attempt to print.
- `P`
- `D`