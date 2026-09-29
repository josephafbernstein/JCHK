---
tags: [jchk, lifecycle, startup, operations]
source_version: "0.6.1"
---
# JCHK Startup

Startup brings an already deployed JCHK back into operation. It assumes that the software is installed and the post-deployment cabling is complete. It is different from [[JCHK JAKD Deployment]], which installs/configures the system, and from [[JCHK Hardware Baselines]], which can include rebuild operations that change firmware or storage settings.

Appendix D gives the startup sequence and estimates about 45 minutes overall, including about 30 minutes for the central services playbook. Timing is a planning guide: advance when a stage has actually completed and its dependencies are healthy. [[JCHK Sources#APP|Appendices]], PDF p. 25; [[JCHK Sources#L05|Lesson 5]], PDF pp. 25–27.

## Why order matters

Applications depend on network paths, naming, identity, storage, and compute resources. Starting a sensor before the services it reports to are ready may leave it unable to register or forward information. Starting every physical device simultaneously also creates a large electrical load. **Dependency** means one component needs another component to function correctly.

Before beginning, confirm the correct site layout, available power, ventilation, intact cables, and the intended access credentials. Use [[JCHK Deployment Roadmap]] for environmental and planning context. The source's circuit recommendations are inconsistent; the electrical plan needs a validated capacity calculation, not just a memorized circuit count. See [[JCHK Source Discrepancies]]. [[JCHK Sources#L05|Lesson 5]], PDF pp. 7–10, 25.

## Documented startup sequence

| Step | Start or verify | Reason to keep the order |
|---|---|---|
| 1 | Analyst workstations | Provide the operator's local controls and the deployment laptop's VM host. |
| 2 | QNAP network switches | Establish local physical/logical connectivity. |
| 3 | FW 3100 and Netgate firewalls | Establish the expected routing and inter-site connectivity. |
| 4 | Provisioner VM | Makes the central deployment/management environment available. |
| 5 | SN 7100 JCRS-D Edge nodes | Brings the cluster's physical compute hosts online. |
| 6 | JCRS-D Edge services | Re-establishes the services the virtual workloads depend on. |
| 7 | Verify required VMs in KubeVirt Manager | Ensures the analysis/collection services are running before sensors begin. |
| 8 | SN 3100/SN 9000 sensors and TAP-connected devices | Restores network observation after the receiving services are ready. |
| 9 | Security Onion Grid health | Confirms the distributed monitoring system is healthy. |

This table follows the numbered Appendix D sequence. The dependency explanations make the sequence easier to understand; they are explanatory reasoning, not additional source commands. [[JCHK Sources#APP|Appendices]], PDF p. 25.

## Starting central services

**JCRS-D Edge** is the kit's central cluster platform. A **node** is one computer in that cluster. **Ansible** is the automation tool, and a **playbook** is a file describing the tasks it should perform.

The source directs the operator to log into the Provisioner VM, open a terminal, become root, connect to the first JCRS-D node, and run the startup playbook:

```bash
sudo su -
ssh bsmith@jcrsd-1
ansible-playbook /usr/jcrsd/ansible/playbooks/flux.yml
```

These are contextual commands, not a single command to paste into an arbitrary machine. `sudo su -` changes to the privileged **root** account on the machine where it is run. `ssh` opens an encrypted remote terminal; after connecting, commands execute on `jcrsd-1`. The `bsmith` account is the account printed in the provided baseline, not a promise that every later kit uses that name. The guide expects SSH-key authentication—proof of possession of an authorized key—so a password prompt is not expected in that baseline.

The `ansible-playbook` command runs the JCRS-D startup workflow. It changes service state and is not a read-only check. Wait for it to finish, approximately 30 minutes in the guide. If it fails or the host/account is not as documented, capture the error and use [[JCHK Troubleshooting and Recovery]] rather than substituting a different account or guessing at commands. [[JCHK Sources#APP|Appendices]], PDF p. 25; [[JCHK Sources#L05|Lesson 5]], PDF p. 26.

## Verify virtual machines before sensors

**KubeVirt Manager** is the interface used to manage VMs running on the cluster. A **VM**, virtual machine, is a software-defined computer. From the Provisioner, open the manager and confirm the intended deployed services are running.

Appendix D lists the Security Onion Manager, three Receiver VMs, six Search VMs, one Fleet VM, and Velociraptor. **Manager** coordinates the Security Onion deployment, **Receivers** accept incoming data, **Search** nodes support storing/searching it, and **Fleet** manages endpoint-agent integration. The exact count must match the deployment selected for this kit; a deliberately excluded optional workload should not be “fixed” by inventing a replacement.

The appendix spells Receiver as `so-reciever-*` in its startup checklist. Use actual deployed VM names shown by JAKD/KubeVirt rather than treating that spelling as authoritative. Record any mismatch with [[JCHK Source Discrepancies]]. [[JCHK Sources#APP|Appendices]], PDF p. 25.

## Prove that the startup is complete

After sensors and **TAPs**, traffic-duplication devices, are started and connected appropriately, open the **Security Onion Console**, or SOC. Check that the intended **Grid members**, the components of the distributed Security Onion installation, report healthy.

Health is necessary but still differs from visibility. With an approved test traffic source, confirm the intended sensor receives traffic and recent logs or packet data appear. **PCAP**, packet capture, is a record of observed network packets; a healthy service with no input may simply have nothing to capture. Avoid diagnosing “no alerts” as a failed boot until you have checked whether the expected traffic is present and whether the selected time range includes it. The training objectives explicitly distinguish readiness, traffic ingest, and analysis evidence. [[JCHK Sources#APP|Appendices]], PDF p. 25; [[JCHK Sources#SG|Student guide]], PDF p. 16.

Related: [[JCHK Post-Deployment Readiness]] · [[JCHK Shutdown and Pack-Out]] · [[JCHK Troubleshooting and Recovery]] · [[JCHK Start Here]]
