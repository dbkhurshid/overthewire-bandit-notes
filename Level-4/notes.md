# Bandit Level 4 → Level 5

## What I learned

I entered the `inhere` directory and found 10 files:

```text
-file00
-file01
-file02
-file03
-file04
-file05
-file06
-file07
-file08
-file09
```

Because the filenames started with `-`, I used `./` when working with them.

I used the `file` command to check what type of data each file contained:

```bash
file ./-file00 ./-file01 ./-file02 ./-file03 ./-file04 ./-file05 ./-file06 ./-file07 ./-file08 ./-file09
```

The results were:

```text
-file00 → data
-file01 → data
-file02 → data
-file03 → data
-file04 → data
-file05 → data
-file06 → OpenPGP Public Key
-file07 → ASCII text
-file08 → data
-file09 → Motorola S-Record
```

I noticed that `-file07` was identified as **ASCII text**, so I read it using:

```bash
cat ./-file07
```

This gave me the password for the next level.

## What happens if I use `cat` on all of them?

I also tried reading all 10 files at once:

```bash
cat ./-file00 ./-file01 ./-file02 ./-file03 ./-file04 ./-file05 ./-file06 ./-file07 ./-file08 ./-file09
```

It worked, but it printed a lot of unreadable characters because some of the files contained binary data. All the outputs were printed together, so I couldn't understand which output belonged to which file.

When I used `file` with all the files, it showed the type of each file one by one, so I could understand which file contained what type of data.

The readable password from `-file07` was mixed in with all the other output when I used `cat`.

This showed me why using `file` first is useful. I can identify the type of each file and then use `cat` on the file that contains readable text.


## What I learned about `file`

`file` examines the contents of a file and tries to identify what type of data it contains.

`data` means that `file` found data but couldn't identify a more specific format.

I also learned that a **Motorola S-Record** is a specific format used to represent binary/program data as text.

## Key takeaway

I learned that `file` can help me identify what type of data a file contains. Instead of using `cat` on every file, I can use `file` first and then investigate the files that look relevant.
