# Bandit Level 3

## What I learned

I used `ls` and found a directory called `inhere`.

When I tried:

```bash
cat inhere
```

it gave me an error because `inhere` is a directory, not a file. I used `cd` to enter the directory:

```bash
cd inhere
```

When I ran `ls`, nothing showed up. I learned that this doesn't always mean the directory is empty because there can be hidden files.

I used:

```bash
ls -a
```

and found:

```text
.  ..  ...Hiding-From-You
```

I learned that:

* `-a` means show all files, including hidden files.
* `.` means the current directory.
* `..` means the parent directory.

I also noticed that the file name started with **three dots**, not one.

I then used:

```bash
cat ...Hiding-From-You
```

to read the file and get the password for the next level.

## Other things I learned

I tried:

```bash
cd .
```

and stayed in the same directory because `.` means the current directory.

I also tried:

```bash
cd ..
```

which took me back to the parent directory.

I learned that I can use:

```bash
file <filename>
```

to check whether something is a file or a directory.

## Key takeaway

I learned about hidden files and how to find them using `ls -a`. I also learned the difference between `.` and `..`, and how to move between directories using `cd`.
