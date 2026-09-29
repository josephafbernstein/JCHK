---
tags: [jchk, guide]
source_version: "0.6.1"
---
# JCHK Network Fundamentals

Up: [[JCHK Start Here]] · Next: [[JCHK Network Address Plan]]

## Begin with one message

When an analyst opens a JCHK tool in a browser, the laptop sends data through its local switch, site firewall, and possibly an encrypted tunnel to the server running that tool. These are different jobs: the switch connects nearby devices, the firewall controls and routes traffic between networks, and the server supplies the application. The physical arrangement is a **topology**.

A **packet** is a small addressed unit of network data. An **Ethernet frame** carries a packet over a local link. A **protocol** is an agreed set of rules for exchanging data. Ethernet describes local-network communication; **IP**, Internet Protocol, provides addresses and delivery across networks. **TCP**, Transmission Control Protocol, provides an ordered, reliable byte stream. **UDP**, User Datagram Protocol, sends individual messages without TCP's delivery guarantees. These definitions explain the JCHK diagrams; they are general networking concepts, not unique kit components.

## Addresses and subnet notation

An **IPv4 address** has four numbers, such as `10.1.17.11`. Each number is an **octet**, an eight-bit value from 0 to 255. An **interface** is a device's network connection; one device can have several interfaces and addresses. A **subnet** is a defined group of IP addresses that can communicate locally under a shared prefix.

**CIDR**, Classless Inter-Domain Routing, is the slash notation following an address. In `10.1.17.0/24`, the first 24 of 32 bits identify the network. A **subnet mask**, such as `255.255.255.0`, expresses the same boundary. A `/24` contains 256 addresses; conventional IPv4 subnets reserve the first as the network address and the last for broadcast. A `/16` contains 65,536 addresses; a `/20` contains 4,096 and can be divided into sixteen `/24` networks. A `/30` contains four addresses and conventionally provides two host addresses, useful for a tunnel between two firewalls.

The JCHK's documented kit block is `10.1.0.0/16`. It is **private address space**, intended for private networks and not globally unique on the public Internet. A partner may already use overlapping private addresses. A valid-looking address alone does not prove that two networks can be joined without routing conflicts. The actual deployed inventory determines the kit's addresses.

A **default gateway** is the router used when the sending device has no more specific route. A **route** states which destination network should use which next device or interface. A **next hop** is that next device. A **static route** is explicitly configured instead of learned through a routing protocol. A reply also needs a valid return path.

## VLANs separate purposes on shared switches

A **VLAN**, virtual local area network, separates Ethernet traffic logically even when devices share physical switches. Its numeric **VLAN ID** identifies that logical segment on the relevant links. The JCHK uses VLANs for analyst access, infrastructure management, hardware management, tools, Kubernetes, the DMZ, and temporary provisioning.

An **access port** normally carries a single VLAN without an Ethernet VLAN tag on the endpoint-facing wire. A **trunk port** carries specified tagged VLANs on one physical link. **Tagged** means the Ethernet frame includes a VLAN identifier; **untagged** means it does not. A **native VLAN** or **PVID** (port VLAN identifier) associates incoming untagged traffic with a VLAN, depending on the switch's configuration. A **hybrid port** combines tagged and untagged membership. An **excluded VLAN** is not allowed through that port.

The same VLAN ID at two sites does not mean they are one shared IP subnet. For example, VLAN 10 is the infrastructure role at each site, but each site has its own addresses. **Layer 2 (L2)** refers here to Ethernet switching and VLANs; **Layer 3 (L3)** refers to IP routing between subnets. JCHK site-to-site tunnels provide routed L3 connectivity, not an automatic extension of every Ethernet segment.

## Four supporting services

| Service | Meaning | Why a JCHK beginner cares |
| --- | --- | --- |
| DHCP | Dynamic Host Configuration Protocol; leases network settings to clients | A laptop may receive its IP, gateway, and DNS settings automatically; statically addressed hosts do not depend on a DHCP lease for their address |
| DNS | Domain Name System; translates names into addresses | `so-manager.kit1.jchk` is easier to use than a numeric address, but only works with the right DNS records and resolver |
| NTP | Network Time Protocol; synchronizes clocks | Accurate time supports log correlation and time-sensitive authentication |
| TLS | Transport Layer Security; protects connections and authenticates servers using certificates | A reachable web page can still fail certificate checks if its name or trust chain is wrong |

A **hostname** names a computer or service. An **FQDN**, fully qualified domain name, includes its full domain, such as `jcrsd-1.kit1.jchk`. A **port number** such as TCP 8220 identifies a software communication endpoint. This is different from physical port 8 on a firewall. **HTTPS** is HTTP web communication protected by TLS. **SSH**, Secure Shell, provides encrypted remote command-line access.

## Management is not packet capture

**In-band management** reaches a running operating system through its normal network interface. **Out-of-band management (OOBM)** reaches a separate hardware controller, often labeled **MGMT**, so an operator can inspect hardware or access its console even if its main operating system is unresponsive. **IPMI**, Intelligent Platform Management Interface, is a management standard; **BMC**, baseboard management controller, is the hardware controller implementing management functions. It still needs appropriate power and network access.

A sensor's **monitor interface** receives copied traffic for analysis. It serves a different purpose from either management path. A working MGMT login does not prove that the capture cable or operating-system interface works.

## Check understanding

An analyst can reach a server by address but not by name: investigate DNS before replacing the capture cable. A link light is on but the laptop cannot reach its gateway: check IP settings and VLAN membership as well as the physical connection. A tunnel handshakes but a web application fails: routing, rules, DNS, certificates, and the application still need verification.

## Sources and related notes

General explanatory definitions support the supplied architecture. JCHK-specific application: [[JCHK Sources#L04|Lesson 4]], PDF pp. 6–10, 15, 19–28; [[JCHK Sources#APP|Appendices]], PDF pp. 16–24; [[JCHK Sources#V3|Volume 3]], PDF pp. 25–34. Continue to [[JCHK Firewalls and WireGuard]], [[JCHK Cabling and Deployment States]], and [[JCHK Glossary]].
