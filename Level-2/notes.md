# Bandit Level 2

## What I learned

I used the `ls` command and found that the filename was `-`.

At first, I tried:

```bash
cat -
```

but it did not read the file. The cursor just kept waiting.

I learned that `-` can have a special meaning in Linux commands, so `cat` was treating it as standard input instead of the filename.

I then used:

```bash
cat ./-
```

The `./` means the current directory, so `./-` tells `cat` to read the file named `-` from the current directory.

## Key takeaway

I learned that filenames can have special characters such as `-`, and using `./` can specify that I am referring to a file in the current directory.
