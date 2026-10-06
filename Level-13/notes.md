# Bandit Level 13

## Level Goal

The password for the next level is stored in `/etc/bandit_pass/bandit14` and can only be read by user `bandit14`.

For this level, I was not given the password directly. Instead, I was given a private SSH key that could be used to log in as `bandit14`.

---

## Logging into Bandit13

First, I logged into the `bandit13` account using the password I obtained from the previous level:

```bash
ssh bandit13@bandit.labs.overthewire.org -p 2220
```

Once I was inside the `bandit13` shell, I started exploring the files in the home directory.

---

## Exploring the Files

I checked the files:

```bash
ls
```

Output:

```text
HINT
sshkey.private
```

I then checked their file types:

```bash
file HINT sshkey.private
```

Output:

```text
HINT:           ASCII text
sshkey.private: OpenSSH private key
```

The `sshkey.private` file was clearly the important part.

I also read the hint:

```bash
cat HINT
```

The hint explained that the current version of OverTheWire prevents logging into one level from another through `localhost`, so I needed to **log out of `bandit13` and connect directly from my own computer**.

---

## Understanding the SSH Private Key

I knew that I normally connect to Bandit using:

```bash
ssh username@host -p 2220
```

I checked the SSH help:

```bash
ssh --help
```

and found:

```text
-i identity_file
```

The `-i` option tells SSH which identity/private-key file to use for authentication.

So I worked out that the command should have the following structure:

```bash
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

However, when I first tried it from inside the `bandit13` session, I got an error saying:

```text
!!! You are trying to log into this SSH server from localhost.
!!! Connecting from/to localhost is blocked to conserve resources.
!!! Please log out and log in again, directly from your client machine.
```

This confirmed what the hint had said.

I then logged out:

```bash
exit
```

and returned to my own Windows terminal.

---

## Why I Needed SCP

When I tried to use the private key from Windows:

```bash
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

I got:

```text
Warning: Identity file sshkey.private not accessible: No such file or directory.
```

The reason was that `sshkey.private` existed on the **Bandit server**, not on my Windows computer.

This is where I learned about `scp`.

`cp` is used to copy files on the same machine, while `scp` is used to securely copy files between machines using SSH.

The general format is:

```bash
scp [options] source destination
```

I checked the SCP help:

```bash
scp --help
```

and saw:

```text
usage: scp ... source ... target
```

I also learned that SCP uses **capital `-P`** for the SSH port, unlike SSH which uses lowercase `-p`.

---

## Copying the Private Key to My PC

The private key was in `bandit13`'s home directory:

```text
~/sshkey.private
```

Since I wanted to copy it to my Windows computer, I ran `scp` from my Windows terminal:

```cmd
C:\Users\jmaru>scp -P 2220 bandit13@bandit.labs.overthewire.org:~/sshkey.private C:\Users\jmaru\Downloads
```

I entered the `bandit13` password when prompted.

The transfer was successful:

```text
sshkey.private  100% 2602     4.1KB/s   00:00
```

I learned that the machine where I run `scp` is considered the **local machine**. In this case:

```text
Windows PC = local
Bandit server = remote
```

So the transfer was:

```text
Bandit13 server
      │
      │ scp
      ▼
Windows Downloads
```

I also learned that a remote path such as:

```text
bandit13@bandit.labs.overthewire.org:~/sshkey.private
```

means:

> use the `sshkey.private` file in the remote user's home directory.

---

## Logging into Bandit14 with the Key

After copying the key, I first tried:

```bash
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

but SSH could not find the file because my current Windows directory was:

```text
C:\Users\jmaru>
```

while the key was in:

```text
C:\Users\jmaru\Downloads\
```

I could have changed into the Downloads directory, but instead I used the full path to the private key:

```cmd
ssh -i C:\Users\jmaru\Downloads\sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

This successfully logged me in as:

```text
bandit14@bandit:~$
```

No password was requested, which confirmed that the private key authentication was working.

---

## Finding the Bandit14 Password

The level goal said the password was stored in:

```text
/etc/bandit_pass/bandit14
```

At first, I mistakenly tried:

```bash
cd /etc/bandit_pass/bandit14
```

and got:

```text
-bash: cd: /etc/bandit_pass/bandit14: Not a directory
```

This reminded me that `cd` is used for directories, while `cat` is used to read files.

I navigated to the directory containing the password files:

```bash
cd /etc/bandit_pass
```

Then I listed its contents:

```bash
ls
```

and found:

```text
bandit14
```

I read the file with:

```bash
cat bandit14
```

This gave me the password for the `bandit14` account.

I will keep the actual password out of these notes because OverTheWire specifically asks players not to post passwords or spoilers.

---

## What I Learned

This level taught me how SSH private keys work and how to transfer files between a remote server and my own computer.

The main flow was:

```text
Log into bandit13
       ↓
Find sshkey.private
       ↓
Understand ssh -i
       ↓
Log out because localhost SSH is blocked
       ↓
Use scp to copy the private key to my PC
       ↓
Use ssh -i with the local key
       ↓
Log into bandit14
       ↓
Read /etc/bandit_pass/bandit14
```

### Useful Commands and Concepts

```text
ssh -i <key> user@host
    → use a private SSH key for authentication

ssh ... -p 2220
    → specify the SSH port

scp -P 2220 source destination
    → securely copy a file between machines

~
    → the current user's home directory

cd
    → change directory

cat
    → read a file

file
    → identify the type of a file
```

One of the biggest things I learned was the difference between **local and remote paths**.

For example:

```bash
scp -P 2220 bandit13@bandit.labs.overthewire.org:~/sshkey.private C:\Users\jmaru\Downloads
```

Here, the source is remote and the destination is local.

I also learned that `scp` is not limited to being run from a local PC. The machine where I execute the command is considered the local side, and the other machine is the remote side.

---

## Authentication Methods I Discovered

During this level, I ended up learning two ways to authenticate as `bandit14`:

### Private key authentication

```bash
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

### Password authentication

```bash
ssh bandit14@bandit.labs.overthewire.org -p 2220
```

The intended solution for **Bandit 13 → 14** was the private SSH key. The password was obtained afterward by reading the password file as `bandit14`, and that password can be used for the separate **Bandit 14 → 15** level.
