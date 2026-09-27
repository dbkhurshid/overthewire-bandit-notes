# Bandit Level 8

## Level Goal

The password for the next level is stored in the file `data.txt` and is the only line of text that occurs only once.

---

## Finding the Password

I first checked the file:

```bash
ls
```

I found:

```text
data.txt
```

The file contained many lines, so I needed a way to find the line that appears only once.

The commands that were useful for this were:

```text
sort
uniq
```

---

## Using `sort`

I learned that `sort` arranges the lines in alphabetical/numerical order.

```bash
sort data.txt
```

This is useful because identical lines will then be next to each other.

---

## Using `uniq`

I checked the manual:

```bash
man uniq
```

I found:

```text
-u, --unique
       only print unique lines
```

I also learned that `uniq` only detects repeated lines when they are **adjacent**.

Therefore, I need to sort the file first.

---

## Using a Pipe

I used:

```bash
sort data.txt | uniq -u
```

The `|` is called a **pipe**. It sends the output of the command on the left to the command on the right.

So:

```text
sort data.txt
      ↓
sorts the lines so identical lines are next to each other
      ↓
|
      ↓
uniq -u
      ↓
prints the line that occurs only once
```

This gave me the correct password.

---

## Important Difference

I learned the difference between `find` and `grep` from the previous level, and now learned `sort`, `uniq`, and pipes:

```text
find → searches for files/directories

grep → searches for text inside files

sort → sorts lines

uniq → finds/removes adjacent duplicate lines

| → sends the output of one command to another command
```

### Main takeaway

When using `uniq` to find unique lines, I may need to use `sort` first because `uniq` only detects duplicates when they are next to each other.
