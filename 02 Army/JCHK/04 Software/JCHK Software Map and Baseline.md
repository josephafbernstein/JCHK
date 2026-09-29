---
tags: [jchk, software, beginner]
baseline: v0.6.1 training snapshot
---
# JCHK Software Map and Baseline

[[JCHK Start Here]] · [[JCHK Sources]]

The Joint Cyber Hunt Kit (JCHK) is a collection of cooperating systems, not one application. Its **software baseline** is the specified set of operating systems, applications, versions, and configurations expected for a release. Learning what each layer does makes the large inventory manageable.

## The layers, from equipment to investigation

| Layer | Plain-language purpose | Documented JCHK examples |
|---|---|---|
| Operating system | Manages a computer's processors, memory, devices, files, and programs | Oracle Linux 9, Rocky Linux 9, Windows 11 Enterprise LTSC |
| Provisioning | Installs and configures systems consistently | Provisioner and JAKD/Zepharis |
| Identity and basic services | Resolves names, coordinates time, and authenticates users or machines | FreeIPA; Citadel for JCRS-D |
| Network boundary | Routes permitted traffic and connects separated sites | pfSense+, WireGuard |
| Shared compute | Runs central applications on several cooperating servers | JCRS-D Edge, Kubernetes, KubeVirt |
| Collection | Observes network packets and endpoint events | Security Onion sensors, Elastic Agent, Velociraptor clients |
| Search and analysis | Makes collected evidence searchable and interpretable | Security Onion Console, Elasticsearch, Zeek, Suricata, Strelka |
| Analyst workspace | Supports detailed examination and communication | Wireshark, FTK Imager, Ghidra, SIFT, Mattermost, productivity tools |

A **virtual machine (VM)** is a software-defined computer with its own operating system. A **container** packages a service and its dependencies while sharing the host's operating-system kernel. **Bare metal** means software runs directly on physical equipment rather than inside a VM. These installation types explain why restarting a laptop application, a VM, and a physical sensor are different actions.

## Where software runs

The three SN 7100 servers support the JCRS-D Edge cluster, which runs central services. Security Onion Manager, Receiver, Search, and Fleet are virtualized here. The SN 9000 and SN 3100 run Oracle Linux and Security Onion directly; these are collection sensors. The FW 3100 and Netgate 6100 MAX run pfSense+. Analyst laptops run Windows and desktop tools, plus optional VMs in VMware Workstation. In the training configuration the Provisioner is itself a VM on a laptop; relocation to an MS 100 appears in the release roadmap.

**LTSC**, Long-Term Servicing Channel, names the Windows servicing model used in the lesson baseline. Some user-guide passages instead say Windows Professional. Treat this as a document inconsistency: inspect the actual image before assuming edition-specific functionality.

## Match the question to a tool

- “What network conversations occurred?” Start with Zeek metadata and Security Onion searches, then inspect packets in Wireshark if needed.
- “Did traffic match a detection rule?” Review Suricata alerts and their supporting context.
- “What did an endpoint do?” Use Elastic Agent telemetry and targeted Velociraptor collection.
- “What is inside this file or disk image?” Use the appropriate forensic tool, such as FTK Imager, RegRipper, or SIFT.
- “What services or weaknesses exist?” Nmap supports network discovery; Nessus and Greenbone support vulnerability assessment. These active tasks require an agreed mission scope and change the traffic being observed.
- “How do we share conclusions?” Use a case record and deployed collaboration tools, keeping evidence references attached to the finding.

**Metadata** describes data, such as a connection's addresses, time, protocol, and byte counts. It is not the same as complete packet contents. **Telemetry** is observational data emitted by a system. **SIEM**, security information and event management, refers to collecting and searching security events from multiple sources. **DFIR**, digital forensics and incident response, combines evidence examination with investigation and response.

## Read release claims carefully

These notes describe the supplied **v0.6.1 training materials**, not proof of the current product. Lesson 3 describes a planned 1.0.0 release on 18 September 2026 and quarterly subsequent updates; it does not establish that those releases occurred.

Lesson 3 marks NP-View, OpenCTI, the FreeIPA replica, Samba, and several desktop tools as not integrated in v0.6.1. Mattermost is contradictory: some lesson slides mark it absent while the operational guide describes a working deployment. A listed license, application, or roadmap item does not prove it is installed and usable. Check the deployed inventory, service health, license state, and task prerequisites.

The asterisk also changes meaning: in Lesson 3 it marks software absent from the training build; in Appendix B it identifies open-source software. Never transfer the meaning between tables.

## Related notes

[[JCHK Service Portal and Identity]] · [[JCHK Security Onion Architecture]] · [[JCHK Analyst Tools and Evidence Handling]] · [[JCHK Collaboration and Reporting]]

## Sources

- [[JCHK Sources#L03|Lesson 3]], PDF pp. 6–8, 10–11, 14–24, 25–44: release snapshot, functions, tools, platform placement, licensing.
- [[JCHK Sources#APP|Appendices]], PDF pp. 10–15: inventories, installation types, asterisk meaning, licenses.
- [[JCHK Sources#V3|Volume 3]], PDF pp. 12, 24, 54–58: operations overview, laptop edition wording, collaboration, optional VMs.
