# Bandit Level 5 → Level 6

## What I learned

There were 20 directories inside `inhere`, so I didn't want to go through each directory manually.

I learned how to use the `find` command to search through directories and look for files based on certain conditions.

I checked the manual with:

```bash
man find
```

The basic structure is:

```bash
find [starting-point] [expression]
```

For this level, I used:

```bash
find . -type f -size 1033c
```

* `.` means start searching from the current directory.
* `-type f` means look for regular files.
* `-size 1033c` means look for files that are exactly 1033 bytes. `c` means bytes.

The command returned:

```text
./maybehere07/.file2
```

I then read the file with:

```bash
cat maybehere07/.file2
```

and found the password.

## Something I learned from my mistake

When I first went into `maybehere07`, I used:

```bash
ls
```

which showed:

```text
-file1  -file2  -file3 ...
```

I didn't use `ls -a`, so I didn't see the hidden `.file2`.

I then accidentally read `-file2` instead of the `.file2` that `find` had actually found. This gave me a completely different output - a long string.

The important thing I learned was to **carefully read the exact path returned by `find`**. In:

```text
./maybehere07/.file2
```

the filename starts with a `.`.

## Main takeaway

I learned how to use `find` to search through multiple directories instead of checking them one by one. I also learned that `ls` doesn't show hidden files, while `ls -a` does, and that I need to pay attention to the exact filename returned by a command.
