# Bandit Level 9 → Level 10

## Goal

The password for the next level is stored in `data.txt` and is one of the few human-readable strings, preceded by several `=` characters.

## What I tried

First, I checked the file:

```bash
cat data.txt
```

The output wasn't useful because the file contained a lot of unreadable/binary-looking characters.

I remembered that `strings` can extract readable text from files, so I tried:

```bash
strings data.txt
```

This gave me a lot of readable strings, but there were still too many results to easily identify the password.

So I used `grep` to look for lines containing `=`:

```bash
strings data.txt | grep "="
```

This gave me:

```text
\========== the
q:=V
K       *=
=zZw
w=-1
vc.=
========== password
```

The line with several `=` characters followed by `password` matched the description from the challenge, so I used the value from that line as the password for the next level.

## What I learned

### `strings`

`strings` is useful when working with files that contain binary or non-readable data. It searches the file and displays sequences of readable characters.

### `grep`

`grep` lets me search for specific text in command output. In this case, I searched for `=` because the challenge said the password was preceded by several `=` characters.

### Combining commands with `|`

I used:

```bash
strings data.txt | grep "="
```

The `|` (pipe) takes the output from the command on the left and sends it to the command on the right.

So in this case:

```text
data.txt → strings → grep → filtered output
```

This was useful because instead of manually looking through all the output from `strings`, I could filter it down to the lines containing `=`.

## Commands to remember

```bash
strings data.txt
```

Extract readable strings from a file.

```bash
strings data.txt | grep "="
```

Extract readable strings and filter the results for lines containing `=`.

## Result

**Bandit Level 9 → Level 10 completed ✅**
