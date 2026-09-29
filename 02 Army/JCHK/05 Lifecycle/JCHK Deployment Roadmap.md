---
tags: [jchk, lifecycle, deployment]
source_version: "0.6.1"
---
# JCHK Deployment Roadmap

The Joint Cyber Hunt Kit (JCHK) must pass through several different states before it can support a mission. **Deployment** means building and configuring the system. **Startup** means bringing an already configured system back into operation. **Validation** means checking that it actually works. These are different jobs: a routine startup does not require reinstalling operating systems or rerunning every deployment playbook.

This note explains the whole journey. Begin with [[JCHK Start Here]] for the kit's purpose, then follow the links below for each stage. The supplied deployment manual is a provisional v0.6.1 snapshot dated August 1, 2026. Where sources disagree, [[JCHK Source Discrepancies]] records the uncertainty rather than quietly choosing a value.

## The four places in a full deployment

The **Analyst Site** is where users operate their workstations. **Hunt Site 1**, also called the primary hunt site, holds the central computing and analysis services. **Hunt Sites 2 and 3** collect information from other network locations and send selected information to the primary site. A **site** is a functional network location; it is not the same as a **stack**, which is a group of transport cases. Equipment from several stacks can be assembled at one site. [[JCHK Sources#V2|Volume 2]], PDF pp. 12–14; [[JCHK Sources#L05|Lesson 5]], PDF pp. 11, 18–21.

## Before unpacking or rebuilding

Identify the mission configuration: the full three-hunt-site kit or the approved subset. Confirm the inventory, hardware labels, approved installation media, licenses, configuration files, and access information. A **baseline** is the known, validated configuration that the deployment process expects. A **configuration file** stores settings so they can be applied consistently; a **license** authorizes specified software features or use.

The deployment package includes the custom Windows 11 **ISO**, a disk-image file; the Provisioner **OVA**, a packaged virtual machine; site-specific switch configuration files; and optional application installers. The manual calls for a USB drive of at least 32 GB that can be erased. The DataLocker SSD marked `CONTENT` holds several configuration files. An **SSD**, or solid-state drive, stores data electronically without spinning disks. [[JCHK Sources#V2|Volume 2]], PDF pp. 14–18, 33, 37, 66.

Check the workspace before connecting power. Keep airflow clear, use grounded supplies, protect cables from strain or foot traffic, and handle components with **ESD** precautions—measures against electrostatic discharge, a spark that can damage electronics. Lesson 5 states that sustained operation was tested at 32–95 °F, warns against operation above 104 °F, and specifies non-condensing humidity. **Non-condensing** means moisture is not forming liquid droplets on equipment. Storage limits are different from operating limits. [[JCHK Sources#L05|Lesson 5]], PDF pp. 6–9.

> [!warning] Resolve the electrical plan before powering the kit
> The supplied materials give inconsistent circuit recommendations alongside a maximum full-kit load of 9,540 W. Do not treat a slide's circuit count as proof that the power available at a particular site is sufficient. Validate the actual load, voltage, circuit capacity, and distribution plan using the current approved electrical plan. See [[JCHK Source Discrepancies]].

## The deployment sequence and its purpose

| Stage | What changes | Why the next stage depends on it |
|---|---|---|
| 1. [[JCHK Hardware Baselines\|Hardware baseline]] | Firmware, boot, storage-controller, and adapter settings are checked or prepared. | Imaging and automation expect recognizable disks and network interfaces. |
| 2. [[JCHK Analyst Laptop Preparation\|Laptop preparation]] | The approved Windows image is installed. | The deployment workstation needs the expected software and virtual networks. |
| 3. [[JCHK Switch Initialization\|Switch initialization]] | Each switch receives its own site configuration. | Management, provisioning, and operational traffic must reach the correct networks. |
| 4. Temporary cabling | Pre-deployment links join the required equipment and sites. | The Provisioner needs to discover and install systems before normal services exist. |
| 5. [[JCHK JAKD Deployment\|Automated deployment]] | The Provisioner and JAKD build infrastructure and selected tools. | Application and management services must exist before final configuration. |
| 6. [[JCHK Post-Deployment Readiness\|Post-deployment work]] | Final cabling, firewall integration, certificates, time, licenses, and first-run setup are completed. | A successful installation is not yet a verified operational system. |
| 7. [[JCHK Troubleshooting and Recovery\|Validation]] | Connectivity, service health, and data visibility are checked. | The team needs evidence that the intended collection and analysis chain works. |

This sequence follows Volume 2's deployment workflow. The pre-deployment network includes temporary connections and a spare switch used as a **WAN**, or wide-area-network, simulation/interconnection device. After deployment, operational inter-site communication uses site firewalls and **WireGuard VPN tunnels**—encrypted paths between sites over the mission partner network. Temporary provisioning links must be removed as documented. [[JCHK Sources#V2|Volume 2]], PDF pp. 14, 42, 53–60, 64, 76–84; [[JCHK Sources#L05|Lesson 5]], PDF p. 14.

## Plan time, then verify milestones

Volume 2 estimates roughly 15 minutes of hardware configuration per device, one hour of laptop imaging per device, 15 minutes per switch, 45 minutes of cabling, up to an hour to import/configure the Provisioner, about 6.5 hours of automation, and about two hours of final tasks. Its overall planning estimate is 1.5 days for a build from scratch. These are planning figures, not completion guarantees; the individual playbook estimates do not sum exactly to the headline figure. [[JCHK Sources#V2|Volume 2]], PDF pp. 14, 60.

Record the actual configuration, hardware included, completed stages, unresolved errors, and any deviations. **Configuration drift** means a device's settings have moved away from the expected baseline. Unrecorded manual fixes can make the next automated step fail or make a future rebuild behave differently. The manual calls for coordinating deviations through configuration management. [[JCHK Sources#V2|Volume 2]], PDF pp. 53, 142, 146.

## After the mission

Use [[JCHK Shutdown and Pack-Out]] to stop collection and services in dependency order, inventory the equipment, and prepare it for transport. At the next location, decide whether the system needs only [[JCHK Startup]] or a deliberate rebuild. Restoring power is not the same as factory-resetting switches, clearing storage arrays, or reinstalling servers.

Related: [[JCHK Analyst Laptop Preparation]] · [[JCHK Hardware Baselines]] · [[JCHK JAKD Deployment]] · [[JCHK Post-Deployment Readiness]]
