---
tags: [jchk, glossary, reference]
source_version: "0.6.1"
---
# JCHK Glossary

Up: [[JCHK Start Here]]

This glossary defines technical terms used throughout the guide. Definitions are intentionally plain-language; each section links to a note that explains the relationships in more depth. Product names describe roles in the supplied source snapshot, not a guarantee that every optional product is installed. Search this note for an acronym or term.

## Mission and architecture

Explore: [[JCHK What the Kit Is]].

| Term | Plain-language meaning |
| --- | --- |
| **Analyst Site** | The location where analysts use laptops to reach kit services. In the documented four-site design, central application servers are at Hunt Site 1. |
| **Architecture** | The organization of components and the relationships between them; it explains where functions run and how they communicate. |
| **Autonomy** | Ability to perform local functions without continuous external connectivity; it does not mean all remote dependencies disappear. |
| **BDP** | Big Data Platform, the enterprise data-platform integration referenced by the JCRS-D course. Integration capability does not prove a particular connection is deployed. |
| **Case** | A physical transport enclosure. In an investigation, a case instead means a record grouping evidence, notes, and decisions. |
| **CPT** | Cyber Protection Team, the defensive cyber team context for which the kit is described. |
| **DCO** | Defensive Cyber Operations: activities intended to protect and defend systems and networks. |
| **Dependency** | Another component or service that a function needs to work; DNS, time, storage, and routing are common examples. |
| **Distributed collection** | Gathering observations at several locations while allowing central analysis of the resulting evidence. |
| **Edge** | Computing located near the mission or data source rather than relying entirely on a distant central facility. |
| **Hunt Site** | A site collecting network data. Hunt Site 1 also hosts the common application cluster; Hunt Sites 2 and 3 are secondary collection sites in the depicted layout. |
| **JCHK** | Joint Cyber Hunt Kit: the deployable hardware, software, and network system described by these notes. |
| **JCRS-D** | Joint Cyber Warfighting Architecture Common Runtime Stack for Data. JCRS-D Edge is the shared application platform on the three SN 7100s. |
| **JCWA** | Joint Cyber Warfighting Architecture, the broader architecture named in the expansion of JCRS-D. |
| **Kit** | The complete supplied equipment and software set, including items that may be spare or unused in a particular topology. |
| **Mission partner** | The organization or network owner whose environment supports the mission or is being investigated. |
| **NET** | New Equipment Training, the familiarization course represented by the supplied slides and student guide. |
| **ON-DoWIN and OFF-DoWIN** | The two source labels for kit variants with different large-sensor bulk storage. The supplied material does not reliably expand DoWIN; do not invent an expansion. |
| **Operational readiness** | Evidence that the required components, connections, services, and data paths work for the intended mission. |
| **Primary Hunt Site** | Hunt Site 1, the hub containing central JCRS-D applications and the VPN hub firewall. |
| **Resilience** | Ability to keep useful functions working or recover when components or links fail; its limits depend on the failed dependency. |
| **Site** | A location or logical role in the operational layout. It is not synonymous with a packed stack. |
| **Stack** | One of the repeatable equipment groupings used for packaging and modularity. The three analytics nodes from the stacks are brought together at Hunt Site 1. |
| **Topology** | The arrangement of devices and connections, physically or logically. |
| **Workflow** | A sequence of related actions that produces a result, such as converting an alert into a documented finding. |

## Network fundamentals

Explore: [[JCHK Network Fundamentals]].

| Term | Plain-language meaning |
| --- | --- |
| **Access port** | A switch port normally assigned to one VLAN, with untagged traffic on the endpoint-facing link. |
| **Broadcast domain** | The set of interfaces that receive a local Ethernet broadcast; a VLAN normally defines such a domain. |
| **CIDR** | Classless Inter-Domain Routing: slash notation such as /24 indicating how many address bits form the network prefix. |
| **Default gateway** | The router used when no more specific route matches the destination. |
| **DHCP** | Dynamic Host Configuration Protocol: supplies leased IP settings to clients. A DHCP pool is the address range available for those leases. |
| **DMZ** | Demilitarized zone: a separate network that hosts selected services exposed through controlled connections. JCHK uses it for endpoint-facing tools. |
| **DNS** | Domain Name System: resolves names to addresses and other records. A resolver is the service a client asks for these answers. |
| **Endpoint** | A host being monitored, such as a workstation; in a VPN discussion, an endpoint instead means the peer's outer network address and port. |
| **Ethernet** | A family of local-network technologies. An Ethernet frame is the local-link container carrying data such as an IP packet. |
| **FQDN** | Fully qualified domain name: a complete name including its domain, such as so-manager.kit1.jchk. |
| **Frame** | A unit of local-link communication; distinguish an Ethernet frame from the IP packet it carries. |
| **Gateway** | A routing next hop that moves traffic toward another network; a default gateway is the fallback route. |
| **Host** | A computer or system on a network. In virtualization, the host is the system that runs guest virtual machines. |
| **Hostname** | The name assigned to a host. Its full domain-qualified form is an FQDN. |
| **Hosts file** | A local text mapping of names to addresses that can affect name resolution on that computer. |
| **HTTP and HTTPS** | Hypertext Transfer Protocol is used for web communication; HTTPS protects it with TLS. |
| **Hybrid port** | A switch port configuration combining tagged VLANs with an untagged VLAN, according to the switch's capabilities. |
| **INFRA** | Infrastructure network label; in JCHK, it generally refers to in-band management on VLAN 10. |
| **Interface** | A physical or logical network connection. Interface names in the operating system may differ from printed port numbers. |
| **IP** | Internet Protocol: addressing and delivery across networks. IPv4 uses 32-bit addresses displayed as four octets. |
| **IP conflict** | Two devices using an address that should uniquely identify one interface within the relevant network. |
| **IPv4 address** | An address such as 10.1.17.11; its subnet prefix determines which portion identifies the network. |
| **LAN** | Local area network. The JCHK firewall LAN side carries internal site networks, often through VLAN subinterfaces. |
| **Layer 2 and Layer 3** | L2 here refers to Ethernet switching/VLANs; L3 refers to IP addressing and routing between networks. |
| **Link indicator** | A light or status display showing physical connection state or activity. It does not establish application health. |
| **Link-local** | An address valid for local-link communication rather than normal routing across networks; IPv4 self-assigned addresses commonly use 169.254/16. |
| **Listener** | A server process waiting for connections or messages at a software port. |
| **Logical network** | A network defined by configuration, such as a VLAN or overlay, rather than simply by cable placement. |
| **Management traffic** | Communication used to administer equipment or software, distinct from the traffic being captured for analysis. |
| **Native VLAN and PVID** | The VLAN association used for untagged frames on a port, according to its configuration. PVID means port VLAN identifier. |
| **Next hop** | The next router or interface selected to forward traffic toward its destination. |
| **NTP** | Network Time Protocol: synchronizes clocks to support authentication and accurate event timelines. |
| **Octet** | Eight bits; each number in a dotted IPv4 address is an octet from 0 through 255. |
| **Overlay and underlay** | An overlay is a virtual network built over another network. The underlay is the underlying transport carrying it. |
| **Packet** | An addressed unit of network data. Capturing a packet records what was visible at a specific observation point. |
| **Ping** | A basic reachability test using Internet Control Message Protocol (ICMP). No reply may mean filtering rather than a failed host. |
| **Port** | A physical connector on hardware, or a numeric software communication endpoint such as TCP 8220. Context must distinguish them. |
| **Private address space** | IP ranges intended for private networks, not globally unique public addressing. Different organizations can use overlapping private ranges. |
| **Protocol** | Rules and message formats that systems follow to communicate. |
| **PROV** | The JCHK label for temporary provisioning traffic; the documented VLAN is 150. |
| **Route** | A rule identifying a destination network and where to forward matching traffic. A static route is explicitly configured. |
| **Router** | A device or software function forwarding IP packets between networks. |
| **Segmentation** | Dividing a network into controlled parts; VLANs, routing, and firewall rules contribute different pieces. |
| **Static address** | An explicitly assigned IP address rather than an automatically leased address. |
| **Subinterface** | A logical interface on a physical network connection, commonly used for one VLAN on a trunk. |
| **Subnet and subnet mask** | A subnet is an address block sharing a network prefix. A mask such as 255.255.255.0 expresses the same boundary as /24. |
| **Switch** | A device connecting local Ethernet interfaces and forwarding frames according to network configuration. |
| **Tagged and untagged** | Tagged Ethernet frames include a VLAN identifier; untagged frames do not. Port configuration decides how each is handled. |
| **TCP** | Transmission Control Protocol: reliable, ordered stream communication between endpoints. |
| **TOOLS** | JCHK's network label for virtualized tool services; the documented VLAN is 30. |
| **Trunk** | A connection configured to carry specified VLANs, normally using tags to distinguish them. |
| **UDP** | User Datagram Protocol: sends individual messages without TCP's delivery and ordering guarantees. |
| **VLAN** | Virtual local area network: a logical Ethernet segment. Its VLAN ID identifies it on the relevant links. |
| **WAN** | Wide area network. In these notes it often means intersite firewall transport, which need not be the public Internet. |

## Firewalls identity and trust

Explore: [[JCHK Mission Partner Connectivity]].

| Term | Plain-language meaning |
| --- | --- |
| **Anti-lockout rule** | A firewall rule intended to preserve administrator access. Its presence on an unintended mission-facing interface can expose administration. |
| **Authentication** | Establishing which identity is accessing a system. |
| **Authorization** | Determining what an authenticated identity is allowed to do. |
| **Certificate** | A digitally signed statement associating an identity, such as a server name, with a public key. |
| **Certificate authority (CA)** | An issuer trusted to sign certificates. A root trust store contains the root certificates a system accepts as trust anchors. |
| **Encryption** | Transforming data so it cannot be read without the appropriate key. Encryption at rest protects stored data; encryption in transit protects communication. |
| **Enrollment** | Registering an endpoint agent with its management service, often using an enrollment token or generated client configuration. |
| **Firewall** | A system that permits or denies traffic according to rules; JCHK firewalls also route and provide VPN functions. |
| **FreeIPA** | The identity/infrastructure suite used for user and host identity, name services, certificates, and related kit functions. |
| **Handshake** | An exchange establishing or refreshing a secure communication session; a WireGuard handshake does not prove every routed application works. |
| **Hub and spoke** | A topology in which each spoke connects to a central hub. Hunt Site 1 is the documented WireGuard hub. |
| **Kerberos** | An authentication protocol using time-sensitive tickets to let services verify identities. |
| **LDAP and LDAPS** | Lightweight Directory Access Protocol accesses directory information; LDAPS protects LDAP communication with TLS. |
| **MISSION** | The dedicated Hunt Site 1 firewall interface for selected partner-to-DMZ service communication; distinct from port-8 WAN transport. |
| **MPN** | Mission partner network: the organization's environment providing mission transport and/or endpoints. |
| **NAT** | Network address translation: rewriting address information as traffic crosses a boundary. It is separate from encryption and traffic authorization. |
| **PAT** | Port address translation: NAT that also translates ports, allowing many internal conversations to share an external address. |
| **Peer** | The other participant in a relationship such as a VPN tunnel. |
| **pfSense Plus** | The firewall/router software running on the documented FW 3100 and Netgate appliances. |
| **Port forward** | A mapping that redirects traffic arriving at a selected external address/port to a selected internal address/port. |
| **Preshared key** | A secret provided to both parties before a protected communication exchange; do not store it in general study notes. |
| **Private key and public key** | Paired cryptographic keys: the private key must remain controlled; the public key can be distributed for verification or encryption, depending on the scheme. |
| **Root** | The highest-privilege Unix/Linux account; separately, a root certificate is a trust anchor. These meanings are unrelated to an ordinary folder name. |
| **SSH** | Secure Shell: an encrypted protocol for remote command-line access and related operations. |
| **TLS** | Transport Layer Security: protects network communication and supports peer identity verification using certificates. |
| **VPN** | Virtual private network: a protected virtual connection carried over another network. |
| **WireGuard** | The VPN protocol used for JCHK site-to-site encrypted tunnels. |

## Hardware cabling and power

Explore: [[JCHK Servers Sensors and Flex Nodes]].

| Term | Plain-language meaning |
| --- | --- |
| **3U and rack unit** | A rack unit (U) is a standard vertical equipment-height unit of 1.75 inches; 3U is three units. Half-width describes rack width, not height. |
| **BMC** | Baseboard management controller: hardware that supports remote power, console, and health management independently of the primary operating system. |
| **CAT6** | Category 6 copper Ethernet cabling. Compatible speed and distance also depend on the interfaces, transceivers, and installation. |
| **Console** | An interface for direct administration, such as a serial terminal or keyboard/video connection. |
| **Core and thread** | A CPU core is a processing engine; a hardware thread is an execution context the processor exposes. Counts do not directly establish workload performance. |
| **CPU** | Central processing unit: the main general-purpose processor. |
| **DAC** | Direct-attach cable: a cable with integrated pluggable ends, used for compatible high-speed copper links. |
| **DDR4 and DDR5** | Generations of double-data-rate memory technology. They identify memory families rather than storage capacity. |
| **ESD** | Electrostatic discharge: a static-electricity event that can damage electronics. |
| **Fiber and optics** | Fiber carries light signals; optical transceivers convert between those signals and the equipment's electrical interface. |
| **Firmware** | Low-level software controlling hardware; BIOS/UEFI and device firmware are examples. |
| **FW 3100** | The SealingTech firewall hardware model used at the Hunt Sites; not the name of the firewall operating system. |
| **GbE and Gbps** | Gigabit Ethernet and gigabits per second. Link speed is measured in bits, while storage capacity is generally measured in bytes. |
| **GPU** | Graphics processing unit: a processor specialized for parallel workloads; graphics memory is its associated working memory. |
| **HDMI and VGA** | High-Definition Multimedia Interface and Video Graphics Array: display-connection standards. |
| **In-band and out-of-band management** | In-band reaches a running OS through its normal network. Out-of-band reaches a separate management controller/path, often labeled MGMT. |
| **Intrusion switch** | A chassis sensor used to indicate physical opening or tampering; a tamper indication is different from a network intrusion alert. |
| **IPMI** | Intelligent Platform Management Interface: a standard for server hardware management, typically implemented by a BMC. |
| **KVM** | In hardware access, keyboard/video/mouse. In virtualization, Kernel-based Virtual Machine, the Linux virtualization subsystem. Always read the context. |
| **LC** | A small optical-fiber connector type used by the listed fiber patch cables. |
| **MGMT** | A management-port label. On the relevant SealingTech equipment, it identifies the out-of-band management connection. |
| **NIC** | Network interface card/controller: hardware providing network connectivity. |
| **Non-condensing** | Humidity conditions in which water does not condense on equipment; condensation can create electrical problems. |
| **OM4 and OS2** | Fiber categories: OM4 is multimode; OS2 is single-mode. Match fiber to the approved optics and link requirements. |
| **PoE++** | A high-power Power over Ethernet category, delivering power over compatible Ethernet cabling. Capability does not mean every kit port supplies it. |
| **Power units** | Watt (W) measures power, volt (V) electrical potential, and ampere (A) current. Source circuit counts conflict; these definitions do not establish safe site loading. |
| **QSFP28** | A quad small form-factor pluggable connector/module family associated with the kit's 100 GbE cluster links. |
| **RAM** | Random-access memory: working memory used by active programs, distinct from persistent drives. |
| **RJ45** | The common name for the modular copper Ethernet connector used in these materials. |
| **SFP, SFP+, and SFP28** | Small form-factor pluggable module families commonly associated here with 1, 10, and 25 GbE. Actual support depends on device and module compatibility. |
| **Smart card** | A card containing a chip used for identity or cryptographic functions when supported by the system. |
| **SN 3100, SN 7100, SN 9000** | SealingTech server model identifiers. The depicted roles are small sensor, central analytics node, and large sensor respectively. |
| **SR-IOV** | Single Root I/O Virtualization: hardware support that exposes virtual functions of a device for assignment to workloads. |
| **Tamper-evident** | Designed to show signs of unauthorized physical access; not a guarantee that tampering is impossible. |
| **Thunderbolt** | A high-speed peripheral interface used by compatible laptop ports and devices. |
| **Transceiver** | A module that transmits and receives signals, adapting equipment to a compatible cable and link type. |
| **USB** | Universal Serial Bus: a peripheral connection standard used for input devices, storage, and adapters. |

## Storage and performance

Explore: [[JCHK Storage Capacity and Retention]].

| Term | Plain-language meaning |
| --- | --- |
| **Backup** | A separate recoverable copy of data/configuration. Redundancy, a VM clone, and a successful shutdown are not automatically complete backups. |
| **Bit and byte** | A bit is a binary digit; a byte is eight bits. Divide a bits-per-second rate by eight before comparing it with bytes of storage. |
| **Bulk storage** | High-capacity storage used for evidence such as packet capture, separate from smaller operating-system drives. |
| **DWPD** | Drive writes per day: a storage-endurance rating describing daily full-drive writes under specified conditions. |
| **GB, TB, and PB** | Gigabyte, terabyte, and petabyte: decimal capacity units of 10^9, 10^12, and 10^15 bytes. Binary units such as GiB/TiB use powers of two. |
| **I/O wait** | Time when processing is waiting on storage input/output; high values can indicate a storage bottleneck, depending on workload. |
| **IOPS** | Input/output operations per second: a measure of storage operation rate, distinct from sustained bytes-per-second throughput. |
| **JBOD** | Just a bunch of disks: drives exposed individually rather than combined into a hardware RAID virtual drive. |
| **Latency** | Time taken for an operation or communication step; low latency and high throughput are different performance goals. |
| **Logical volume or virtual drive** | A storage unit presented to software, which may be built from one or more physical devices. |
| **M.2 and U.2** | Drive form-factor/connection arrangements. They are not themselves promises about capacity or performance. |
| **NVMe** | Non-Volatile Memory Express: a protocol designed for fast storage access, commonly over PCI Express. |
| **Parity** | Additional calculated information that can help reconstruct data after certain drive failures. |
| **RAID** | Redundant Array of Independent Disks: combines drives for selected resilience/performance goals. RAID does not replace backup. |
| **RAID 1** | Mirroring: duplicates data across drives, reducing usable capacity relative to their total raw capacity. |
| **RAID 6** | A RAID layout with dual distributed parity, able to tolerate two member-drive failures within its design assumptions. |
| **Raw and usable capacity** | Raw is the sum of drive capacities. Usable capacity is what remains after parity/replication, formatting, reservations, and other overhead. |
| **Retention** | How long evidence remains available before deletion or overwriting. It depends on usable space, data rate, policy, and evidence type. |
| **SATA** | Serial ATA: a storage interface/protocol family used by some drives and controllers. |
| **SED** | Self-encrypting drive: a drive with built-in encryption capability. Key management and actual configuration still matter. |
| **SSD** | Solid-state drive: non-volatile storage without rotating disks. |
| **Storage controller** | Hardware/software managing storage devices; a RAID controller can present several drives as a virtual drive. |
| **TBW** | Terabytes written: an SSD endurance specification, distinct from the amount of data it can hold at once. |
| **Throughput** | Rate of useful work/data processed. Sequential storage throughput measures sustained transfer, while a network port rating describes a link limit. |
| **Volatile and non-volatile** | Volatile memory loses contents when power is removed; non-volatile storage retains them. Persistence does not imply an independent backup. |

## Virtualization and Kubernetes

Explore: [[JCHK JCRS-D and Kubernetes]].

| Term | Plain-language meaning |
| --- | --- |
| **API** | Application programming interface: a structured way for software to request actions or data from other software. |
| **Bare metal** | Software running directly on physical hardware rather than inside a guest VM. |
| **CDI** | Containerized Data Importer: a Kubernetes extension used with KubeVirt to import and manage VM disk data. |
| **Ceph** | Distributed storage software that pools devices across nodes and provides storage services. |
| **Citadel** | The user-management/authentication component named in the JCRS-D course; its integration state is release dependent. |
| **Clone** | A copy of a VM or disk used to create another instance. It may share identity settings unless deliberately changed. |
| **Cluster** | A group of machines managed together to provide a service or application platform. |
| **CNI** | Container Network Interface: the interface specification and plugin ecosystem used to attach container workloads to networks. |
| **Container** | An isolated application environment sharing the host kernel, rather than running a separate full guest kernel like a VM. |
| **Control plane** | The cluster components that manage desired state, scheduling, and coordination. |
| **Controller and operator** | A controller reconciles actual state toward desired state. A Kubernetes operator applies that pattern to application-specific management. |
| **DataVolume** | A CDI resource describing a VM disk's data import or creation workflow; it works with persistent storage resources. |
| **Declarative configuration** | Describes the desired end state so automation can work toward it, instead of listing only manual actions. |
| **Deployment** | In ordinary kit work, installation/configuration. In Kubernetes, a resource managing replicas and updates of a set of pods. |
| **etcd** | The distributed key/value store used to hold Kubernetes control-plane state. It is not the primary packet-evidence database. |
| **Guest and host** | A guest is a virtualized operating system; the host is the system providing its execution environment. |
| **Hypervisor** | Software that runs VMs. A type 1 hypervisor is directly associated with the hardware layer; a type 2 runs within a host operating-system environment. |
| **Image** | A packaged operating-system or application template; in forensics, an image is an evidence copy of storage. The two uses have different handling requirements. |
| **Ingress** | Incoming access to cluster applications; associated resources/controllers direct external requests to services. |
| **JSON and YAML** | Structured data formats used for configuration and API information. JSON expands to JavaScript Object Notation; YAML to YAML Ain't Markup Language. |
| **Kernel and user space** | The kernel manages core OS resources; user space is where ordinary applications run outside the kernel. |
| **kube-apiserver and kubectl** | The API server accepts Kubernetes requests; kubectl is the command-line client used to issue them. |
| **Kubernetes or K8s** | An orchestration platform that manages applications and resources across cluster nodes. |
| **KubeVirt** | An extension that lets Kubernetes manage virtual machines alongside container workloads. |
| **KubeVirt Manager** | A web interface for creating, inspecting, accessing, and managing KubeVirt VMs. |
| **Load balancer** | Distributes requests or connections among available backends. |
| **Manifest** | A structured declaration of desired Kubernetes objects and their settings. |
| **Multus** | A CNI meta-plugin that supports multiple network attachments for a pod. |
| **Namespace** | A Kubernetes naming and organization boundary for namespaced objects; it does not automatically provide complete security isolation. |
| **NetworkAttachmentDefinition** | A resource describing an additional network attachment used with Multus. |
| **Node** | A machine participating in a Kubernetes cluster, physical or virtual. |
| **Pod** | Kubernetes' basic workload unit, containing one or more containers with shared context. |
| **PV and PVC** | PersistentVolume is a cluster-scoped storage resource; PersistentVolumeClaim is a namespaced request for storage. A source slide incorrectly reverses the PVC scope. |
| **qcow2** | A QEMU virtual-disk format supporting features such as sparse allocation; a virtual-disk file is not the same thing as a complete VM definition. |
| **QEMU and KVM** | QEMU supplies machine emulation/virtualization components; Linux Kernel-based Virtual Machine provides kernel virtualization support. |
| **Replica** | An additional managed copy of a workload or data. Replication behavior and failure tolerance depend on the component. |
| **Reverse proxy** | Accepts a client connection and forwards it to a backend service; Traefik supplies such functions in the described platform. |
| **Rocky Linux and Oracle Linux** | Linux operating-system distributions named in the kit's host/sensor software baseline. |
| **Rook** | A Kubernetes operator framework used to automate management of Ceph storage in this platform. |
| **Runtime** | The software environment that executes an application or workload. |
| **Service** | A Kubernetes resource providing stable access to a set of workloads even when individual pods change. |
| **StorageClass** | A definition of a storage type and its provisioning behavior for Kubernetes workloads. |
| **Traefik** | The reverse-proxy/ingress and load-balancing software named in the kit's application-access design. |
| **VM** | Virtual machine: a software-defined computer running a guest operating system with allocated CPU, memory, disk, and networking resources. |
| **VMI** | VirtualMachineInstance: the KubeVirt object representing a running VM instance; distinct from the persistent VirtualMachine definition. |
| **VMware Workstation** | The desktop virtualization software used on analyst laptops, including the documented Provisioner VM workflow. |
| **Volume** | A unit of storage attached to a workload; its persistence and lifecycle depend on its type and configuration. |

## Deployment and lifecycle

Explore: [[JCHK Deployment Roadmap]].

| Term | Plain-language meaning |
| --- | --- |
| **AHCI** | Advanced Host Controller Interface: a storage-controller operating mode named in firmware configuration contexts. |
| **Air-gapped or offline** | Air-gapped describes intentional separation from external networks; offline simply means disconnected in the relevant context. |
| **Ansible and playbook** | Ansible automates tasks against systems; a playbook specifies tasks and their target settings. |
| **Baseline and configuration drift** | A baseline is a defined reference configuration. Drift is divergence from it, particularly when changes are untracked. |
| **BIOS and UEFI** | Basic Input/Output System and Unified Extensible Firmware Interface: firmware interfaces involved in hardware configuration and booting. |
| **Boot and bootable** | Booting starts a computer's operating system. Bootable media contains the structures needed to start an installation or OS environment. |
| **CLI** | Command-line interface: interaction through typed commands, distinct from a graphical/web interface. |
| **Configuration file** | A file of named settings used by a program or automation. A variable is a named value that can alter behavior. |
| **DEB and RPM** | Software package formats used by different Linux distribution families; package choice must match the intended platform. |
| **Factory reset** | Restoring device defaults, often removing its customized configuration. It is different from restarting the device. |
| **Graceful shutdown** | An orderly stop that gives applications/OS time to finish writes and close services before power is removed. |
| **ISO** | An optical-disc image format commonly used to distribute bootable operating-system installation media. |
| **JAKD** | Joint Automated Kit Deployer: the web-based automation/deployment interface, also associated with the Zepharis name in the course. |
| **License** | Authorization to use software under its terms; a listed tool does not prove its license is activated or its deployment complete. |
| **LTSC** | Long-Term Servicing Channel, a Windows/Office servicing designation referenced by the supplied software lists. |
| **OVA and OVF** | Open Virtual Appliance is a packaged virtual appliance; Open Virtualization Format describes virtual-system packaging/metadata. |
| **Provisioner** | The host/VM supplying JAKD, installation materials, and supporting provisioning services. |
| **Provisioning** | Preparing systems with operating systems, configuration, and software so they can perform their assigned roles. |
| **PXE** | Preboot Execution Environment: network-based booting used to start installation before the normal local OS is available. |
| **Registry and repository** | A container registry stores container images; a package repository stores installable software packages. Windows Registry is a different concept. |
| **Rufus** | A utility used in the supplied workflow to create bootable USB installation media. |
| **Self-hosted** | A service run on infrastructure operated for the kit/team rather than consumed solely as an external hosted service. |
| **Startup and restart** | Startup brings systems into service; restart stops and starts a system again. Neither inherently reinstalls its software. |
| **TFTP** | Trivial File Transfer Protocol: a simple file-transfer protocol used for boot files in the provisioning workflow. |
| **Validation** | Checking observable results against required behavior. End-to-end validation checks the full path, not just one component's status. |
| **Zepharis** | The product/name associated with the kit deployer in the training material; use the actual release's interface labels. |

## Collection analysis and evidence

Explore: [[JCHK Security Onion Investigation Workflow]].

| Term | Plain-language meaning |
| --- | --- |
| **Agent** | Software installed on a monitored host to collect observations or carry out authorized collection tasks. |
| **Alert** | A notification produced when observed activity matches detection criteria; it is a lead to investigate, not proof of compromise. |
| **Artifact** | An item useful as evidence, such as a file, process record, registry value, or log entry. In Velociraptor, an artifact can also mean a named collection definition. |
| **Chain of custody** | A record of evidence handling, possession, and transfers that supports accountability and reproducibility. |
| **Collection** | Gathering evidence; in endpoint tools, it can refer to a specific job requesting selected artifacts from a client. |
| **Confidence and severity** | Confidence expresses support for an interpretation; severity expresses potential importance/impact. A severe alert can still have low evidentiary confidence. |
| **Correlation** | Relating observations across sources, identities, and time to test whether they describe connected activity. |
| **Cross-cluster search** | Searching data across multiple search clusters from a shared query interface; exact architecture depends on node roles. |
| **DFIR** | Digital forensics and incident response: evidence examination and response activities around security incidents. |
| **Digital forensics** | Collecting, preserving, and examining digital evidence to answer specific questions. |
| **EDR** | Endpoint detection and response: host-based visibility and response capabilities. It complements network observation. |
| **False positive and false negative** | A false positive incorrectly flags benign activity; a false negative misses relevant activity. No alerts is not proof that a network is safe. |
| **Forensic image** | A carefully created evidence copy of storage, with acquisition and verification details recorded. |
| **Full duplex and bidirectional** | Full duplex allows simultaneous sending in both directions; bidirectional describes traffic in both directions. Observe both directions when reconstructing a conversation. |
| **Hash** | A fixed-size value calculated from data; cryptographic hashes help check whether evidence bytes changed. |
| **Host log** | A record generated on a computer about events or actions, distinct from traffic observed by a network sensor. |
| **Hypothesis** | A testable explanation or question guiding an investigation. |
| **IDS** | Intrusion detection system: identifies potentially suspicious activity through rules or other detection logic. |
| **Index and indexing** | An index organizes data for efficient search; indexing is the process of preparing/storing it in that structure. |
| **Ingestion** | Receiving and introducing data into a processing or storage pipeline. |
| **Inline TAP** | A traffic access device placed in the observed network path, supplying traffic copies to tools. Installation can affect the live link. |
| **Log** | A recorded event or observation. Logs describe selected facts rather than necessarily preserving all original packet bytes. |
| **Malware analysis** | Examining suspicious software to understand behavior, capabilities, or indicators, using an appropriate isolated environment. |
| **Metadata** | Descriptive information about activity, such as addresses, ports, protocol, and time, rather than all original content. |
| **Monitor interface** | A sensor interface receiving copied traffic for analysis; separate from infrastructure and hardware-management interfaces. |
| **NSM** | Network security monitoring: using network observations to identify and investigate activity. |
| **Packet broker** | A device/function that filters, aggregates, or distributes copied traffic to monitoring tools; a simple TAP need not implement every broker feature. |
| **Packet loss or drops** | Packets not delivered or processed at some point in the capture/analysis path. A percentage needs a defined measurement point and interval. |
| **PCAP and FPCAP** | Packet capture and full packet capture: recorded network packets used as evidence. PCAP also names capture-file formats; it is not a separate network protocol. |
| **Pivot** | Move from one useful observation to related records, such as from an alert to the same host's other connections. |
| **Queue and buffering** | A queue holds work/data awaiting processing; buffering temporarily absorbs differences in arrival and processing rates. |
| **Reverse engineering** | Analyzing a program or file to understand its structure and behavior. |
| **Sandbox and isolation** | A sandbox is a controlled execution environment. Isolation restricts interactions with other systems; merely calling a VM a sandbox does not prove sufficient separation. |
| **Scope** | The authorized systems, data sources, time period, and actions covered by a mission or collection. |
| **Sensor** | A system that receives and processes observed network traffic; JCHK sensors store packet evidence and produce/forward other observations according to role. |
| **SIEM** | Security information and event management: collection, search, correlation, and investigation of security-related events. |
| **Signature** | In detection, a pattern used to identify activity. A cryptographic digital signature instead verifies origin/integrity of signed data. |
| **SOC** | In the JCHK tool interface, Security Onion Console. Elsewhere SOC often means security operations center; do not assume the meanings are interchangeable. |
| **SPAN** | Switched Port Analyzer/port mirroring: a switch-generated traffic copy. Its visibility and loss behavior depend on source and switch configuration. |
| **TAP** | Traffic access point: hardware providing a copy of network traffic to monitoring tools. The depicted ASF01 has network-side N1A/N1B and tool-side T1A/T1B ports. |
| **Telemetry** | Observations sent by a system or agent about activity, state, or events. |
| **Threat hunting** | A deliberate investigation for suspicious activity using questions, evidence, and tested explanations, rather than relying only on automatic alerts. |
| **Time series** | Values recorded over time, such as sensor throughput or storage utilization. |
| **Traceability** | Ability to connect a statement or action to its supporting source, evidence, and record. |
| **Vulnerability** | A weakness that could be exploited; a scanner finding requires interpretation and validation within scope. |

## Software names encountered in the guide

Explore: [[JCHK Software Map and Baseline]].

| Term | Plain-language meaning |
| --- | --- |
| **Cilium** | The cluster networking/security component named in the platform architecture; availability/configuration depends on the deployed baseline. |
| **ElastAlert2** | An alerting component associated with search-based detection workflows in the software inventory. |
| **Elastic Agent and Fleet** | Elastic Agent collects endpoint telemetry; Fleet manages enrollment/policy for agents and associated communication. See the dedicated endpoint note. |
| **Elasticsearch** | The search and analytics engine that indexes data for investigation; indexed metadata and original PCAP have different storage roles. |
| **Grid** | Security Onion's view of deployment members and health, used to inspect node status and related metrics. |
| **InfluxDB** | A database designed for time-series information, named in monitoring components. |
| **Kali Linux** | A Linux distribution containing security-testing tools; tool availability does not authorize testing outside mission scope. |
| **Kibana** | A web interface for exploring and visualizing Elasticsearch data. |
| **Logstash and Redis** | Logstash processes/transports events; Redis supplies in-memory data/queue capabilities in the documented data pipeline. |
| **Mattermost** | Team messaging/collaboration software. Its availability conflicts across supplied v0.6.1 sources, so verify the deployed service. |
| **Navigator** | The JCRS-D application portal named in the platform lessons. |
| **NiFi** | Apache dataflow software used to move and transform data through defined pipelines. |
| **Rsyslog** | Software for receiving, processing, and forwarding log messages; listed availability varies across the source baseline. |
| **SCAP and STIG** | Security Content Automation Protocol supports machine-readable security checks; Security Technical Implementation Guides describe security configuration requirements. |
| **Security Onion** | The kit's network/host security monitoring and investigation platform, combining multiple collection, search, and analysis components. |
| **Security Onion node roles** | Manager coordinates and exposes investigation access; receiver handles incoming data; search stores/indexes it; forward sensor captures/processes traffic; heavy roles combine additional local analysis/search responsibilities. |
| **Strelka** | A file-analysis/scanning component associated with the sensor pipeline. |
| **Suricata** | A network detection and packet-processing engine, including the documented capture role in this baseline. |
| **Sysmon** | A Windows system-monitoring tool that records selected host activity for investigation. |
| **Velociraptor** | An endpoint digital-forensics/collection platform used to request artifacts and investigate hosts. |
| **Windows Registry** | Windows' hierarchical configuration database; distinct from a container-image registry. |
| **XML** | Extensible Markup Language: a structured text format used by some configuration and compliance data. |
| **YARA** | A rule language/tool used to identify patterns in files or other data during analysis. |
| **Zeek** | A network analysis engine that parses activity and produces structured protocol metadata. |

## Source context

The main terminology references are [[JCHK Sources#APP|Appendices]], PDF pp. 48–54 (acronyms and glossary), together with the cited source pages in each linked topic. General definitions explain those terms rather than treating every source expansion as correct. In particular, PCAP means packet capture, KVM has two context-dependent meanings, and Kubernetes PVCs are namespaced. The corrections and source conflicts are explained in [[JCHK Source Discrepancies]] and [[JCHK JCRS-D and Kubernetes]].
