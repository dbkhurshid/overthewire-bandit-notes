# Bandit Level 14

## Level Goal

The password for the next level can be retrieved by submitting the current level's password to port 30000 on localhost using a network connection.

For this level, I needed to understand how to connect to a service running on a specific port.

## Connecting to Port 30000

I was already logged into the Bandit server as `bandit14`.

First, I used Netcat to connect to port 30000 on localhost:

```bash
nc localhost 30000
```

This established a connection. I entered the password for bandit14, and the service returned the password for bandit15.

## Understanding Netcat

I wanted to understand how I could tell whether a service was accepting connections, so I checked the connection using verbose mode and a timeout:

```bash
nc -v -w 3 localhost 30000
```

The output was:

```text
Connection to localhost (127.0.0.1) 30000 port [tcp/*] succeeded!
```

This confirmed that the TCP connection was successful.

I learned that:

- `-v` enables verbose output, showing more information about the connection.
- `-w 3` sets a three-second timeout for relevant connection and network operations.
- `localhost` refers to the machine where the command is executed. Since I was inside the SSH session, it referred to the Bandit server.
- `127.0.0.1` is the IPv4 loopback address, commonly used for localhost.
- `30000` is the destination port.

## Understanding Ports and Services

I learned the difference between a port and a service.

A **port** is a numbered communication endpoint, while a **service** is a program that receives and processes requests.

For example, port 30000 was the destination I connected to, and the service handled the password I submitted.

I also learned that TCP stands for Transmission Control Protocol. It provides reliable, ordered delivery of data between applications.

In this level:

- Netcat was the tool I used to connect.
- TCP provided the network connection.
- The service checked the password and returned a response.

Netcat itself did not check whether the password was correct; that was handled by the service.

## What I Learned

This level taught me how to connect to a network service using Netcat and how to interpret its connection output.

I also learned that a successful connection confirms that something accepted the connection, but it does not necessarily tell me how the service works internally.

The main command I learned was:

```bash
nc localhost 30000
```

This connects to port 30000 on the machine where the command is executed.
