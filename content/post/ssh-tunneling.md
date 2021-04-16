---
title: "Setting up SSH Tunneling - Port Forwarding"
date: 2021-04-16T23:54:31+05:30
draft: false
categories: ["tutorial", ".net", "learning"]
authors: ["Mayank Khanna"]
images: ["/images/ssh-tunneling.png"]
description: A step by step guide for setting up SSH Tunneling
tags: ["ssh", "ssh-tunneling", "port-forwarding", "security"]
---

SSH Tunneling or SSH Port forwarding is the method of creating an encrypted SSH connection between a Client and a Server, from where we can relay the ports.<!--more-->

There are many advantages of using ssh tunneling. Here is a list of a few:

1. Transferring network data of services that use and unencrypted protocol like **VNC** _(Virtual Network Computing)_ or **FTP** _(File Transfer Protocol)_
2. Accessing geo-restricted content
3. By-passing intermediate firewalls, etc.

In simple words, we can bypass any TCP Port and tunnel the traffic over a secure SSH connection.

There are manily 3 types of SSH port forwarding:

- **Local Port Forwarding** - Forwarding a connection from the client host to SSH server host and then to the destination host port.
- **Remote Port Forwarding** - Forwarding a port from the server host to a client host and then to the destination host port.
- **Dynamic Port Forwarding** - Creates a _SOCKS proxy_ server that allows communication across a range of ports.

Confused ? Lets understand them in detail in the next sections.

---

## Local Port Forwarding

This method is linked to the port on your local machine. It allows you to forward a port on the local machine to a port on the remote server, which is later forwarded to the destination machine/port.

So basicall what happens is that the SSH Client listens to every connection made to given port and creates an SSH tunnel to the remote SSH Server. The connection is later sent to the destination server. Please note that the Destination Server can be same as the remote server or a different machine also.

This method is most often used to connect to a remote service on our local network, for example a database or VNC Instance.

There are different ways to create Local Port Forwarding for different operating systems. For Linux, Unix OS and macOS we can pass the `-L` option to the following command:

```bash

    ssh -l [LOCAL_IP:]LOCAL_PORT:DESTINATION:DESTINATION:PORT [USER@]SSH_SERVER

```

The options used in the above command are:

1. `[LOCAL_IP:]LOCAL_PORT` - The local machine IP address and port number. **NOTE** - When the `LOCAL_IP` is omitted, the ssh client binds on the localhost.

2. `DESTINATION:DESTINATION_PORT` - The IP/ hosname and the port of the destination machine.

3. `[USER@]SERVER_IP` - The remote SSH user and server IP address.

Any port number greater than `1024` can be used as `LOCAL_PORT`. This is because the port numbers below `1024` are privilaged ports and can be used only by `root`. If the SSH Server is listening on a port other than `22` _(default)_, we need to use `-p [PORT_NUMBER]` option.

---

## Remote Port Forwarding

As the name suggests, remote port forwarding is exactly the opposite of what the local port forwarding does. It allows us to forward a port on the **remote server** to a port on the **local machine**, which is then forwarded to a port on the **destination machine**.

In this type of forwarding, the SSH Server listens on a given port and tunnels any connection to that port, to the specifies port on your local machine, which later on connects to the destination machine/server. **NOTE** - The destination server can be local/ remote machine.

There are different ways to create Local Port Forwarding for different operating systems. For Linux, Unix OS and macOS we can pass the `-R` option to the following command:

```bash

    ssh -R [REMOTE:]REMOTE_PORT:DESTINATION:DESTINATION_PORT [USER@]SSH_SERVER

```

The options used in the above command are:

1. `[REMOTE:]REMOTE_PORT` - The IP and the port number on the remote SSH server. An empty `REMOTE` means that the remote SSH server will bind on all interfaces.

2. `DESTINATION:DESTINATION_PORT` - The IP/ hosname and the port of the destination machine.

3. `[USER@]SERVER_IP` - The remote SSH user and server IP address.

This method is mostly used to give access to and internal service to someone from the outside. If there are any errors faced while setting up port forwarding, make sure that the `GatewayPorts` is set to `yes` in the remote SSH Server config.

---

## Dynamic Port Forwarding

This type of port forwarding allows us to create a socket on the local SSH Client (machine), which acts as a _SOCKS proxy server (Socket Secure)_. When a client connects to this port the connection is forwarded to a dynamic port on the destination machine.

This way all the apps using the SOCKS proxy will connect to the SSH Server and the server will forward all the traffic to its actual destination.

There are different ways to create Local Port Forwarding for different operating systems. For Linux, Unix OS and macOS we can pass the `-D` option to the following command:

```bash

    ssh -D [LOCAL_IP:]LOCAL_PORT [USER@]SSH_SERVER

```

The options used in the above command are:

1. `[LOCAL_IP:]LOCAL_PORT` - The local machine IP address and port number. **NOTE** - When the `LOCAL_IP` is omitted, the ssh client binds on the localhost.

2. `[USER@]SERVER_IP` - The remote SSH user and server IP address.

The port forwarding has to be separately configured for each application that you want to tunnel the traffic thought it.

---

## Conclusion

We discussed how to set up SSH tunnels and forward the traffic through a secure SSH connection. For ease of use, you can define the SSH tunnel in your SSH config file or create a Bash alias that will set up the SSH tunnel.

We will be adding a tutorial to create a SSH Config file and bash alias to set up SSH Tunnel in future. You can always tell me the topics for which I should prepare a tutotial video/blog using the contact option.
