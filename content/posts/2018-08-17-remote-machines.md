---
title: "Working with remote machines"
date: 2018-08-17
type: post
tags: ['shell', 'zsh','functions']
---

As a heavy shell user, I typically try to reduce the number of keystrokes required to fulfill a certain task. As I am
working a lot with remote machines, I am logging into those machines pretty often.

## Problem statement

In order to log into a remote machine, I am using the `ssh` command. The typical command is `ssh <user>@192.168.0.123`,
if the user is named differently on the other machine. I am often facing the following problems:

1. What the heck is the IP of the remote machine (which is not easy since I am working with freshly flashed embedded devices)?
1. How can I prevent to type in the password all the time?
1. Is there a way to shorten the command above?

## Solution #1

There is a neat Linux command which allows to query the machines in your local network called [nmap](https://nmap.org/).
`nmap` can easily be used to query your local network by the following command:

```sh
> nmap -sn 192.168.0.0/24

Starting Nmap 7.70 ( https://nmap.org ) at 2018-08-17 15:34 CEST
Nmap scan report for 192.168.0.1
Host is up (0.00063s latency).
Nmap scan report for 192.168.0.100
Host is up (0.0073s latency).
Nmap scan report for 192.168.0.101
...
Nmap done: 256 IP addresses (12 hosts up) scanned in 3.05 seconds
```

Although I get a nice list of IP addresses registered in my network, I am somehow missing the type of the machine being
connected to the specific IP address. This can easily be solved by using the `nmap` command in `sudo` mode:

```sh
> sudo nmap -sn 192.168.0.0/24

Starting Nmap 7.70 ( https://nmap.org ) at 2018-08-17 15:34 CEST
Nmap scan report for 192.168.0.1
Host is up (0.00017s latency).
MAC Address: D0:5B:A8:8E:84:92 (zte)
...
Nmap scan report for 192.168.0.121
Host is up (0.00029s latency).
MAC Address: 88:4A:EA:DF:AA:A0 (Texas Instruments)
Nmap done: 256 IP addresses (12 hosts up) scanned in 3.04 seconds
```

Much better since you can see that the device connected at `192.168.0.121` is a TI device (in that case my BeagleBone
Black).

## Solution #2

Now I can login into the machine using `ssh debian@192.168.0.121`. The prompt asks me for a password, which is the
standard behavior. For default Debian 9.4 images, the default password is `temppwd`. Although this is not that
complicated, I would like to automatically log into the machine w/o a password prompt. Well, the only thing you have to
do is to add your public ssh key of your machine into the `.ssh/authorized_keys` file on the remote machine:

```sh
> ssh debian@192.168.0.121
> mkdir -p .ssh
> vi authorized_keys
# Paste in your local .ssh/id_rsa.pub key
```

This allows to login without any password prompt.

## Solution #3

Still always typing the whole `ssh` command is tedious, so I made the following `zsh` function which reduces the command
for logging in to `bbb 121`:

{{< highlight sh >}}
# SSH to a machine in the local network
function bbb {
  if [[ $# < 1 ]]; then
    echo "Please specify last number of IP (e.g. bbb 120 means ssh debian@192.168.0.120)"
  else
    ssh debian@192.168.0.$1
  fi
}
{{</ highlight >}}

You can change the network sub-mask according to your local network.
