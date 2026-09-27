# Bandit Level 6

## Level Goal

The password for the next level is stored **somewhere on the server** and has these properties:

* Owned by user `bandit7`
* Owned by group `bandit6`
* 33 bytes in size

---

## Finding the File

Since the password could be **anywhere on the server**, I searched from the root of the filesystem:

```bash
find / -user bandit7 -group bandit6 -size 33c
```

### Breaking down the command

```text
find /
```

Start searching from `/`, which is the root directory of the filesystem.

```text
-user bandit7
```

Look for files whose **user owner** is `bandit7`.

```text
-group bandit6
```

Look for files whose **group owner** is `bandit6`.

```text
-size 33c
```

Look for files that are exactly **33 bytes**. `c` means bytes.

The command returned:

```text
/var/lib/dpkg/info/bandit7.password
```

I also got many `Permission denied` messages because `bandit6` does not have permission to access some directories. This did not mean the command failed. `find` continued searching through the locations it could access and eventually found the required file.

---

## Reading the Password

I used:

```bash
cat /var/lib/dpkg/info/bandit7.password
```

This displayed the password for `bandit7`.

---

# Understanding the File

I checked the file with:

```bash
ls -l /var/lib/dpkg/info/bandit7.password
```

and got:

```text
-rw-r----- 1 bandit7 bandit6 33 Jun 24 14:59 /var/lib/dpkg/info/bandit7.password
```

The general format of `ls -l` is:

```text
PERMISSIONS  LINKS  USER_OWNER  GROUP_OWNER  SIZE  DATE  TIME  FILENAME
```

So in my output:

```text
-rw-r----- 1 bandit7 bandit6 33 Jun 24 14:59
            ↑       ↑         ↑       ↑
          user    group     size    date/time
          owner   owner
```

### User owner vs group owner

```text
bandit7 bandit6
   ↑       ↑
 user    group
 owner   owner
```

* `bandit7` is the **user owner** of the file.
* `bandit6` is the **group owner** of the file.

Being the owner and having permission are related but **not the same thing**.

The permissions determine what the owner, group, and everyone else can do.

---

## Understanding `rw-r-----`

The 9 permission characters are divided into three groups:

```text
rw-   r--   ---
 ↑     ↑     ↑
 │     │     └── Others
 │     └──────── Group
 └────────────── Owner
```

For this file:

| Who               | Permission | Meaning       |
| ----------------- | ---------- | ------------- |
| `bandit7` (owner) | `rw-`      | Read + Write  |
| `bandit6` (group) | `r--`      | Read only     |
| Others            | `---`      | No permission |

So **`bandit6` does not have read + write permission**. The `bandit6` group only has **read permission**.

Because I am logged in as `bandit6`, the group permissions apply to me:

```text
r--
```

That's why I can read the password using `cat`, but I don't have permission to modify the file.

### Important idea

The **owner and group tell Linux which permission set applies**.

For this file:

```text
Owner:  bandit7 → rw-
Group:  bandit6 → r--
Others:           ---
```

---

## Understanding the first `-`

The first character in:

```text
-rw-r-----
^
```

tells me the type of filesystem object.

```text
-  → regular file
d  → directory
l  → symbolic link
```

So `bandit7.password` is a **regular file**.

---

## Understanding `ls -l` Further

```text
-rw-r----- 1 bandit7 bandit6 33 Jun 24 14:59
```

* `-` → regular file
* `rw-` → owner can read and write
* `r--` → group can read
* `---` → others have no permission
* `1` → number of hard links
* `bandit7` → user owner
* `bandit6` → group owner
* `33` → file size in bytes
* `Jun 24 14:59` → last modification date and time

---

# Using `stat`

I also used:

```bash
stat /var/lib/dpkg/info/bandit7.password
```

This gave more detailed information:

```text
size: 33
Uid: (11007/ bandit7)
Gid: (11006/ bandit6)
```

This confirms:

* The file is **33 bytes**.
* The user owner is **bandit7**.
* The group owner is **bandit6**.

It also showed several timestamps:

```text
Access
Modify
Change
Birth
```

* **Access** → when the file was last accessed/read
* **Modify** → when the contents were last modified
* **Change** → when the file's metadata, such as permissions or ownership, changed
* **Birth** → when the file was created

`ls -l` normally shows the **modification time**, while `stat` provides more detailed timestamps.

---

## Main Takeaways

This level taught me how to combine multiple conditions with `find`:

```bash
find / -user bandit7 -group bandit6 -size 33c
```

I learned that:

* `/` means the root of the filesystem.
* The server hostname is different from `/`.
* `find` can search the entire filesystem.
* `Permission denied` messages do not necessarily mean `find` failed.
* `-user` searches by user owner.
* `-group` searches by group owner.
* `-size 33c` searches for a file that is exactly 33 bytes.
* `ls -l` shows permissions, ownership, size and modification time.
* The first name after the link count is the **user owner**.
* The second name is the **group owner**.
* Owner, group and others have separate permission sets.
* `stat` provides more detailed information about a file.
