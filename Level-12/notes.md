# Bandit Level 12

## What I needed to do

The file `data.txt` was a **hexdump**, and the goal was to recover the original data and keep extracting/decompressing it until I found the password for the next level.

I worked in `/tmp` because I needed a place where I could create and modify files.

## Step 1: Convert the hexdump back to binary

First, I checked the file:

```bash
file data.txt
```

It was a hexdump, so I converted it back into binary using:

```bash
xxd -r data.txt > reverse_dump
```

I used `>` here because `xxd -r` writes the converted data to standard output.

Then:

```bash
file reverse_dump
```

showed that it was gzip compressed.

## Step 2: Keep identifying and extracting each layer

I repeated the process of checking the file type with `file` and then using the appropriate command.

The chain I found was:

```text
hexdump
↓ xxd -r
gzip
↓ gzip -d
bzip2
↓ bzip2 -d
gzip
↓ gzip -d
tar
↓ tar -xf
tar
↓ tar -xf
bzip2
↓ bzip2 -d
tar
↓ tar -xf
gzip
↓ gzip -d
ASCII text
```

Some of the files had names like `.bin` even though they were actually compressed files. This taught me not to trust the file extension.

For example:

```bash
file data6.bin
```

showed:

```text
data6.bin: bzip2 compressed data
```

So I renamed it:

```bash
mv data6.bin data6.bz2
```

and then:

```bash
bzip2 -d data6.bz2
```

## Commands I used

For gzip:

```bash
gzip -d file.gz
```

For bzip2:

```bash
bzip2 -d file.bz2
```

For tar:

```bash
tar -xf archive
```

To identify the next layer:

```bash
file filename
```

## Something I learned about `>`

At first I thought I could always use:

```bash
gzip -d file > newfile
```

but that created an empty file.

The reason is that `gzip -d` normally creates the decompressed file itself.

When I specifically want gzip to send the decompressed data to standard output, I can use:

```bash
gzip -dc file.gz > newfile
```

The `-c` makes the difference.

## Final result

After following all the layers, the final file was ASCII text and contained the password for **Bandit Level 13**.

## What I learned

This level was mainly about learning how to identify unknown files and choose the correct tool instead of guessing.

The main workflow I learned was:

```text
file → identify → use the correct tool → file again
```

I also got more comfortable with `mv`, `gzip`, `bzip2`, `tar`, `file`, and shell redirection.
