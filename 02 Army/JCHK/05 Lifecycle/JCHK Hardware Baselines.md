---
tags: [jchk, lifecycle, hardware, deployment]
source_version: "0.6.1"
---
# JCHK Hardware Baselines

JCHK deployment automation expects the hardware to present its disks, network ports, and boot devices in a known way. Preparing this **hardware baseline** is the work done before an initial build or approved rebuild. It is not a normal step during [[JCHK Startup]].

Start at [[JCHK Start Here]] if the component names are new. This note explains the reason behind the settings and the expected results; the device-specific screenshots in Volume 2 remain necessary when selecting an exact controller or firmware menu.

## The terms that matter

**Firmware** is software stored on a hardware device that controls low-level behavior. **BIOS**, the Basic Input/Output System, is the common name used in the guide for a system's firmware setup. **UEFI**, Unified Extensible Firmware Interface, is the modern firmware/boot environment used to access the documented controller utilities. **Booting** means loading the operating system, or OS, after power-on.

A **BMC**, or baseboard management controller, is a separate management processor. **IPMI**, Intelligent Platform Management Interface, provides out-of-band hardware management through that controller. **Out-of-band** means the management path can operate separately from the main OS. The dedicated `MGMT` connector is therefore not interchangeable with every ordinary network port.

A **NIC**, or network interface card, connects a system to a network. A **storage controller** connects and manages disks. Its **logical volume** or **virtual drive** is the disk-like object it presents to the OS. A **physical drive** is the actual storage device installed in the chassis.

## Identify the device and access method

| Device | Enter firmware setup | Management and storage points to check |
|---|---|---|
| Dell Pro Max 16 Plus laptop | `F2` | Laptop storage and boot settings; no BMC listed. |
| SN 7100 | `Delete` | Dedicated IPMI; OS RAID controller; Broadcom 9560 bulk-storage controller; Intel E810 adapter. |
| SN 9000 | `Delete` | Dedicated IPMI; OS RAID controller; Broadcom 9670 bulk-storage controller; Intel E810 adapter. |
| SN 3100 and FW 3100 | `Delete` | Dedicated IPMI and validated firmware settings; the quick-reference table lists no separate add-on RAID/NIC utility. |

Use local keyboard/monitor access or the documented remote console as appropriate. First identify the physical node and its controller model; similarly named menus can manage completely different disks. [[JCHK Sources#V2|Volume 2]], PDF pp. 16–17.

## Laptop firmware baseline

Volume 2 restores factory defaults and then checks the following laptop settings:

| Setting | Documented value | Meaning |
|---|---|---|
| SATA/NVMe operation | `AHCI/NVMe` | The OS sees storage through the expected interface. AHCI is a SATA-controller interface; NVMe is the protocol used by modern PCIe storage. |
| Intel TXT | Off | Trusted Execution Technology is a hardware trust feature; this is the specific image's documented baseline. |
| Secure Boot | Off | Secure Boot verifies permitted boot software; the supplied image procedure specifies it disabled. |
| Thunderbolt Boot Support | On | Allows the relevant external hardware path to participate in startup. |
| USB PowerShare | On | Enables the laptop's supported USB power-sharing behavior. |

These are JCHK v0.6.1 build settings, not general recommendations for every computer. Save the intended settings and verify the resulting state. The source repeatedly says “stop here” after individual checks even though later checks follow; read the entire baseline list. [[JCHK Sources#V2|Volume 2]], PDF pp. 98–106.

## Storage is deliberately different on the two large server types

**RAID**, Redundant Array of Independent Disks, combines drives into logical storage. **RAID 1** mirrors data between two drives. **RAID 6** uses distributed parity information and can tolerate two member-drive failures. **JBOD**, Just a Bunch of Disks, exposes individual drives rather than combining them into a hardware RAID array. JBOD does not itself provide redundancy; another storage layer may do that.

| Storage role | Controller and devices in the source | Baseline |
|---|---|---|
| SN 7100 OS | Supermicro AOC-SLG4-2H8M2, using Broadcom SAS3808 utilities; two 2 TB M.2 NVMe drives | RAID 1 |
| SN 9000 OS | Same OS-controller arrangement; two 2 TB M.2 NVMe drives | RAID 1 |
| SN 7100 bulk data | Broadcom 9560-16i; three 61.44 TB U.2 NVMe drives | JBOD, for software-defined storage |
| SN 9000 bulk data | Broadcom 9670-24i; six or fourteen 61.44 TB U.2 NVMe drives depending on kit variant | RAID 6 |

**M.2** and **U.2** describe drive form-factor/connection arrangements. **Software-defined storage** means software above the controller manages storage organization and resilience. The source explicitly says OS RAID 1 is required for its automation. Do not substitute a different layout simply because the controller offers it. [[JCHK Sources#V2|Volume 2]], PDF pp. 109–110, 115, 119, 125–126.

> [!danger] Clearing an array is destructive rebuild work
> Volume 2's clean-build procedures clear existing controller configurations before creating new arrays or switching between RAID and JBOD. That can make persistent data unavailable or destroy it. It is not a harmless repair for a slow application or a routine reboot. Confirm the intended rebuild, data disposition, and exact controller before using those procedures.

After configuration, check that all expected drives are detected, the intended logical volumes exist, no array is degraded, firmware matches the approved version, and the correct boot device is visible. **Degraded** means the array has lost part of its normal redundancy or functionality. RAID protects against some hardware failures; it is not a substitute for a separate backup. [[JCHK Sources#V2|Volume 2]], PDF pp. 109–110, 119–137.

## Network-adapter baseline and source gaps

The Intel E810-CQDA2 has different port configurations. The deployment guide requires `2x1x100`: two 100 GbE ports, where **GbE** means gigabits-per-second Ethernet. Other modes divide lanes into lower-speed interfaces. The guide allows changing that setting after deployment on SN 9000 sensors only; it does not give equivalent permission for the central SN 7100 hosts. [[JCHK Sources#V2|Volume 2]], PDF pp. 138–141.

Volume 2 says the SN 7100/SN 9000 procedure will configure **SR-IOV**, a feature that exposes virtual functions of a PCIe device to workloads, along with NVMe firmware source and UEFI boot mode. The shown text then only restores optimized defaults. Do not invent missing menu values: verify the current approved baseline. This gap is recorded in [[JCHK Source Discrepancies]]. [[JCHK Sources#V2|Volume 2]], PDF pp. 106–108.

Related: [[JCHK Deployment Roadmap]] · [[JCHK Analyst Laptop Preparation]] · [[JCHK Troubleshooting and Recovery]]
