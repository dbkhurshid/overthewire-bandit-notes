# Bandit Level 9

## Goal

The password for the next level is stored in `data.txt` in a binary-looking file. We need to find the human-readable strings and identify the password.

## What I used

```bash
strings data.txt | grep "="
```

### `strings`

`strings` extracts readable text from a file. This is useful when a file contains binary or non-readable data but also has some normal text hidden inside it.

### `grep "="`

I used `grep` to filter the output and only show lines containing `=`.

The output included:

```text
========== password
```

This showed where the password was located.

## What I learned

`strings` is useful when I need to search through a file that doesn't look like normal text.

Combining commands with `|` makes it easier to filter the output. Here, `strings` found the readable text and `grep` narrowed it down to the lines containing `=`.

## Command to remember

```bash
strings data.txt | grep "="
```
