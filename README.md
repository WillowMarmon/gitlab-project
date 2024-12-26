# Setup an On Prem Dev-Ops server from scratch using git-lab

✅ [Prerequisites]()

## Description

This project consists of the installation and setup of my actual on-prem Dev-Ops
server that I use for open-source project development. It involves the installation,
configuration, and deployment of GitLab using Docker and Docker-Compose.

### Technologies Used

- Docker
- Docker-Compose

### Languages Used

- YAML
-

## Prerequisites

### 1. A server

You can use just about anything for this, one common choice is to use an old
office tower for the purpose of a home server, but in this example we'll be using
my personal server which is a combination of purpose built server hardware and
some more consumer grade elements.

Mine is running nix-os (a Linux distribution), but yours can be running Macos, Linux,
or even openBSD. So long as git-lab is available there's a good chance it'll work.

### 2. Networking

This project is not really about networking, docker networking, or what kinds of
networking you're going to need. If you need more information about configuring your
home-lab network I'd recommend [serve the home]() they have great resources, and
if you're looking for docker networking I'd recommend [this article]() by SOMEGUY.

### 3. An understanding of your operating system and included tools

While I will be walking you through the docker-compose setup, a baseline of
knowledge about your tools is expected. For example, I'll be using a text editor
called vim, but other options like nano, emacs, or notepad would work just as
well just so long as you understand the tools yourself.

