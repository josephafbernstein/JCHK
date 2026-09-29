---
tags: [jchk, foundations]
source_version: "0.6.1"
---

# JCHK What the Kit Is

The **Joint Cyber Hunt Kit (JCHK)** is a transportable collection of computers, network equipment, storage, and software for investigating and defending networks. It gives a **Cyber Protection Team (CPT)**—a team assigned to protect and investigate information systems—the resources to perform **Defensive Cyber Operations (DCO)**: activities that detect, investigate, and respond to threats to those systems.

The useful starting picture is a small security operations center that can travel. Some equipment observes the mission partner's network, some stores and processes the resulting evidence, and analyst laptops provide the interfaces people use to investigate it. **Mission partner** means the organization whose network the team supports. This is a synthesis of the architecture in Volume 1, not a claim that every software feature is present in every fielded kit.

## The problem the Kit solves

A team may need to investigate a network without sending evidence to an external service or depending on the partner to supply servers and storage. The JCHK is designed for this situation. Volume 1 describes standalone operation in disconnected environments and processing/storage supplied by the Kit itself. **Disconnected** means that required external connectivity is unavailable; **offline** means operating without access to an external online service. Neither term means the Kit's own internal network is absent.

For example, a team might observe traffic at three buildings, keep the original captured traffic at each building, and investigate the observations from a separate workroom. The three collection locations are **Hunt Sites**; the workroom is the **Analyst Site**. The central processing servers reside at **Hunt Site 1**. The physical distance can be much smaller: different rooms or network segments in one building still fit this model.

## Five functions to recognize

1. **Collect network evidence.** A **packet** is a unit of data transmitted over a network. A **network TAP** is hardware that duplicates traffic from a network link for a monitoring tool. A **sensor** receives the copies and examines them.
2. **Retain original evidence.** **Packet capture (PCAP)** is recorded packet data. **Full packet capture (FPCAP)** refers to retaining full packet contents within the configured capture policy, rather than only summaries. Storage capacity, traffic volume, and capture settings determine how much history is available.
3. **Create searchable observations.** **Metadata** describes traffic—for example, when a connection occurred and which systems participated. An **alert** is a tool-generated indication that an observation matched a detection rule or condition; it needs investigation before it becomes a confirmed finding.
4. **Investigate computers as well as traffic.** An **endpoint** is a monitored computer or other end device. An **agent** is software installed on an endpoint to gather logs or support authorized investigation. A **log** is a recorded event from a system or application.
5. **Provide shared analysis and collaboration.** Applications allow analysts to search evidence, correlate events, investigate endpoints, and exchange findings. A **workflow** is the ordered set of actions used to reach an investigative result.

Read [[JCHK Data Journey and Resilience]] for an end-to-end example.

## How the physical system is organized

The **Kit** is the whole inventory. A **stack** is one of three repeatable equipment groupings used for packing and modularity. A **case** is a physical transport enclosure. A **site** is a location or operational role. These are different categories: one packed stack does not automatically equal one fully independent operational site in the documented version. The three analytics servers are brought together at Hunt Site 1.

The full inventory includes three analytics servers, six sensors, three small flex servers, nine laptops, six TAPs, nine switches, and six firewalls. The default four-site arrangement uses a subset of the networking inventory; extra equipment should not be mistaken for extra mandatory sites. See [[JCHK Kit Inventory and Packaging]] and [[JCHK Sites and System Architecture]].

## The two variants

The sources call the variants **ON-DoWIN** and **OFF-DoWIN**. They describe the same basic hardware/software design with a different amount of bulk storage in the large SN 9000 sensors. The supplied architecture material does not give a reliable expanded definition of “DoWIN”; the labels should be understood here as the two documented mission variants.

In the listed operational storage requirement, each ON-DoWIN large sensor has six 61.44 TB drives; each OFF-DoWIN large sensor has fourteen. A **terabyte (TB)** is a storage-capacity unit, conventionally one trillion bytes in drive specifications. The additional drives extend retention; they do not make the other variant a different analysis platform. Installed equipment may differ because the source itself allows supply-driven substitutions. [[JCHK Storage Capacity and Retention]] separates raw capacity from usable space.

## What the version means

These notes describe the supplied **v0.6.1 development snapshot**, principally dated August 2026. **Baseline** means a documented reference configuration. It does not certify the condition of a particular Kit today. Lesson 1 labels single-stack and fully co-located deployment models as planned for v1.0.0; they are not established v0.6.1 procedures simply because the physical design is modular.

The documents contain inconsistencies in specifications, addresses, and availability of some services. [[JCHK Source Discrepancies]] keeps those visible. Explanations and examples in these notes are teaching aids; actual operation depends on the authorized configuration, installed version, and applicable procedures.

## Sources

- [[JCHK Sources#V1|Volume 1]], PDF pp. 9–10, 18–29: purpose, variants, architecture, data paths, inventory, and provisional status.
- [[JCHK Sources#L01|Lesson 1]], PDF pp. 6–14: major capabilities and future deployment models.

## Related notes

[[JCHK Start Here]] · [[JCHK Sites and System Architecture]] · [[JCHK Data Journey and Resilience]] · [[JCHK Kit Inventory and Packaging]] · [[JCHK Storage Capacity and Retention]]
