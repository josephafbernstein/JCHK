---
tags: [jchk, lifecycle, troubleshooting, recovery]
source_version: "0.6.1"
---
# JCHK Troubleshooting and Recovery

Troubleshooting means identifying the cause of a failure before changing the system. Recovery means deliberately returning it to a usable, known state. A failure at one layer can appear as a symptom somewhere else: an incorrect clock may look like an authentication problem, and an incorrect VLAN may look like an application outage.

Start with [[JCHK Start Here]] to identify the role of the failing component. Use [[JCHK Deployment Roadmap]] to determine whether this is a new build, normal operation, startup, or shutdown. Those states have different expected behavior.

## Use four questions

Volume 2 organizes troubleshooting into **symptom**, **likely cause**, **validation checks**, and **corrective action**. In plain language:

1. What exactly is wrong, from which machine, and since when?
2. Which prerequisite could explain that observation?
3. What observation would confirm or disprove that explanation?
4. What is the smallest appropriate recovery within the validated configuration?

Record the device/site, time, observed error, recent change, checks performed, and result. **Traceability** means someone else can reconstruct what happened. **Configuration drift** means the actual settings no longer match the expected baseline. The guide calls for pausing and escalating through configuration management when resolution would require an unvalidated change. That is an operational process described by the manual, not an instruction for the note-writing assistant to change equipment. [[JCHK Sources#V2|Volume 2]], PDF pp. 142, 146.

## Separate diagnosis from actions that change the kit

| Action class | Examples | Implication |
|---|---|---|
| Observe | Read logs; inspect link lights; inspect health/status pages; compare labels and current settings. | Establishes evidence without intentionally changing configuration. |
| Restart a service | Restart a JAKD service or the affected application. | Interrupts that service and can clear transient state; it does not reinstall the kit. |
| Reboot a device | Restart a sensor or server through its normal OS controls. | Interrupts every workload on that device. Follow dependencies. |
| Reapply configuration | Import a switch/firewall file or rerun a configuration playbook. | May replace current settings and change management access. |
| Redeploy/reimage | Run an installation workflow again. | May replace applications, configuration, and disk contents. |
| Factory reset/array clearing | Reset a switch/firewall or clear storage-controller virtual drives. | Removes established configuration and can cause data loss. |

These actions are not interchangeable. “Try a reset” is too ambiguous to be a useful recovery instruction. Identify the exact target and effect before acting. The destructive imaging and storage warnings appear in [[JCHK Analyst Laptop Preparation]] and [[JCHK Hardware Baselines]]. [[JCHK Sources#V2|Volume 2]], PDF pp. 18, 23, 32, 37, 64–67, 109–110.

## Troubleshoot from foundations upward

| Symptom | First checks | What the checks distinguish |
|---|---|---|
| Device unexpectedly stops or becomes unresponsive | Power source/load, correct cables, ventilation, ambient temperature. | An electrical or thermal issue versus a software problem. |
| Disk missing, degraded array, or post-image boot failure | Firmware boot mode/order, controller identity, drive detection and health, intended RAID/JBOD layout. | Missing hardware or wrong baseline versus an application issue. |
| Installation USB absent or installer not starting | Reseat media, try another port, inspect F12 boot menu, compare firmware baseline, verify media. | Bad access/media versus a fault in the installed OS. |
| Switch inaccessible | Link indicators, current laptop address, intended access port, management VLAN/address, correct imported file. | Physical connection, addressing, and switch-configuration failures. |
| Another site inaccessible | Firewall interface status, WireGuard peer/tunnel state, routes, and actual external endpoints. | Local connectivity versus inter-site encryption/routing failure. |
| Network boot fails | Provisioner reachability, DHCP/TFTP service health, correct virtual-adapter mapping, provisioning cabling/VLAN. | Discovery/boot-service failure versus a later installer problem. |
| JAKD playbook stops | Exact failed task and error, management connectivity, IPMI access, selected hardware, earlier stage results. | A real failed prerequisite versus a later symptom. |
| Security Onion sensor has no data | Capture-interface links, TAP input/output, appropriate optics, storage health, Security Onion service status. | Missing traffic, failed capture, or failure in onward processing. |
| Tool web page fails or is slow | DNS, certificate/time state, credentials, VM CPU/memory, firewall/service port reachability. | Naming/trust/access/resource pressure versus application failure. |

**CPU** is the processor; **memory** is the working space applications use while running. **Resource contention** means workloads compete for limited CPU, memory, or storage resources. **Degraded RAID** means the storage array has lost some redundancy or normal function. **TFTP** transfers boot files; **DHCP** supplies addressing. [[JCHK Sources#V2|Volume 2]], PDF pp. 142–146.

## Reason carefully about “reachable”

A cable's link light confirms a physical link, not correct network segmentation. A successful address-level connection does not prove DNS is correct. **DNS**, the Domain Name System, converts a service name into an address. A responding HTTPS page does not prove the correct certificate is trusted, the login works, or the application is receiving data.

Likewise, a successful login verifies access, not a complete mission workflow. Volume 2's immediate post-deployment check is deliberately limited to opening and logging into deployed services; Mattermost still requires initialization. Full readiness additionally requires healthy services, correct final cabling, and working collection. [[JCHK Sources#V2|Volume 2]], PDF pp. 61–63, 89–97.

## Interpreting missing network data

**PCAP**, packet capture, is the captured packet record. **Metadata** describes observed communication—for example, who communicated, when, and over which protocol—without necessarily containing every original packet. **Suricata** and **Zeek** are engines used by Security Onion to inspect traffic and produce detections or structured network records.

If neither PCAP nor metadata arrives, start near the traffic source: is the intended link active, is the TAP observing it, are monitor outputs connected to the correct sensor capture ports, and are interfaces seeing input? Then inspect sensor services and storage. If local capture exists but central searches show nothing, check forwarding/receiving, indexing, time range, and connectivity. This is a diagnostic decomposition of the architecture, not proof of the failing component in any specific incident.

Volume 2 lists incorrect NIC profiles, TAP connections, and storage faults as potential causes. Its recovery options include checking/synchronizing the Security Onion Grid, reviewing optics, reinitializing interfaces, or rebooting sensors. A **Grid** is the collection of Security Onion nodes. Apply a recovery only after checking its intended scope and current mission impact. [[JCHK Sources#V2|Volume 2]], PDF p. 145.

## Reruns, backups, and rebuild boundaries

The guide allows rerunning an affected playbook after correcting the cause, but does not establish that every deployment task preserves existing mission data. Do not treat the word “automation” as a guarantee of non-destructive behavior. Capture the original error and examine the selected playbook's role before rerunning it. [[JCHK Sources#V2|Volume 2]], PDF pp. 59–60, 145.

Keep recoverable configuration and data through the approved backup process before planned replacement, image installation, or destructive storage work. An exported firewall/switch configuration is a settings backup, not a backup of all packet captures and application data. A VM package or snapshot likewise is not automatically a complete backup of the distributed kit. Volume 2 documents configuration-file restore and clean-build operations, but does not provide a complete, tested whole-kit disaster-recovery procedure; do not infer one from those individual actions.

The ASF01 lost-password procedure is a special example: the source uses an Apploader command that resets non-volatile-memory settings and then reboots to the default account state. **Non-volatile memory** retains settings when power is off. That process is broader than merely viewing a password and must be treated as a device reset, followed by restoring intended settings and credentials. The ordinary `passwd` command then changes the logged-in account's password. Default passwords are not reproduced in these notes. [[JCHK Sources#V2|Volume 2]], PDF pp. 92–95.

For hardware discrepancies, conflicting addresses, unexplained playbook behavior, or missing baseline details, preserve evidence and use the current validated configuration instead of combining guesses from conflicting tables. See [[JCHK Source Discrepancies]].

Related: [[JCHK Post-Deployment Readiness]] · [[JCHK JAKD Deployment]] · [[JCHK Startup]] · [[JCHK Shutdown and Pack-Out]]
