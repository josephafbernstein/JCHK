---
tags: [jchk, foundations, virtualization]
source_version: "0.6.1"
---

# JCHK Virtual Machines and Containers

JCHK runs software in several forms. Some software runs directly on a physical computer; some runs inside a virtual machine; some runs as a container. Understanding the execution form helps you locate the software, know which management interface controls it, and identify what else it depends on.

## Three execution forms

| Form | What it means | JCHK example |
|---|---|---|
| Bare metal | The operating system is installed on a physical machine, rather than inside a VM | JCRS-D Edge host on SN 7100; Security Onion sensor OS on SN 9000/SN 3100 |
| Virtual machine (VM) | A software-defined computer with virtual CPU, RAM, disks, network interfaces, and a full guest OS | Central Security Onion roles managed through KubeVirt |
| Container | An application runtime isolated through host-OS mechanisms, normally sharing the host kernel | Containerized services within the platform; sensor software components |

An **operating system (OS)** manages resources and applications. Its **kernel** provides the core hardware and process-management functions. A **process** is a running program. A **hypervisor** provides the environment in which virtual machines execute. The physical computer is the **host**, and an OS inside a VM is the **guest**.

The terms describe different levels. Calling Security Onion a bare-metal sensor does not mean it has no containers internally: the sensor's OS runs on the physical hardware while its application components can run in containers.

A **Type 1 hypervisor** runs at the hardware virtualization layer without an ordinary host desktop OS beneath it. A **Type 2 hypervisor** runs as an application on a host OS. The lesson uses traditional Type 1 infrastructure as a comparison with KubeVirt's Kubernetes-based management model. The classification alone does not describe the full security or availability of a particular deployment.

## Why use both VMs and containers?

A VM supplies a whole guest operating system, useful for applications that expect a conventional server, a particular OS, or low-level guest access. It brings the memory/storage overhead of that OS. A container packages an application and its required user-space dependencies, generally making it smaller and faster to start. **Dependencies** are other libraries or services the application needs. **User space** is the part of an OS where ordinary application processes run, outside the kernel.

An **image** is a packaged template used to create a container or VM. A running container is an execution instance of its image. A **volume** supplies storage to the workload. **Isolation** limits what one workload can see or affect, but depends on configuration and the underlying software; it is not a blanket guarantee against malicious code or operator mistakes.

Lesson 6's process/user-namespace comparison is oversimplified. A **namespace** in the Linux isolation sense controls which processes or resources a process can see. Separate VMs each have their own guest kernel/process view; they do not share one universal process namespace merely because they are VMs. Containers may or may not share particular namespaces depending on configuration. A Kubernetes namespace, discussed in [[JCHK JCRS-D and Kubernetes]], is a different organizational concept.

## KubeVirt on the central servers

**KubeVirt** extends Kubernetes with virtual-machine resources and controllers. **Kubernetes** orchestrates workloads across nodes. KubeVirt supplies the VM functionality while Kubernetes handles scheduling and associated resources, as explained in the [official KubeVirt architecture guide](https://kubevirt.io/user-guide/architecture/).

The JCHK lesson identifies QEMU/KVM as the virtualization technology. **QEMU** provides machine/device emulation; **KVM**, Kernel-based Virtual Machine, is the Linux kernel virtualization facility. This KVM meaning is distinct from keyboard/video/mouse access in a crash-cart discussion. KubeVirt allows familiar VM-based tools to run on the same managed platform as containers, without requiring every application to be rewritten as a container-native application.

The main objects are:

- **VirtualMachine:** the persistent definition of a VM—its requested properties and lifecycle configuration.
- **VirtualMachineInstance (VMI):** a running instance of that VM. A stored VM definition can exist while no VMI is running.
- **DataVolume:** a storage-import object used to prepare the VM's disk through CDI.
- **NetworkAttachmentDefinition:** a definition of an additional network attachment that a workload can request.

**CDI**, Containerized Data Importer, imports, uploads, or clones disk-image data into persistent storage. A **clone** is a new copy created from an existing source. **Multus** is a Kubernetes networking extension enabling additional interfaces beyond the primary pod connection. A **network interface** is a connection through which a system communicates; virtual interfaces serve that role inside a VM. **CNI**, Container Network Interface, is the plugin interface used by these Kubernetes network components.

**KubeVirt Manager** supplies the browser interface for managing VMs, disks, nodes, and available networks. **Traefik** routes the browser request to the appropriate service. The baseline example URL is `https://kubevirt.jcrsd.kit1.jchk`; it is an internal Kit hostname, so successful access depends on the right network, name resolution, credentials, and certificates. It is not a public internet service.

## Laptop virtualization

Analyst laptops use VMware Workstation Pro as the documented default for packaged VMs; Hyper-V is also listed as available. **OVA**, Open Virtual Appliance, is a bundled VM package. **qcow2** is a virtual-disk image format used in the KubeVirt lesson; it is a disk format rather than a complete synonym for an OVA package.

Laptop VM roles include CAPEv2 for malware analysis, FLARE for reverse engineering, SIFT for digital forensics, Kali and Commando for threat-emulation tooling, and Greenbone/Nessus for vulnerability analysis. **Malware analysis** investigates harmful software; **forensics** examines evidence; **vulnerability analysis** searches for weaknesses. These are tool categories, not authorization to run any activity on a partner network.

The course cautions that malware-analysis VMs require care to avoid contaminating the workstation. **Shared folders** expose files between host and guest, so an exercise that demonstrates them does not imply they belong in every analysis configuration. Read [[JCHK Analyst Tools and Evidence Handling]] before treating a packaged VM as a ready-made containment policy.

## Version and availability caveats

Lesson 7 lists FreeIPA replica, NP-View, and Rsyslog server VMs with a note that they are not present in the current version. Some overview tables nevertheless list a FreeIPA replica and Rsyslog as platform roles. A role diagram describes intent; the installed baseline and actual deployment determine availability. Likewise, cluster support for high availability does not prove that every VM is redundant or that every failure triggers seamless migration.

## Sources

- [[JCHK Sources#L06|Lesson 6]], PDF pp. 11–15: VM/container concepts and simplified comparisons.
- [[JCHK Sources#L07|Lesson 7]], PDF pp. 6–23: workload placement, KubeVirt, Multus, CDI, Manager, object vocabulary, laptop VMs.
- [[JCHK Sources#V1|Volume 1]], PDF pp. 56–60: platform roles and extension placement.
- [KubeVirt architecture](https://kubevirt.io/user-guide/architecture/): general relationship to Kubernetes, used for conceptual clarification rather than an assertion about the installed release.

## Related notes

[[JCHK Start Here]] · [[JCHK JCRS-D and Kubernetes]] · [[JCHK Analyst Workstations and Accessories]] · [[JCHK Software Map and Baseline]] · [[JCHK Analyst Tools and Evidence Handling]] · [[JCHK Source Discrepancies]]
