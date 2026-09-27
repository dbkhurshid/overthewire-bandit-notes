# Bandit Level 7

## Level Goal

The password for the next level is stored in the file `data.txt` next to the word `millionth`.

---

## Finding the Password

I first checked the files in my home directory:

```bash
ls
```

I found:

```text
data.txt
```

The file contained a huge number of lines, so manually searching through it would be impractical.

I learned that `find` was **not** the right command for this level.

In Level 6, I used `find` because I didn't know where the file was.

Here, I already knew the file was `data.txt`. I needed to find specific **text inside the file**.

That's where `grep` is useful.

---

## Using `grep`

I used:

```bash
grep "millionth" data.txt
```


---

## What is `grep`?

`grep` searches for text/patterns inside files and displays the lines containing that text.

Basic structure:

```bash
grep "WHAT_TO_SEARCH" FILE
```

For example:

```bash
grep "millionth" data.txt
```

means:

> Search `data.txt` for lines containing `millionth`.

---

## `find` vs `grep`

This is an important difference I learned:

```text
find → searches for files/directories
grep → searches for text inside files
```

For example:

```bash
find . -name "data.txt"
```

means:

> Find a file named `data.txt`.

While:

```bash
grep "millionth" data.txt
```

means:

> Find the text `millionth` inside `data.txt`.

### Main takeaway

I don't need to manually search through a huge text file. If I know what text I'm looking for, `grep` can quickly find the line containing it.
