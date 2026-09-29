---
tags: [jchk, hardware, inventory]
source_version: "0.6.1"
---

# JCHK Kit Inventory and Packaging

A JCHK **Kit** is the entire supplied equipment set. A **stack** is one of three repeatable packing groups. Each stack has three main equipment **cases** and three backpacks. A **site** describes an operational location and job; it is not a synonym for a stack. During the default deployment, all three analytics-server cases contribute to the central cluster at Hunt Site 1.

## Major inventory

| Component | Full-Kit quantity | Main purpose |
|---|---:|---|
| SN 7100 analytics server | 3 | Hosts the JCRS-D Edge application cluster |
| SN 9000 large sensor | 3 | High-capacity traffic processing, packet storage, local metadata indexing |
| SN 3100 small sensor | 3 | Additional traffic collection and local packet storage |
| MS 100 flex server | 3 | Spare compute or assigned lightweight functions |
| FW 3100 firewall | 3 | Routing, filtering, and inter-site connectivity at Hunt Sites |
| Netgate 6100 MAX firewall | 3 | Analyst-site firewall equipment; one appears in the default four-site layout |
| QNAP QSW-M7308R-4X switch | 3 | High-speed analytics-cluster interconnection |
| QNAP QSW-M3216R-8S8T switch | 6 | General site/device connectivity; fewer are shown active in the default operational layout |
| Gigamon ASF01 TAP | 6 | Copies traffic from one bidirectional network link per TAP |
| Dell Pro Max 16 Plus laptop | 9 | Analyst and deployment workstations |
| SealingTech 3U transit case | 9 | Carries the major server/networking components |
| Half-width drawer | 3 | Stores small accessories within the case arrangement |
| Travel backpack | 9 | Laptops, peripherals, and distributed accessories |
| Cable/accessory bag | 1 | Shared cables, optics, and tools |

A **bidirectional link** carries traffic in both directions. A **TAP** is the copying device; a **sensor** is the computer that processes its output. A **server** supplies computing services; a **switch** connects local devices; a **firewall** controls traffic between networks. These roles are separate even when their enclosures look similar.

The primary count source is Volume 1 PDF p. 29. Appendix A's consolidated table omits some equipment that appears in Appendix E's packing lists, including MS 100 and Netgate units. Check both the system inventory and detailed packing lists rather than interpreting an omission as proof an item is absent.

## The three case types

| Per-stack case | Contents | Where its main function goes in the default layout |
|---|---|---|
| Case 1 | One SN 7100, one MS 100, one QSW-M7308R-4X, short locking power extension | Three SN 7100 groups are co-located at Hunt Site 1 |
| Case 2 | One SN 9000 | One large sensor at each Hunt Site |
| Case 3 | One FW 3100, one SN 3100, one QSW-M3216R-8S8T, one drawer, two ASF01 TAPs, optics and short power extension | A collection/networking group at each Hunt Site |

**3U** describes rack height: three standard rack units. **Half-width** means equipment occupies approximately half a rack's width so that compatible components can sit side by side. **Rack-mounted** equipment attaches to a supporting frame rather than resting loose in a bag.

Volume 1 PDF p. 30 places the drawer in the “second” case, but Lesson 2 and all three Appendix E inventories put it in Case 3. The case-specific lists are the clearest documented arrangement; retain the inconsistency in [[JCHK Source Discrepancies]].

## What goes in the backpacks

Each stack's three backpacks repeat a common base: one laptop and its power supply, wired keyboard, wired mouse, USB Wi-Fi adapter, a ten-foot Category 6 Ethernet cable, and a power strip with the appropriate cord. **Ethernet** is the wired-network technology used here; **Category 6 (Cat6)** is the copper cable category.

The additional equipment is distributed by backpack role:

- **Backpack 1:** one Netgate 6100 MAX firewall.
- **Backpack 2:** one general-purpose QNAP switch and a USB-to-RJ45 serial console cable.
- **Backpack 3:** a DataLocker 2 TB external drive, portable monitor, VGA video cable, driver tools, and a Buffalo 1 TB portable SSD listed in the detailed inventory.

An **SSD**, or solid-state drive, stores data electronically without rotating platters. A **serial console** is a direct administrative terminal connection; an RJ45-shaped console port is not interchangeable with an Ethernet network port merely because the connector fits.

## Shared transport items

The cable bag contains the remaining copper patch cables, fiber patch cables, high-speed direct-attach cables, transceivers, extension cords, cable-making tools, and a crash-cart adapter. A **transceiver** converts a device's network interface to the required electrical or optical medium. A **direct-attach cable (DAC)** is a short cable assembly with the pluggable ends already attached. [[JCHK Network Fundamentals]] and [[JCHK Cabling and Deployment States]] explain which connection uses which medium.

The optional SKB laptop case holds nine laptops and their power supplies. The source explicitly says this larger case is **not carry-on compliant**. The nine smaller transit cases are described as carry-on-sized, at 22 × 14 × 9 inches, and can weigh up to 50 lb depending on configuration. The published description is a design characteristic, not confirmation of a particular carrier's current allowance.

## Inventory as a practical reasoning tool

An inventory should distinguish **supplied**, **packed**, **connected**, and **active**. A supplied firewall may remain packed; a temporary switch may be connected for provisioning but removed for operations. The diagram showing one analyst firewall does not reduce the full inventory from three Netgate units to one. Similarly, the default six TAPs support six observed links; more available sensor ports do not create extra TAP hardware.

The detailed Appendix E tables are the best place to record actual on-hand items and substitutions. These notes explain the organization; they are not a completed physical inspection.

## Sources

- [[JCHK Sources#V1|Volume 1]], PDF pp. 19, 29–31, 51–55: inventory, transport, accessory descriptions.
- [[JCHK Sources#L02|Lesson 2]], PDF pp. 5–12: Kit/stack/case terminology and layouts.
- [[JCHK Sources#APP|Appendices]], PDF pp. 7–9 and 28–47: consolidated and detailed per-container inventories.

## Related notes

[[JCHK Start Here]] · [[JCHK Sites and System Architecture]] · [[JCHK Servers Sensors and Flex Nodes]] · [[JCHK Analyst Workstations and Accessories]] · [[JCHK Cabling and Deployment States]]
