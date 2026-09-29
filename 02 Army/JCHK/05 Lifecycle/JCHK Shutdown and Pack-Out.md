---
tags: [jchk, lifecycle, shutdown, inventory]
source_version: "0.6.1"
---
# JCHK Shutdown and Pack-Out

An orderly shutdown stops incoming work, lets services finish, and then removes their supporting infrastructure. Pulling power from the central cluster first can interrupt writes and leave storage or applications inconsistent. **Graceful shutdown** means allowing an operating system or service to complete its normal stopping process.

The JCHK shutdown procedure is not just the startup list read backward; use the explicit sequence in Appendix D. It estimates approximately 45 minutes, with roughly 30 minutes for the JCRS-D service-shutdown playbook. See [[JCHK Startup]] and [[JCHK Start Here]] for context. [[JCHK Sources#APP|Appendices]], PDF pp. 25–27; [[JCHK Sources#L11|Lesson 11]], PDF pp. 6–13.

## Before stopping collection

Confirm that collection can end, required mission material has been preserved through the approved handling process, and operators know services will become unavailable. **Collection** is the act of receiving traffic, logs, or other evidence. A **backup** is a separate recoverable copy of data or configuration; a powered-off server is not itself a backup.

The supplied shutdown checklist begins by unplugging TAP network and power connections. A **TAP** duplicates traffic from a monitored link, and its physical placement can involve the mission partner's live network. Coordinate the approved removal/restoration method for that actual monitored link before disconnecting inline equipment. The checklist does not establish that unplugging an arbitrary live network connection is disruption-free. [[JCHK Sources#APP|Appendices]], PDF p. 26; [[JCHK Sources#L11|Lesson 11]], PDF p. 6.

## Stop in this order

1. End the TAP collection connections using the approved site procedure.
2. Shut down the three SN 3100 small sensors and three SN 9000 large sensors that are present in the full deployment.
3. Stop running virtual machines in KubeVirt Manager.
4. Stop the core JCRS-D services using the shutdown playbook and wait for completion.
5. Shut down each SN 7100 JCRS-D physical server.
6. Halt firewalls in this order: Hunt Site 3, Hunt Site 2, Hunt Site 1, then Analyst Site.
7. Power off network switches.
8. Power off the Provisioner VM.
9. Power off the analyst/deployment laptops.

The firewall order keeps the core and analyst management paths available while remote systems are being stopped. Do not remove network connectivity early merely because the applications are no longer visible to end users. Appendix D explicitly requires allowing each layer enough time to finish. [[JCHK Sources#APP|Appendices]], PDF pp. 26–27.

## Sensor shutdown: know which machine receives each command

The documented workflow begins inside the Provisioner VM, uses administrative privileges to access configured SSH keys, and connects to each sensor. **SSH**, Secure Shell, opens an encrypted terminal on another host. **Root** is the fully privileged Linux account. The small sensors are `so-sensor-sm-X`; the large sensors are `so-sensor-lg-X`, where `X` is a site number such as `1`, `2`, or `3`.

For one small sensor, the sequence described in the appendix is:

```bash
# On the Provisioner VM:
sudo su -
ssh jchk-admin@so-sensor-sm-1

# After the prompt confirms you are on that sensor:
sudo su -
poweroff
```

`poweroff` shuts down the machine on which it is executed. Confirm the hostname/prompt first: running it on the Provisioner would stop the management machine instead. Repeat through separate sessions for each intended sensor, using `so-sensor-lg-X` for the large sensors. The source expects preconfigured SSH keys, so the lack of a password prompt does not mean that access is unauthenticated. [[JCHK Sources#APP|Appendices]], PDF p. 26; [[JCHK Sources#L11|Lesson 11]], PDF p. 7.

## Stop virtual workloads, then their host services

In KubeVirt Manager, stop all running VMs, including the core and endpoint-related services deployed for this kit. Lesson 11 identifies FreeIPA Replica, Rsyslog, Security Onion Manager/Receiver/Search/Fleet, and Velociraptor. **Rsyslog** handles system-log messages; a **replica** is another instance holding copied service data. Stop any other running VMs as well. The documented interface action is **Actions → Power Off**. Because “Power Off” may not guarantee an application has saved all pending work, finish required application/data handling first and verify that each VM has stopped. [[JCHK Sources#L11|Lesson 11]], PDF pp. 8–9.

Next, from the Provisioner, connect to the first JCRS-D host as the baseline account shown in the source and run:

```bash
ansible-playbook /usr/jcrsd/ansible/playbooks/shutdown.yml
```

An **Ansible playbook** is a set of automated tasks. This playbook stops central JCRS-D services; the guide says to run it once on one JCRS-D server, not independently on all three. Wait for it to complete. Then use the documented remote access procedure to shut down each SN 7100 with `poweroff`, with the required administrative privileges on that server. Stopping the playbook's services and powering off the physical nodes are separate steps. [[JCHK Sources#APP|Appendices]], PDF p. 26; [[JCHK Sources#L11|Lesson 11]], PDF pp. 10–11.

For each pfSense firewall, use **Diagnostics → Halt System** and wait for the halt before proceeding. A **halt** stops the operating system; it differs from factory reset, which removes configuration. Finish with switches, Provisioner, and laptops in the stated order. [[JCHK Sources#L11|Lesson 11]], PDF pp. 12–13.

## Disassemble and inventory

Only handle and pack the hardware after it is fully powered down. Use a clean, dry workspace, keep walkways clear, and use appropriate team lifting for transit cases. **ESD**, electrostatic discharge, can damage sensitive electronics; retain normal protective handling practices. Avoid pressure on laptop screens, fiber cables, and optical modules.

Inventory against Appendix E's device/case checklists, not memory. The training materials recommend checks before the mission, after the mission while still at the customer location, and after transport. Record missing or damaged components at the location where the discrepancy is discovered. Transit cases, backpacks, and the cable bag hold different groups of equipment, so repack to the intended location rather than consolidating everything loosely. [[JCHK Sources#L11|Lesson 11]], PDF pp. 16–20.

Fit dust caps to **optics**, the transceiver modules and optical connectors used for fiber networking. Coil fiber without strain and place it in protective sleeves. Group power cords with the appropriate adapters, secure cables, inspect case seals, and engage every latch. **Transceivers** transmit and receive network signals and can be easy to overlook during a pack-out inventory. These steps protect both the hardware and the next deployment team's ability to reconstruct the intended kit. [[JCHK Sources#L11|Lesson 11]], PDF pp. 16–19.

Related: [[JCHK Deployment Roadmap]] · [[JCHK Startup]] · [[JCHK Troubleshooting and Recovery]]
