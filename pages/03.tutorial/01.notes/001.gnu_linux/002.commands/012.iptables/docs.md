---
title: iptables
taxonomy:
    category: docs
---

### Iptables Alternatives

Note that [Netfilter](https://en.wikipedia.org/wiki/Netfilter) in Linux Kernel provides firewall functionality. There are some tools to use this functionality. [iptables](https://en.wikipedia.org/wiki/Iptables) is one of them. It's considered a legacy tool. [nftables](https://en.wikipedia.org/wiki/Nftables) is going to replace it.

#### Fedora/Red Hat

In modern Fedora/Red Hat distors, the default tool to interact with Netfilter is `nftables`. Red Hat developed a tool named [firewalld](https://en.wikipedia.org/wiki/Firewalld) that uses `nftables` as its back-end.

#### Ubuntu

Ubuntu still uses iptables. By default it uses [Uncomplicated Firewall (UFW)](https://en.wikipedia.org/wiki/Uncomplicated_Firewall) on top of iptables.

### Flowchart

For a complete flowchart of how iptables works refer to this [link](https://www.frozentux.net/iptables-tutorial/images/tables_traverse.jpg). A simplified versions from [Arch wiki](https://wiki.archlinux.org/title/Iptables#Basic_concepts) is below. The lowercase word on top is the table and the upper case word below is the chain. All incoming packets start from "Network" entry in the flowchart. On "Routing decision" entry it will be decided the packet is for local machine or it should be forwarded to another machine. In the former the packet goes to "INPUT" chain in "filter" table and eventually delivered to corresponding local process. In the latter the packet goes to "FORWARD" chain in filter table and after second "Routing decision" it will be sent to destination machine.

For packets that are generated in local machine, the start entry in the flowchart is "[local process]". Then it goes to "OUTPUT" chain in nat table and so on and eventually it will be sent to destination machine.

The first "Routing decision" involves deciding if the final destination of the packet is this machine or another machine. In the former case the packet goes through INPUT chains. In the latter case the packet goes through FORWARD chains.

The second "Routing decision" involves deciding what interface to assign to an outgoing packet.

```
                               XXXXXXXXXXXXXXXXXX
                             XXX     Network    XXX
                               XXXXXXXXXXXXXXXXXX
                                       +
                                       |
                                       v
 +-------------+              +------------------+
 |table: filter| <---+        | table: nat       |
 |chain: INPUT |     |        | chain: PREROUTING|
 +-----+-------+     |        +--------+---------+
       |             |                 |
       v             |                 v
 [local process]     |           ****************          +--------------+
       |             +---------+ Routing decision +------> |table: filter |
       v                         ****************          |chain: FORWARD|
****************                                           +------+-------+
Routing decision                                                  |
****************                                                  |
       |                                                          |
       v                        ****************                  |
+-------------+       +------>  Routing decision  <---------------+
|table: nat   |       |         ****************
|chain: OUTPUT|       |               +
+-----+-------+       |               |
      |               |               v
      v               |      +-------------------+
+--------------+      |      | table: nat        |
|table: filter | +----+      | chain: POSTROUTING|
|chain: OUTPUT |             +--------+----------+
+--------------+                      |
                                      v
                               XXXXXXXXXXXXXXXXXX
                             XXX    Network     XXX
                               XXXXXXXXXXXXXXXXXX
```

![detailed flowchart](tables_traverse.jpg)


### Tables

* `filter` is the default table and we do all the filtering here like dropping a packet
* `nat` is used for destination NAT ([DNAT](https://en.wikipedia.org/wiki/Network_address_translation#DNAT)), source NAT ([SNAT](https://en.wikipedia.org/wiki/Network_address_translation#SNAT)) and IP masquerading

### Chains

A chain has a set of rules that are followed from top to bottom until a rule is matched. If we cannot find a match the default policy for chain will be applied.

* The default table `filter` contains three built-in chains: "INPUT", "OUTPUT" and "FORWARD" chains
* The table `nat`contains "PREROUTING", "POSTROUTING" and "OUTPUT" chains
* Chains have a default policy which can be either `ACCEPT` or `DROP`. If the packet pass through all existing rules in the chain and no match is found, then the default policy will be applied

#### User-Defined Chains

In a rule instead of ACCEPT or DROP target, we can jump to a user-defined chain. Like the predefined, the rules are pass through from top to bottom until a match is found. otherwise we return to previous chain. The following image is from this [link](https://www.frozentux.net/iptables-tutorial/iptables-tutorial.html#USERTABLES) that shows the process of visiting a user-defined chain:

![user-defined chain workflow](table_subtraverse.jpg)

### Useful Links

* [Arch Wiki](https://wiki.archlinux.org/title/Iptables)
* [Simple stateful firewall by Arch Wiki](https://wiki.archlinux.org/title/Simple_stateful_firewall)
* [Flowchart](https://www.frozentux.net/iptables-tutorial/images/tables_traverse.jpg)
* [Flowchart explanation](https://www.frozentux.net/iptables-tutorial/iptables-tutorial.html#TRAVERSINGOFTABLES)
* [Iptables tutorial](https://www.frozentux.net/iptables-tutorial/iptables-tutorial.html): This is a very good explanation of Internet Protocols as well as how iptables works
