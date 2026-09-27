# Bandit Level 11

## Level Goal

The password for the next level is stored in `data.txt`, where all lowercase (`a-z`) and uppercase (`A-Z`) letters have been rotated by 13 positions.

This is called **ROT13**.

## What I tried

First, I checked the files in the directory:

```bash
ls
```

There was a file called:

```text
data.txt
```

I then read the level description and looked up ROT13. I initially thought there might be a command called `rot13`, so I tried:

```bash
man rot13
```

but there was no manual entry.

I also tried running `rot13`, but the command was not found.

I then looked at the commands suggested by the challenge and noticed `tr`.

I checked its manual:

```bash
man tr
```

The description was:

```text
tr - Translate or delete characters
```

This showed me that `tr` can be used to translate characters from one set to another.

## Testing `tr`

Before using it on the challenge file, I tested how `tr` works:

```bash
echo "abc" | tr abc npq
```

The output was:

```text
npq
```

This helped me understand that `tr` maps each character in the first set to the corresponding character in the second set:

```text
a → n
b → p
c → q
```

I then created the ROT13 mappings.

For lowercase letters:

```text
abcdefghijklmnopqrstuvwxyz
nopqrstuvwxyzabcdefghijklm
```

For uppercase letters:

```text
ABCDEFGHIJKLMNOPQRSTUVWXYZ
NOPQRSTUVWXYZABCDEFGHIJKLM
```

I tested the complete mapping with:

```bash
echo "abc" | tr abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ nopqrstuvwxyzabcdefghijklmNOPQRSTUVWXYZABCDEFGHIJKLM
```

The output was:

```text
nop
```

This confirmed that the translation was working.

## Applying it to `data.txt`

I first tried:

```bash
echo data.txt | tr abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ nopqrstuvwxyzabcdefghijklmNOPQRSTUVWXYZABCDEFGHIJKLM
```

This translated the text `data.txt` itself instead of reading the file.

I realised that `echo` gives `tr` text that I provide, while `cat` can read the contents of a file.

So I used:

```bash
cat data.txt | tr abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ nopqrstuvwxyzabcdefghijklmNOPQRSTUVWXYZABCDEFGHIJKLM
```

This decoded the ROT13 text and returned a message containing the password for the next level.

## What I learned

### ROT13

ROT13 rotates each letter by 13 positions in the alphabet.

For example:

```text
A → N
B → O
C → P
...
M → Z
N → A
```

The same applies to lowercase letters.

Because there are 26 letters in the alphabet, applying ROT13 twice returns the original text.

### Punctuation and other characters

ROT13 only changes uppercase and lowercase English letters.

Spaces, numbers, punctuation, and other characters remain unchanged.

For example:

```text
Hello, world! 123
```

becomes:

```text
Uryyb, jbeyq! 123
```

This is why I only needed to include the uppercase and lowercase alphabets in my `tr` command.

### `tr`

`tr` can translate characters from one set to another.

The basic structure is:

```bash
tr SET1 SET2
```

Each character in `SET1` is replaced by the corresponding character in `SET2`.

### `echo` vs `cat`

`echo` is useful when I want to provide some text as input:

```bash
echo "abc"
```

`cat` is useful when I want to read the contents of a file:

```bash
cat data.txt
```

Using a pipe lets me send that output into another command:

```bash
cat data.txt | tr SET1 SET2
```

## Commands to remember

```bash
man tr
```

Read the manual for `tr`.

```bash
echo "text"
```

Print text.

```bash
cat data.txt
```

Display the contents of a file.

```bash
echo "abc" | tr SET1 SET2
```

Use `tr` to translate characters in text.

```bash
cat data.txt | tr SET1 SET2
```

Use `tr` to translate characters in a file.
