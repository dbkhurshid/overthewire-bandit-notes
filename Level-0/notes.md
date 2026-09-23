# Bandit Level 0

## What I learned

I understood what **SSH (Secure Shell)** is and that it can be used to connect to a remote computer.

I connected to the Bandit server through my Windows PowerShell using:

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

The general format is:

```bash
ssh <username>@<hostname> -p <port>
```

* `bandit0` → username
* `bandit.labs.overthewire.org` → hostname/server
* `-p 2220` → specifies the port number

It then asked me for a password to log in.

## What I found interesting

I didn't know I could use my Windows computer to connect to a Linux machine somewhere else.

## Key takeaway

I learned how to use SSH to connect to a remote Linux machine from my Windows computer and understood the basic structure of an SSH command.
