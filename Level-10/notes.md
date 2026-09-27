# Bandit Level 10

## Level Goal

The password for the next level is stored in `data.txt`, which contains Base64-encoded data.

## What I tried

First, I checked what files were in the directory:

```bash
ls
```

There was only one file:

```text
data.txt
```

I also checked the hidden files:

```bash
ls -a
```

Then I checked the type of `data.txt`:

```bash
file data.txt
```

It showed:

```text
data.txt: ASCII text
```

This made sense because Base64-encoded data is represented using text characters.

I then looked at the contents:

```bash
cat data.txt
```

The file contained a string that looked like:

```text
VGhlIHBhc3N3b3JkIGlzIHBZZk9ZNkh3VXNEajVyTDlVdnloVTdNQ212OHZONVJvCg==
```

Since the level mentioned Base64 encoding, I checked the `base64` command:

```bash
man base64
```

I learned that the `base64` command can both encode and decode data.

I then used the `-d` option to decode the contents of the file:

```bash
base64 -d data.txt
```

This returned a message containing the password for the next level.

## What I learned

### Base64

Base64 is a way of representing data using text characters. It is **encoding, not encryption**, so it should not be treated as a method of securing a password.

Base64-encoded data can be converted back to its original form using Base64 decoding.

### `base64 -d`

The `-d` option tells the `base64` command to decode the input.

```bash
base64 -d data.txt
```

This reads the encoded data from `data.txt` and decodes it.

## Commands to remember

```bash
file data.txt
```

Check the type of a file.

```bash
cat data.txt
```

Display the contents of a file.

```bash
base64 -d data.txt
```

Decode Base64-encoded data.
