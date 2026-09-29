---
tags: [jchk, hardware, servers]
source_version: "0.6.1"
---

# JCHK Servers Sensors and Flex Nodes

The Kit uses four main server roles: analytics, large sensor, small sensor, and flex server. A model number identifies a hardware platform; a role describes what software makes it do. The similar names **SN 3100** and **FW 3100** must not be interchanged: the first is a sensor, the second a firewall.

## Reading a hardware specification

The **central processing unit (CPU)** performs computation. A **core** is an execution unit inside it; a **thread** in a hardware specification is a logical execution context, not another full physical core. **Random-access memory (RAM)** holds actively used data and programs and normally loses its contents when power is removed. A **gigabyte (GB)** is a capacity unit; the memory and storage sizes listed here reproduce the source's units.

**DDR4** and **DDR5** are generations of memory technology. **NVMe**, Non-Volatile Memory Express, is a high-speed storage interface protocol. **M.2** and **U.2** describe drive connection/form-factor families; M.2 by itself does not guarantee NVMe rather than SATA. **SATA** is another storage interface. **OS storage** holds the operating system; **bulk storage** holds the much larger application or evidence data.

## Role and capacity comparison

| Model | Quantity and placement | CPU and memory | Listed storage requirement | Main software role |
|---|---|---|---|---|
| SN 7100 | Three at Hunt Site 1 | AMD EPYC, 128 cores/256 threads; 576 GB DDR5 | Two 1.92 TB OS drives; three 61.44 TB bulk drives | JCRS-D Edge analytics-cluster node |
| SN 9000 | One at each Hunt Site | AMD EPYC, 128 cores/256 threads; 192 GB DDR5 | Two 1.92 TB OS drives; six 61.44 TB bulk drives ON-DoWIN or fourteen OFF-DoWIN | Security Onion Heavy Node |
| SN 3100 | One at each Hunt Site | Intel Xeon D, 20 cores/40 threads; 128 GB DDR4 | One 1.92 TB OS drive; one 30.72 TB bulk drive | Security Onion Forward Node |
| MS 100 | Three supplied; deployment-dependent use | Intel i3-N305, eight cores/eight threads; 16 GB memory | One 2 TB drive; interface type disagrees between sources | Flex compute |

These figures are the v0.6.1 documented requirement, not an observation of a particular machine. Volume 1 explicitly warns that storage deliveries may vary. The source describes much larger chassis maximum capacities for the SN 9000; those are hardware possibilities and should not be substituted for the installed drive count.

The three analytics servers contribute 384 cores, 768 hardware threads, and 1,728 GB RAM. The six sensors contribute 444 cores, 888 threads, and 960 GB RAM. Adding those groups gives the architecture slide's 828 cores, 1,656 threads, and approximately 2.7 TB RAM. This derived reconciliation shows that the headline excludes laptops, firewalls, and flex servers; it is not a count of every processor in every supplied device.

## SN 7100: applications and shared compute

The SN 7100 runs JCRS-D Edge, the Kubernetes-based platform used to host shared services. A **virtual machine (VM)** is a software-defined computer with its own operating system. A **container** packages an application and its dependencies while using a host kernel. Three SN 7100 nodes let the platform distribute work and provide redundant services when correctly configured.

Its network roles are deliberately separated:

- Port 1: provisioning, used to install/configure the node.
- Port 2: infrastructure access, including host management.
- Port 7: VM trunk, carrying the logical networks used by virtual machines.
- Port 8: private Kubernetes inter-node networking.
- Dedicated MGMT port: hardware management through IPMI.

A **trunk** carries multiple virtual LANs over one physical link. **IPMI**, Intelligent Platform Management Interface, is the hardware-management mechanism. **Out-of-band (OOB)** management uses a separate management path from the normal application network. The physical IPMI controller is commonly called the **baseboard management controller (BMC)**.

The SN 7100 has 10 GbE copper interfaces, 25 GbE SFP28 interfaces, and 100 GbE QSFP28 interfaces. **GbE** expresses Ethernet link speed in gigabits per second; it is not the same as drive capacity in gigabytes. [[JCHK Hunt Site 1 Cabling]] explains their connections.

## SN 9000: large local evidence store

The large sensor runs Security Onion directly on its operating system, which the sources call **bare metal** rather than a VM. Its Heavy Node role includes local Elasticsearch metadata indexing. This matters during a network interruption: its evidence-processing responsibilities extend beyond packet forwarding.

The standard table assigns port 1 to provisioning, port 2 to infrastructure, four SFP28 ports to monitoring, and IPMI to hardware management. The two 100 GbE interfaces are listed as unconfigured. A port's rated maximum speed does not prove the configured collection rate or establish a tested processing guarantee. Physical Linux interface names differ between Lesson 2 and Volume 1 for this model; match the installed interface inventory to the approved wiring plan before using a name from a slide.

## SN 3100: smaller collection footprint

The small sensor retains packet capture locally but forwards metadata to the central Security Onion pipeline. Its operating-system port names are simpler in the reference table: `eno5` for provisioning, `eno6` for infrastructure, and `eno7np0`/`eno8np2` for monitoring. These strings are **interface names**, identifiers used by the operating system to refer to network connections. The separate MGMT port is IPMI.

The black SN 3100 and blue FW 3100 can look similar. The blue unit runs pfSense firewall software; it does not replace the small sensor. FW 3100 memory is inconsistent—32 GB in Volume 1, 64 GB in Lesson 2—so a local inventory should record the actual hardware.

## MS 100: flexible supporting compute

The flex server is a small additional computer, described for spare computing capacity, a Linux workstation, or other lightweight duties. It has two 2.5 GbE network ports and dedicated IPMI. The source says it can use a 12 V power supply or **PoE++**, a method of delivering power over a compatible Ethernet connection. This is a capability, not evidence that the supplied QNAP switch provides the needed power on an arbitrary port.

Volume 1 calls its drive NVMe while Lesson 2 calls it a SATA M.2 SSD. The notes preserve that conflict rather than choosing a replacement drive type.

## Physical tamper indicators

**Tamper-evident** features make physical interference observable. Lesson 2 identifies drive-cover wire ties, lid security screws/ties, and lid-intrusion switches on the relevant server models. An **intrusion switch** detects opening of an enclosure; it is unrelated to a network intrusion-detection alert. The SN 7100 has front/rear drive-cage covers and lid features; the SN 9000 has front/rear drive tie features and a lid switch; the SN 3100 has front-drive, lid, and M.2 security features. Their presence does not prove that an inspected unit has remained intact. Record actual condition during authorized inspection and follow the handling procedure before opening or servicing equipment.

## Sources

- [[JCHK Sources#V1|Volume 1]], PDF pp. 28, 32–38: specifications, roles, interface tables, caveats.
- [[JCHK Sources#L02|Lesson 2]], PDF pp. 14–28: hardware details and anti-tamper features.
- [[JCHK Sources#L01|Lesson 1]], PDF p. 7: aggregate compute/memory headline, reconciled above by arithmetic.
- [[JCHK Sources#L06|Lesson 6]], PDF pp. 31–32: analytics-cluster role and active SN 7100 port functions.
- [[JCHK Sources#APP|Appendices]], PDF pp. 10–11: software placement.

## Related notes

[[JCHK Start Here]] · [[JCHK Kit Inventory and Packaging]] · [[JCHK JCRS-D and Kubernetes]] · [[JCHK Security Onion Architecture]] · [[JCHK Storage Capacity and Retention]] · [[JCHK Source Discrepancies]]
