# stream editor


`sed` is a text processing utility written by Lee E. McMahon while at
Bell Labs. McMahon describes `sed` as a "non-interactive context
editor designed to be especially useful in three case:"

0. To edit files too large for comfortable interactive editing;
1. To edit any size file when the sequence of editing commands is too
complicated to be comfortably typed in interactive mode.
2. To perform multiple global editing functions efficiently in one
pass through the input.

You've likely seen `sed` used in a shell script or as a part of long
shell command. In fact, complex sequences of `sed` commands can be
written in a `sed` script. It is helpful to think of `sed` as a
[programming language](http://sed.sourceforge.net/grabbag/scripts/turing.txt).

In this lesson we'll learn how `sed` works, and review a few simple
examples.

## How it Works

`sed` contains a library of functions used to manipulate text, for
example substitution or deletion. It is powered by simple,
compositional mechanisms for defining when commands should be applied.

Text processing begins by reading from an input stream, for example a
text file or the result of some program. Typically, `sed` operates on
__one__ line of input at a time. When a line of text is read it is
said to be in 'pattern space'. `sed` increments an internal
line-number counter for each line of text read. With text inside the
pattern space, `sed` applies a series of commands on the pattern space
in the order they were specified. Thus, chaining commands creates a
stream of textual changes: the result of one command becomes the input
of the next command.

In future lessons, we'll review how `sed` operates and discuss
techniques for manipulating the pattern space.

## Simple Beginnings

`sed` is invoked with the following format

```sh
$ sed [options] command file
```

We'll expand on this pattern later, but for now it will serve this
tutorial well. Let's write our first `sed` command.

```sh
$ sed '' example.txt
```

The simplest command is the empty command. In this example, we ask
`sed` to do _nothing_ with each line of text. So why do we see
output from this command?

By default, `sed` will print the contents of the pattern space before
moving on to the next line. This will be useful when we get to
substitution since it will print the modified line. For now, the
command above functions a lot like `cat`.

We can disable this behaviour with the `-n` flag.

```sh
$ sed -n '' example.txt
```

What happens? Nothing. With the `-n` flag we ask `sed` not to print
unless explicitly directed to. This introduces our first `sed` command:
`p`. The `man` documentation for `p` reads:

> Write the pattern space to standard output.

Simple enough.

```sh
$ sed -n 'p' example.txt
```

We see the print command overrides the `-n` flag. So what is
happening? `sed` begins processing example.txt by reading in
a single line of text. The first line of text
"I look to the left," is read into the pattern space, then
the `p` command is applied to the pattern space which prints
the contents to stdout. When it finishes processing, the `-n`
flag disables further printing, and `sed` moves on to the next
line of text.

What would happen if we applied the print command without `-n`?

```sh
$ sed 'p' example.txt
```

Each line is printed twice!

## Recap

- `sed` reads an input stream line-by-line into the 'pattern space'.
- A series of commands are applied to the pattern space in the order specified.
- By default `sed` prints the pattern space after processing. We can disable
this feature with the `-n` flag.
- `p` is a sed command for printing the pattern space.
