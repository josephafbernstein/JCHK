---
tags: [jchk, lifecycle, jakd, automation]
source_version: "0.6.1"
---
# JCHK JAKD Deployment

**JAKD**, Joint Automated Kit Deployer, is the web interface used to select the JCHK's deployment layout and run its automated installation tasks. The **Provisioner VM** is the virtual machine that contains JAKD, installation media, and supporting services. **Provisioning** means preparing computers with their operating systems, network settings, and software.

The Provisioner provides the materials needed for an **air-gapped installation**, meaning the installation can operate without internet connectivity. It runs on an analyst laptop in **VMware Workstation**, software that hosts VMs. Only one Provisioner VM is needed for the documented deployment. [[JCHK Sources#V2|Volume 2]], PDF pp. 53–54. For the overall sequence, see [[JCHK Deployment Roadmap]] and [[JCHK Start Here]].

## What automation depends on

Hardware baseline, switch settings, power, and pre-deployment cabling must already be correct. The software cannot install through a missing cable or compensate for the wrong storage layout. **Configuration drift**, meaning untracked differences from the validated configuration, can break dependencies and cause a partly completed deployment.

The core services include:

| Service or term | Plain-language function |
|---|---|
| PXE, Preboot Execution Environment | Starts a machine from the network so it can be installed before its normal OS is ready. |
| DHCP, Dynamic Host Configuration Protocol | Supplies network address settings. |
| DNS, Domain Name System | Resolves computer/service names to network addresses. |
| TFTP, Trivial File Transfer Protocol | Transfers small boot files in the provisioning workflow. |
| Repository | Stores software packages that installation tasks retrieve. |
| Registry | Stores container images—packaged applications and their runtime contents. |
| FreeIPA | Supplies identity-related services, names, certificates, and other kit infrastructure functions. |
| IPMI | Provides access to hardware management outside the installed OS. |

The Provisioner needs the correct path to each service and device. Volume 2 calls out VLAN/trunk configuration, visible link lights, routing/VPN availability where applicable, IPMI access, and boot-service reachability. [[JCHK Sources#V2|Volume 2]], PDF p. 53.

## Import the Provisioner, then preserve adapter order

An **OVA**, Open Virtual Appliance, is a portable VM package. Open the approved Provisioner OVA in VMware Workstation, choose a VM name, and let the import complete; the guide allows up to an hour because the package is large.

After import, map its three virtual network adapters in this documented order:

| Adapter | VMware custom virtual network |
|---|---|
| Network Adapter | `INFRA-VLAN10` |
| Network Adapter 2 | `IPMI-VLAN20` |
| Network Adapter 3 | `PROVISIONING-VLAN150` |

A **virtual network adapter** is a VM's software-defined network connector. The names above are the supplied deployment mappings, not arbitrary labels. Reversing them may connect a deployment service to the wrong traffic segment. Power on the VM and use the assigned login information. The guide opens JAKD at `https://jakd` from inside the Provisioner. A short name such as `jakd` depends on the intended local naming configuration. [[JCHK Sources#V2|Volume 2]], PDF pp. 54–57.

## Tell JAKD what exists and what to deploy

The configuration can be uploaded through **Settings → Configurations** or selected in the interface. For an uploaded file, verify that it is successfully loaded and marked as the current configuration.

In **Hardware Configuration**, cases and backpacks are assigned to sites. An item left in **Unconfigured** is excluded from the deployment. In **Software Layout**, software is assigned to hardware, and its variables can be edited. A **variable** is a named setting that changes how a deployment task behaves. Manual layout selection inside JAKD is part of the supported process; it differs from making unrelated manual changes on target devices outside automation.

Before running anything, check that the selected hardware, software roles, network plan, and kit domain agree. The **kit domain** is the naming suffix used by services, such as the source's example `kit1.jchk`. The **kit network** is the overall address allocation; changing it also requires matching switch management and Analyst Site firewall changes. [[JCHK Sources#V2|Volume 2]], PDF pp. 15, 57–59.

## Run playbooks in dependency order

**Ansible** is the automation engine. A **playbook** is an ordered set of configuration tasks it runs against specified systems. These deployment playbooks can install or reconfigure systems; they are not passive tests.

| Order in the v0.6.1 table | Purpose | Source estimate |
|---|---|---|
| `00 Initialization` | Prepares the Provisioner's deployment services. | 30 minutes |
| `01 Deploy Infrastructure` | Deploys pfSense firewalls, Oracle Linux sensor operating systems, and the custom Rocky Linux 9 JCRS-D hosts. | 50 minutes |
| `02 Configure JCRS-D` | Configures the central cluster and supporting software. | 45 minutes |
| `03 Deploy Security Onion` | Deploys Manager, Receiver, Search, Fleet, and sensor roles. | 4 hours |
| `04 Deploy Mattermost` | Deploys the team communication server if included. | 15 minutes |
| `05 Deploy Velociraptor` | Deploys the endpoint-investigation server if included. | 15 minutes |

A **cluster** is a group of computers/services cooperating as a system. **Bare metal** sensors run on physical machines rather than inside VMs. The core infrastructure stages are required for a new deployment; the source describes tool stages as selectable according to need. Volume 2's nearby prose says to begin at `01`, but its numbered table includes `00` and explicitly says `00 → 05`. Preserve the initialization dependency and confirm the matching release's actual playbook list. See [[JCHK Source Discrepancies]]. [[JCHK Sources#V2|Volume 2]], PDF pp. 59–60.

In JAKD's deployment page, select a playbook, start **New Run**, and wait for the result before proceeding. A green checkmark is the guide's success indicator. If a run fails, inspect its error and prerequisite state rather than repeatedly launching later stages. [[JCHK Sources#V2|Volume 2]], PDF pp. 61, 145.

## Installation success versus readiness

After successful deployment, the guide restarts JAKD services with `sudo systemctl restart jakd-*` on the Provisioner. `sudo` requests administrative privileges, `systemctl` controls Linux services, and `restart` stops and starts matching JAKD services to reload the Kit Operations page. It can interrupt JAKD access briefly; it is not a whole-kit reboot.

Then use **Kit Operations** to open FreeIPA, pfSense, Security Onion, and Velociraptor and verify intended logins. The page includes access information, so it should be treated as sensitive rather than copied into a general note. Mattermost requires its first-run initialization before ordinary login is available. These checks establish that services were deployed and reachable; they do not replace [[JCHK Post-Deployment Readiness]] or an end-to-end data test. [[JCHK Sources#V2|Volume 2]], PDF pp. 61–63, 89–92.

Related: [[JCHK Switch Initialization]] · [[JCHK Hardware Baselines]] · [[JCHK Troubleshooting and Recovery]] · [[JCHK Startup]]
