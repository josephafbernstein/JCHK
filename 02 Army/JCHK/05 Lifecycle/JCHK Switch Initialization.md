---
tags: [jchk, lifecycle, switching, deployment]
source_version: "0.6.1"
---
# JCHK Switch Initialization

A **network switch** connects devices on local networks. The JCHK switches also separate traffic into **VLANs**, or virtual local area networks, so management, provisioning, and operational traffic can follow different logical paths even when cables share one physical switch. Correct switch configuration is therefore a prerequisite for [[JCHK JAKD Deployment]].

This note covers bringing an unconfigured switch to its approved starting state. It does not recommend resetting a working mission switch as routine maintenance. Begin with [[JCHK Start Here]] and [[JCHK Deployment Roadmap]] for context.

## Why each switch needs its own file

Two QNAP models are configured in Volume 2. Four `QSW-M3216R-8S8T` switches support the Analyst Site and hunt-site access functions. Three `QSW-M7308R-4X` switches support the central cluster at Hunt Site 1. A spare switch used for temporary WAN connectivity is a separate role; do not infer that every physically present spare receives one of the seven site-specific files below.

| Switch role | Configuration file named in Volume 2 |
|---|---|
| Analyst Site switch 1 | `Analyst_Site_Switch-1.conf` |
| Hunt Site 1 switch 4 | `Hunt_Site_1_Switch-4.conf` |
| Hunt Site 2 switch 1 | `Hunt_Site_2_Switch-1.conf` |
| Hunt Site 3 switch 1 | `Hunt_Site_3_Switch-1.conf` |
| Hunt Site 1 switch 1 | `Hunt_Site_1_Switch-1.conf` |
| Hunt Site 1 switch 2 | `Hunt_Site_1_Switch-2.conf` |
| Hunt Site 1 switch 3 | `Hunt_Site_1_Switch-3.conf` |

A `.conf` file holds configuration settings. Importing the wrong file may leave the switch reachable but place ports or its management address in the wrong network. Match the switch's label, model, intended site, and file before import. The files are provided on the DataLocker SSD marked `CONTENT`. [[JCHK Sources#V2|Volume 2]], PDF pp. 30, 32–33, 37–38.

## Isolated first contact

The source says a factory-reset switch initially tries **DHCP**, the service that automatically supplies IP addressing. If no DHCP server answers, the documented fallback is `169.254.100.101`. This is a **link-local** address: it supports communication on the local link rather than ordinary routed communication between sites.

For the documented isolated setup, temporarily set the configuration laptop's Ethernet adapter to `169.254.100.102`, with subnet mask `255.255.0.0`. A **subnet mask** identifies which part of an IPv4 address represents its local network. Configure one switch at a time, because the factory-reset switches share the same fallback address. Connecting several together in that state creates an **IP conflict**, where different devices try to use one address.

The manual specifically avoids connecting the switch to an existing DHCP-providing network before importing its configuration; otherwise its address can change and become harder to locate. For the M3216 model, it uses port 1 for laptop access. For the M7308 model, the text first specifies the rear `MGMT` port and then says port 1. That conflict must be resolved against the approved device procedure and actual labels; see [[JCHK Source Discrepancies]]. [[JCHK Sources#V2|Volume 2]], PDF pp. 30–33, 38.

## Factory reset is a configuration change

> [!warning] A factory reset removes the switch's established configuration
> It can remove the site's working addressing, VLANs, and access settings. Use it for the planned clean-build workflow or an approved recovery, with the correct configuration file available. A reboot merely restarts the device and is a different operation.

The documented reset behavior differs between models:

| Model | Source's full-reset action | Important distinction |
|---|---|---|
| QSW-M3216R-8S8T | With only power connected, wait about two minutes, then hold the front `Rst` button at least 20 seconds. | A shorter hold resets credentials rather than performing the documented full factory reset. |
| QSW-M7308R-4X | With only power connected, wait about two minutes, then hold the rear `Reset` button at least 10 seconds. | Observe the described status-light changes; the reset is complete when the light becomes solid. A shorter hold only resets credentials. |

Do not transfer the timing from one model to the other. [[JCHK Sources#V2|Volume 2]], PDF pp. 32, 37.

## Import and verification

Open the switch's documented HTTPS management page from the locally connected laptop. **HTTPS** is encrypted web communication. The initial configuration may present a certificate warning because the laptop has not yet established trust in the device's certificate. The manual accepts this during the isolated first-contact procedure; that is not a general reason to ignore unexpected certificate warnings on other networks.

Use the device's approved initial access procedure, complete any required temporary password change, and open **System Settings → Backup & Restore → Restore System Settings**. Select the correct file, confirm the import, and wait for the reboot to finish. The switch's address and credentials can change as part of restoring the file. The notes do not repeat the default passwords printed in the sources. [[JCHK Sources#V2|Volume 2]], PDF pp. 33–41.

Verify the resulting device identity and site assignment, management access, intended port/VLAN settings, and physical link indicators. A **link indicator** proves that the physical connection is detected; it does not by itself prove that VLAN tagging, routing, or applications work. **VLAN tagging** is the labeling of Ethernet traffic with its VLAN identity. An **access port** normally serves one untagged network; a **trunk port** carries multiple VLANs using tags.

When all imports are complete, change the laptop back to “Obtain an IP address automatically,” restoring DHCP. That step is essential before later firewall and service access. [[JCHK Sources#V2|Volume 2]], PDF pp. 41, 143–144.

The guide has a count typo (“all six” in the M7308 procedure despite listing three) and address differences elsewhere in the source package. Keep source-specific configuration values together and use the validated configuration for the actual kit. See [[JCHK Source Discrepancies]].

Related: [[JCHK Hardware Baselines]] · [[JCHK Analyst Laptop Preparation]] · [[JCHK Post-Deployment Readiness]] · [[JCHK Troubleshooting and Recovery]]
