---
title: Firewall
taxonomy:
    category: docs
---

### Interacting with [Netfilter](https://en.wikipedia.org/wiki/Netfilter)

Netfilter in Linux Kernel provides firewall functionality. There are some tools to use this functionality. [iptables](https://en.wikipedia.org/wiki/Iptables) is one of them. It's considered a legacy tool. [nftables](https://en.wikipedia.org/wiki/Nftables) is going to replace it. For a basic nftables setup you can read this [page](https://wiki.archlinux.org/title/Nftables#Simple_stateful_firewall).

#### Fedora/Red Hat

In modern Fedora/Red Hat distors, the default tool to interact with Netfilter is `nftables`. Red Hat developed a tool named [firewalld](https://en.wikipedia.org/wiki/Firewalld) that uses `nftables` as its back-end.

#### Ubuntu

#### 21.10 and newer

The default firewall tool is `nftables`.

#### Versions before 21.10
 
The default firewall tool is `iptables`. You can use [Uncomplicated Firewall (UFW)](https://en.wikipedia.org/wiki/Uncomplicated_Firewall) on top of iptables. Refer to Ubuntu wiki [page](https://ubuntu.com/server/docs/security-firewall) to see ufw tutorial.

##### Make nftables the default firewall tool

You can switch to nftables by installing it first:

```
$ sudo apt install nftables
```

Make sure it starts at boot time by `systemctl enable nftables.service` and then you can make it the default firewall tool:

```
$ sudo update-alternatives --set iptables /usr/sbin/iptables-nft
$ sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-nft
$ sudo update-alternatives --set arptables /usr/sbin/arptables-nft
$ sudo update-alternatives --set ebtables /usr/sbin/ebtables-nft
```

Fore more information refer to this Debian wiki [page](https://wiki.debian.org/nftables).

### Tips

* You can use port names (e.g. http instead of 80). For a complete list see:

```
cat /etc/services
```

