# Addresses

We learned in lesson1 that `sed` operates line-by-line on an entire
input stream. As each line is processed, it is put into the pattern
space. Processing continues until the end of the input-stream.

In this lesson we'll learn about `sed`s mechanism for limiting what
lines of text are processed.

## Filtering with Addresses

It is often unnecessary to apply `sed` commands to an entire
file. Perhaps we know something of the structure of the file and only
need to apply commands to specific lines, or to a range of lines.
Addresses allow us to target lines in a stream for editing.

With `sed`, we can use two types of addresses:

- line-number
- context

### Line-number Addresses

The simplest type of address is the line-number address. `sed`
maintains an internal line-number counter (initialized to 0), and
increments it each time a line is read from the input stream.
If we imagine each line in a input stream as numbered from 1 to `n`,
then a line-number address functions like an array index: it applies
the `sed` command to a particular line in a file.

We can specify an addressed command with the following format:

```sh
[address1] <function>
```

Let's use the `p` function we learned in the previous lesson with an
address.

```sh
sed -n '1 p' example.txt
```
```
Oh, if you're a bird, be an early bird
```

Again, we use the `-n` flag to disable automatic printing, and use the
`p` function to explicitly tell `sed` when to print a pattern space.
In this example we tell `sed` to only apply the `p` function when the
line-number counter is equal to 1. We can use any address we want, so
long as it is an integer.

```sh
sed -n '3 p' example.txt
```
```
If you're a bird, be an early bird-
```

Notice that if we specify an address that exceeds the total number of
lines we get no output.

```sh
sed -n '30 p' example.txt
```

`sed` uses `$` as an alias for the last line of the input stream.

```sh
sed -n '$ p' example.txt
```
```
But if you're a worm, sleep late.
```

### Context Addresses

Context addresses are regular expression patterns. A context address
matches if the regular expression matches the pattern space. Unlike
line-number addresses, context addresses can match multiple lines.

Let's write a `sed` command for applying the print function to all
lines that contain 'bird'. We'll use the same pattern we used for
line-number addresses:

```sh
sed -n '/bird/ p' example.txt
```
```
Oh, if you're a bird, be an early bird
If you're a bird, be an early bird-
```

Notice that context addresses are contained within slashes. Seeing the
results is useful, but what if we want to know the line numbers
affected by this command. We'll use the `=` function, which prints the
line-number instead of the line contents.

```sh
sed -n '/bird/ =' example.txt
```
```
1
3
```

Let's run a similar command for 'worm'.

```sh
sed -n '/worm/ =' example.txt
```
```
2
4
```

Context addresses are regular expressions so they can be more complex.

```sh
sed -n '/b.*l/ p' example.txt
```
```
1
2
3
```

### Practice

- Using the `uuids.txt` file, write a `sed` command to find the uuid
containing 'D2C4'.
- On what line number does that appear?
- What are the 10 uuids following it?
- The `=` command works with line-number addresses too. Write a `sed`
command that returns the total number of lines in a file. Compare
your function with `wc -l`.

## Address Ranges

Two addresses define an address range. For line-number address, the
range is as you expect: it matches the first line-number and applies
the script until (and including) the next line-number address.

```sh
sed -n '2,3 p' example2.txt
```
```
  # This is an extra line.
And catch the worm for your breakfast plate.
```

The format for specifying a range of addresses is

```sh
[address1[,address2] <function>
```

Context address ranges work like line-number address. The script is
applied to the first line that matches the first address. The script
is then applied to all lines until (and including) the __next__ line
which matches the second address. After the second address is matched
another attempt is made on subsequent lines to match the first
address. This process continues until the end of the stream.

```sh
sed -n '/bird/,/worm/ p' example2.txt
```
```
Oh, if you're a bird, be an early bird
  # This is an extra line.
And catch the worm for your breakfast plate.
If you're a bird, be an early bird-
  # One more line captured.
But if you're a worm, sleep late.
```

Be sure to read the example2 file carefully. You'll see that some
lines are excluded.

Note that matching the second address always happens on subsequent
lines. The following is legal.

```sh
sed -n '/late/,/late/ p' example2.txt
```
```
And catch the worm for your breakfast plate.
  # This line won't always be captured.
If you're a bird, be an early bird-
  # One more line captured.
But if you're a worm, sleep late.
```

We can mix line-number and context address. Lets print all the lines
starting from the first comment to the end of the file.

```sh
sed -n '/#/,$ p' example2.txt
```
```
  # This is an extra line.
And catch the worm for your breakfast plate.
  # This line won't be captured.
If you're a bird, be an early bird-
  # One more line captured.
But if you're a worm, sleep late.
```

Not all commands accept addresses, and not all commands accept address
ranges. Be sure to read the documentation on commands for more
information.

## Recap

- Addresses define when commands should be applied.
- There are two kinds of addresses: line-number and context.
- If no address is specified, the commands are applied to every line.
- Two addresses define an address range.