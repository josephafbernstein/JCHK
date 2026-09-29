---
tags: [jchk, forensics, tools, evidence]
baseline: v0.6.1 training snapshot
---
# JCHK Analyst Tools and Evidence Handling

[[JCHK Start Here]] · [[JCHK Software Map and Baseline]]

The analyst laptop is a workspace for accessing kit services and examining selected evidence in detail. A tool's presence in an inventory is not proof it was installed, licensed, configured, or approved for the current task. The training materials distinguish desktop applications from optional virtual machines and flag several tools as absent from v0.6.1.

## Choose by evidence and question

| Question or evidence | Documented tool | What it does |
|---|---|---|
| Network packets | Wireshark | Decodes captured packets and helps inspect conversations |
| Sessions/files from network captures | NetworkMiner Free/Pro | Extracts network artifacts; edition/integration varies |
| Disk or file acquisition/preview | FTK Imager | Acquires and previews forensic data |
| Windows memory acquisition | WinPmem | Captures volatile memory |
| Memory examination | Volatility 3 | Analyzes memory structures; marked absent in Lesson 3 |
| Windows Registry | RegRipper | Extracts useful information from registry data |
| General forensic investigation | SIFT Workstation; Autopsy | Forensic tool environment / graphical forensic platform; Autopsy marked absent |
| Program structure | Ghidra; FLARE VM | Supports reverse engineering and malware examination |
| Controlled malware behavior | CAPEv2 | Executes samples in a purpose-built analysis environment |
| Network discovery | Nmap | Discovers hosts and services through network probes |
| Vulnerability assessment | Nessus; Greenbone/OpenVAS | Checks systems/services for known weaknesses |
| Configuration compliance | SCAP Compliance Checker; STIG Viewer | Checks or reviews security-configuration requirements |

**Volatile memory** loses its contents when power is removed; a disk normally retains stored data. A **forensic image** is an acquired representation of a storage device or other evidence source. The **Windows Registry** is a database of operating-system/application settings. **Reverse engineering** studies a program's structure or behavior to understand how it works. A **sandbox** is an intentionally isolated environment for running untrusted software.

**Vulnerability** means a weakness that may be exploitable. A vulnerability-scan finding is not proof it has been exploited. **SCAP**, Security Content Automation Protocol, describes standards for automated configuration checking; **STIG**, Security Technical Implementation Guide, names prescribed security configuration guidance. A compliance finding and a compromise finding answer different questions.

## Evidence handling fundamentals

The following workflow is explanatory practice supporting the tools in the course, not a replacement for the mission's formal evidence procedures:

1. Record the question and source system before acquiring data.
2. Record collection time/time zone, collector, tool/version, parameters, and any errors.
3. Preserve the original acquisition/export and perform examination on a working copy.
4. Use an appropriate cryptographic **hash**, a fixed-length digest of data, to help check whether a file changes. A matching hash supports integrity; it does not prove who created the evidence or that the acquisition was complete.
5. Track transfers, access, and storage locations. **Chain of custody** is the record of who controlled evidence and when.
6. Link findings back to exact evidence items and distinguish observations from interpretations.

The kit includes encrypted external SSDs for transporting data. **Encryption at rest** protects stored content when properly locked; it does not replace access control, backups, or careful handling while unlocked. Volume 3 describes automatic secure erasure after exhausted password attempts. Do not experiment with password guesses on a drive holding unique evidence. Follow the supported unlock, eject/lock, and power-off procedures.

## Optional VMs and isolation

The supplied VM images are packaged as **OVA (Open Virtual Appliance)** files for import into VMware Workstation. They support forensics, reverse engineering, malware analysis, vulnerability assessment, and authorized security testing. They are optional and are not all required for basic kit deployment.

The source's CAPEv2 procedure changes Windows virtualization/security settings to support nested virtualization and can disable features used by WSL and Rancher Desktop. It is a task-specific preparation procedure, not general laptop optimization. The source calls for a clean dedicated analysis environment, network isolation during malware work, and reimaging afterward. Do not execute an unknown file merely to identify it; preserve it and choose an appropriate controlled analysis method.

**Kali Linux**, Mandiant Commando, and Burp Suite are security-testing environments/tools. Their inclusion describes available capabilities, not blanket authorization to scan or test partner systems. Define the agreed target set and activity before active assessment.

## Availability and source corrections

Lesson 3's asterisks mark unintegrated tools, while Appendix B's asterisks mean open source. Appendix B also lists more tools than the training slides. Volume 3's optional-VM table incorrectly describes Kali as a VMware vMotion network; the software lessons identify it as a Debian-based security distribution. Licensing statements, especially Nessus registration/trial wording, are source snapshots that must be checked against the installed entitlement before planning a mission.

## Related notes

[[JCHK Security Onion Investigation Workflow]] · [[JCHK Velociraptor Endpoint Forensics]] · [[JCHK Collaboration and Reporting]]

## Sources

- [[JCHK Sources#L03|Lesson 3]], PDF pp.25–32, 40, 44: tool purpose and availability.
- [[JCHK Sources#L08|Lesson 8]], PDF pp.44–51: laptop categories and optional VMs.
- [[JCHK Sources#V3|Volume 3]], PDF pp.58–68, 73–74: VM precautions/setup, licensing examples, external SSD handling.
- [[JCHK Sources#APP|Appendices]], PDF pp.12–15: inventory and license snapshot.
