---
tags: [jchk, guide]
source_version: "0.6.1"
---
# JCHK Hunt Site 1 Cabling

Up: [[JCHK Start Here]] · Prerequisite: [[JCHK Cabling and Deployment States]]

## Identify the devices before following ports

Hunt Site 1 contains the central three-server **JCRS-D cluster** plus one large sensor, one small sensor, a site firewall, and the deployment laptop in the supplied diagrams. A **cluster** is a group of computers operating together. The three SN 7100s are named `jcrsd-1`, `jcrsd-2`, and `jcrsd-3`; the source sometimes shortens the cable-table names to `jcrs-1` and so on. They are the same intended cluster nodes, not three extra servers.

Switches 1, 2, and 3 are QNAP QSW-M7308R-4X models associated with the three SN 7100s. Switch 4 is the QSW-M3216R-8S8T connecting the sensors and deployment laptop to the site network. The large sensor is `so-sensor-lg-1` (SN 9000); the small sensor is `so-sensor-sm-1` (SN 3100); the firewall is `fw-site-1` (FW 3100).

These are reference connections transcribed from the supplied Wiring Guide, not an assertion that every actual kit matches it. Cross-check physical labels and the deployed switch configuration. The small-sensor provisioning port is inconsistent across sources and is flagged below.

## Persistent cluster connections

**QSFP28 DAC** means the high-speed direct-attach cable used for these links. Every row remains in both pre-deployment and post-deployment diagrams.

| From | To | Cable |
| --- | --- | --- |
| switch-1 port 2 | switch-2 port 1 | QSFP28 DAC |
| switch-1 port 1 | switch-3 port 1 | QSFP28 DAC |
| switch-1 port 3 | jcrsd-1 port 7 | QSFP28 DAC |
| switch-1 port 4 | jcrsd-1 port 8 | QSFP28 DAC |
| switch-2 port 3 | jcrsd-2 port 7 | QSFP28 DAC |
| switch-2 port 4 | jcrsd-2 port 8 | QSFP28 DAC |
| switch-3 port 3 | jcrsd-3 port 7 | QSFP28 DAC |
| switch-3 port 4 | jcrsd-3 port 8 | QSFP28 DAC |

The Wiring Guide specifies eight one-meter cables. The physical diagram forms a switch-1-centered arrangement, not a complete triangle among all switches. Two cables per server can serve different network roles; they are not proof that either cable is interchangeable with the other. Appendix C distinguishes the port-3 trunk and port-4 Kubernetes backend access role.

## Persistent hardware-management links

**MGMT** is the hardware management port leading to the controller, separate from the operating system's infrastructure interface. All rows use CAT6; pluggable switch ports listed here need the specified RJ45 transceiver.

| Device side | Switch side | Source cable length |
| --- | --- | --- |
| jcrsd-1 MGMT | switch-1 port 8 | 3 ft |
| jcrsd-2 MGMT | switch-2 port 8 | 3 ft |
| jcrsd-3 MGMT | switch-3 port 8 | 3 ft |
| fw-site-1 MGMT | switch-1 port 7 | 10 ft |
| so-sensor-lg-1 MGMT | switch-4 port 8 | 10 ft |
| so-sensor-sm-1 MGMT | switch-4 port 5 | 3 ft |

Source: Wiring Guide PDF pp. 13–14 and 26–28. IPMI management is a separate function from ingesting packets or reaching an application's login page.

## Persistent infrastructure links

| From | To | Media |
| --- | --- | --- |
| switch-1 port 6 | jcrsd-1 port 2 | CAT6 with switch-side RJ45 transceiver |
| switch-2 port 6 | jcrsd-2 port 2 | CAT6 with switch-side RJ45 transceiver |
| switch-3 port 6 | jcrsd-3 port 2 | CAT6 with switch-side RJ45 transceiver |
| fw-site-1 port 7 | switch-1 port 11 | SFP+ DAC |
| switch-1 port 12 | switch-4 port 15 | SFP+ DAC |
| switch-4 port 4 | so-sensor-lg-1 port 2 | CAT6 |
| switch-4 port 3 | so-sensor-sm-1 port 6 | CAT6 |
| switch-4 port 1 | deployment laptop Ethernet | CAT6 |

The deployment laptop connection is shown as a **trunk** in the Appendix C baseline: it carries selected VLAN-tagged networks for the provisioner. Do not assume it is configured like an ordinary analyst access port. Source: Wiring Guide PDF pp. 14–16 and 28–30; Appendix C PDF pp. 23–24.

## Temporary provisioning links

These support installation and automation; remove them only at the prescribed transition after successful deployment validation.

| From | To in Wiring Guide | Note |
| --- | --- | --- |
| switch-1 port 10 | jcrsd-1 port 1 | CAT6; RJ45 transceiver at switch |
| switch-2 port 10 | jcrsd-2 port 1 | Same |
| switch-3 port 10 | jcrsd-3 port 1 | Same |
| switch-1 port 9 | fw-site-1 port 5 | CAT6; RJ45 transceiver at switch |
| switch-4 port 12 | so-sensor-lg-1 port 1 | CAT6; RJ45 transceiver at switch |
| switch-4 port 10 | so-sensor-sm-1 port 5 | **Unresolved**: WIRE p. 11 says sensor port 5; V2 p. 47 instead says port 7, which its next page also assigns to capture. WIRE remote sites use port 1, while V2 remote tables pp. 50–51 use port 5 |
| Hunt 1 switch-2 port 12 | Hunt 2 switch-1 port 16 | Temporary SFP+ DAC intersite uplink |
| Hunt 1 switch-3 port 12 | Hunt 3 switch-1 port 16 | Temporary SFP+ DAC intersite uplink |

Do not guess the disputed small-sensor port from cable color. Check its physical/operating-system interface mapping, current inventory, and approved wiring revision. Source: Wiring Guide PDF pp. 10–11, 17, 19.

## WAN and TAP ingest

During staging, firewall port 8 connects via CAT6 and a compatible RJ45 transceiver to temporary WAN switch port 2. During operations, that firewall WAN port connects to the assigned partner transport port. For the independent port-6 MISSION connection see [[JCHK Mission Partner Connectivity]].

The guide's example uses one ASF01 TAP per sensor. A TAP has **network ports** in the observed link and **tool ports** carrying copied traffic to the sensor. Tool-output mappings are:

| TAP output | Sensor monitor input |
| --- | --- |
| Large sensor's TAP T1A | so-sensor-lg-1 port 3 |
| Large sensor's TAP T1B | so-sensor-lg-1 port 4 |
| Small sensor's TAP T1A | so-sensor-sm-1 port 8 |
| Small sensor's TAP T1B | so-sensor-sm-1 port 7 |

The small-sensor direction assignment also differs across packages: WIRE pp. 31/33/36 and SG pp. 30/33/36 map T1A to port 8 and T1B to port 7, while V2 pp. 81/83/84 reverses those two inputs. The table above explicitly follows WIRE. Verify and label both directions against the approved configuration.

The guide's worked example uses CAT6 and RJ45 transceivers; the PNG legend deliberately says TAP media varies. The network-side link and tool-side interfaces must have compatible speed/media. Observe both traffic directions. Do not connect a production traffic path to an arbitrary management or tool-output port. See [[JCHK TAPs and Traffic Visibility]] for placement and directionality.

## Verification and sources

Trace one complete example: TAP tool output → small sensor monitor interface for capture; small sensor port 6 → switch-4 → switch-1 → firewall/service networks for management and metadata; small sensor MGMT → switch-4 for independent hardware management. Three cables can reach one sensor for three different reasons.

[[JCHK Sources#WIRE|Wiring Guide]], PDF pp. 5, 7, 9–16, 25–31; [[JCHK Sources#APP|Appendices]], PDF pp. 22–24; [[JCHK Sources#PRE1|Hunt 1 pre-deployment diagram]]; [[JCHK Sources#POST1|Hunt 1 post-deployment diagram]]. Related: [[JCHK Source Discrepancies]], [[JCHK Remote and Analyst Site Cabling]].
