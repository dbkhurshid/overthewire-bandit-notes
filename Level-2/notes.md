# Bandit Level 2

## What I learned

I used the `ls` command and found a file named:

```text
--spaces in this filename--
```

At first, I tried to read it using:

```bash
cat --spaces in this filename--
```

but it didn't work because the spaces made the filename get treated as separate arguments, and `--spaces` was also interpreted as an option.

I then tried putting the filename in quotes:

```bash
cat "--spaces in this filename--"
```

This fixed the spaces, but it still didn't work because the filename starts with `--`.

I learned that `--` can be used to tell a command to stop treating what follows as options. The command that worked was:

```bash
cat -- "--spaces in this filename--"
```

I also learned that `./` means the current directory. For example, this would also work:

```bash
cat "./--spaces in this filename--"
```

## Key takeaway

I learned how spaces in filenames can affect commands and how `--` and `./` can be used when working with filenames that could be interpreted specially.
