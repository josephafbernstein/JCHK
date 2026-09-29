---
tags: [jchk, hardware, workstations]
source_version: "0.6.1"
---

# JCHK Analyst Workstations and Accessories

The **analyst workstation** is the computer the operator uses directly. In the JCHK, it provides access to shared Kit applications and runs local investigation tools. It is also a place to run specialist virtual machines, so its purpose is broader than simply displaying a remote web page.

## The supplied laptop

The v0.6.1 hardware baseline lists nine Dell Pro Max 16 Plus laptops. Each has an Intel Core Ultra 7 265HX processor with twenty cores/twenty threads, 64 GB of DDR5 memory, two 1 TB NVMe solid-state drives, and a 24 GB NVIDIA RTX Pro 5000 Blackwell graphics processor.

The **CPU** performs general computation; a **core** is a processing unit within it. **RAM** is working memory. An **SSD** is persistent solid-state storage. **NVMe** is the storage-interface protocol. The **graphics processing unit (GPU)** is a processor designed for highly parallel work such as graphics and certain machine-learning workloads. Its **graphics memory** is separate from the laptop's 64 GB of system RAM. The hardware's capability does not establish that a particular artificial-intelligence application or model is installed or approved.

| Interface or feature | Listed configuration | What it is used for |
|---|---|---|
| Wired Ethernet | One 2.5 GbE RJ45 port | Connection to the site network |
| USB Type-C / Thunderbolt | One Thunderbolt 4 and two Thunderbolt 5 ports | Compatible high-speed peripherals, display connectivity, and power functions |
| USB Type-A | Two USB 3.2 ports | Keyboard, mouse, removable media, adapters |
| HDMI | One HDMI 2.1 output | External display |
| Card readers | SD and smart-card readers | Removable SD media and compatible authentication cards |
| Audio | Universal audio jack | Compatible wired audio peripheral |
| Power | 280 W adapter listed | Laptop power and charging |

**RJ45** is the connector style used for the copper Ethernet port. **GbE** expresses Ethernet speed in gigabits per second. **USB** is a standard peripheral connection family; Type-A and Type-C describe connector shapes. **Thunderbolt** adds high-speed data/display capabilities over compatible Type-C connections. **HDMI** is a digital display/audio interface. A **smart card** can hold identity credentials used for authentication.

## Local tools and server tools are different

Server applications remain on the Hunt Site 1 analytics cluster even when their interface opens in a laptop browser. A browser is a client: software that accesses a service supplied elsewhere. Losing the laptop-to-site connection can therefore make a healthy server application appear unavailable to that analyst.

Other tools run locally. The sources include packet-analysis applications, reverse-engineering applications, productivity tools, and virtual-machine packages. **Reverse engineering** investigates how compiled software works. A **virtual machine (VM)** provides a separate guest operating system on the laptop. The documented default for the supplied laptop VMs is VMware Workstation Pro; Windows Hyper-V is also listed as available. A **hypervisor** is the software layer supporting virtual machines. See [[JCHK Virtual Machines and Containers]] for how this differs from KubeVirt on the central servers.

The supplied **OVA**, Open Virtual Appliance, is a packaged VM distribution. Importing an OVA creates or registers a VM for use; it does not mean that every packaged tool is already running. The software inventory describes both installed applications and available packages, so availability and active execution must be checked separately.

## Wireless capability requires careful wording

The laptop specification says there is no built-in Wi-Fi, Bluetooth, camera, or microphone. Appendix A/E also list nine USB Wi-Fi adapters. Those facts are compatible: a laptop can lack an integrated radio while an external radio is supplied as an accessory. The presence of an adapter does not establish that wireless use is part of the active mission configuration. The standard cabling diagrams show wired workstation connections.

The operating-system edition disagrees between sources: Volume 1 PDF p. 59 says Windows 11 Enterprise; Appendix B PDF p. 11 says Windows 11 Pro. Treat “Windows 11 laptop baseline” as the common fact and record the installed edition during inventory. [[JCHK Source Discrepancies]] tracks the conflict.

## Accessories that matter when something fails

| Accessory | Purpose and distinction |
|---|---|
| USB-to-RJ45 console cable | Direct serial administration of supported switches/TAPs/firewalls; not an ordinary Ethernet patch cable |
| USB/VGA crash-cart adapter | Uses a laptop to obtain direct keyboard/video/mouse-style access to a compatible server |
| Portable monitor and wired keyboard/mouse | Direct local access without relying on the production network |
| External DataLocker SSD | Portable encrypted storage listed in the inventory; handling still depends on the evidence policy |
| Cable tester and termination tools | Checks or repairs appropriate copper cabling; not a test of application health |
| Fiber and transceiver supplies | Adapts connectivity to the actual link medium and speed |

**VGA** is an analog video interface. In the crash-cart context **KVM** means keyboard, video, and mouse; in virtualization it can instead mean Kernel-based Virtual Machine. The surrounding topic determines the meaning. **Encryption at rest** protects stored data when the relevant keys are unavailable; it does not automatically govern how data is shared after the drive is unlocked.

## Handling and support context

The source specifically cautions against stacking operating laptops because magnetic lid sensors can cause unexpected behavior, and it calls for stable, ventilated surfaces and supplied power adapters. The portable monitor, console cable, and crash-cart adapter are alternative access methods when ordinary network management is unavailable; they do not bypass the need for valid credentials.

The sources describe three-year hardware support, with contract-specific terms. The support portal recorded in the source is `https://sealingtech.samanage.com/`; the support email is `support@sealingtech.samanage.com`. These are documentary references, not a verification of current warranty eligibility or an instruction to send credentials or evidence. The supplied material's drive-retention warranty wording and destruction-standard citation need confirmation through the applicable support process.

## Sources

- [[JCHK Sources#V1|Volume 1]], PDF pp. 16, 42–45, 52–61: laptop hardware, precautions, accessories, software roles, support.
- [[JCHK Sources#L02|Lesson 2]], PDF pp. 34, 40–45: laptop and accessory specifications, support statements.
- [[JCHK Sources#L07|Lesson 7]], PDF pp. 20–23: laptop hypervisors and OVA workflows.
- [[JCHK Sources#APP|Appendices]], PDF pp. 8–11, 30–45: wireless adapters, OS-edition discrepancy, backpack inventory.

## Related notes

[[JCHK Start Here]] · [[JCHK Kit Inventory and Packaging]] · [[JCHK Virtual Machines and Containers]] · [[JCHK Analyst Tools and Evidence Handling]] · [[JCHK Source Discrepancies]]
