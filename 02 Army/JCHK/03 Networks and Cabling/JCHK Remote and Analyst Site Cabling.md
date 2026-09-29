---
tags: [jchk, guide]
source_version: "0.6.1"
---
# JCHK Remote and Analyst Site Cabling

Up: [[JCHK Start Here]] · Prerequisite: [[JCHK Cabling and Deployment States]]

## Hunt Sites 2 and 3 use the same pattern

Each secondary Hunt Site has an FW 3100 firewall, one QSW-M3216R-8S8T switch, one SN 9000 large sensor, and one SN 3100 small sensor. The standard drawings show two ASF01 TAPs, one per sensor. The central SN 7100 cluster remains at Hunt Site 1; there is no SN 7100 drawn at either remote site.

In the tables below, `N` means either site 2 or site 3. Names such as `so-sensor-lg-N` are explanatory placeholders; replace N with the site's number when identifying a device. Each site's switch is locally called `switch-1`.

## Persistent remote-site connections

| Purpose | From | To | Media |
| --- | --- | --- | --- |
| Firewall hardware management | fw-site-N MGMT | switch-1 port 7 | CAT6 |
| Large sensor hardware management | so-sensor-lg-N MGMT | switch-1 port 8 | CAT6 |
| Small sensor hardware management | so-sensor-sm-N MGMT | switch-1 port 5 | CAT6 |
| Internal firewall trunk | fw-site-N port 7 | switch-1 port 15 | SFP+ DAC |
| Large sensor infrastructure | so-sensor-lg-N port 2 | switch-1 port 4 | CAT6 |
| Small sensor infrastructure | so-sensor-sm-N port 6 | **port 3 in pre-deployment text; port 1 in post-deployment text** | CAT6; unresolved source difference |

The small-sensor difference occurs in the Wiring Guide itself: PDF pp. 18/21 specify switch port 3, whereas pp. 32/35 specify switch port 1. Both ports belong to the infrastructure access range in Appendix C, but that does not prove a deliberate cable move was intended. The supplied PNGs keep the small-sensor connection in the same visual switch area. Label the cable and verify the approved plan instead of silently deciding one source is correct.

## Temporary remote provisioning connections

| From | To | Media |
| --- | --- | --- |
| fw-site-N port 5 | switch-1 port 11 | CAT6 with switch-side RJ45 transceiver |
| so-sensor-lg-N port 1 | switch-1 port 12 | CAT6 with switch-side RJ45 transceiver |
| so-sensor-sm-N port 1 in WIRE; port 5 in V2 | switch-1 port 10 | CAT6 with switch-side RJ45 transceiver; source conflict |
| Hunt 2 switch-1 port 16 | Hunt 1 switch-2 port 12 | SFP+ DAC |
| Hunt 3 switch-1 port 16 | Hunt 1 switch-3 port 12 | SFP+ DAC |

The small-sensor provisioning endpoint follows WIRE pp. 17/19 in the first alternative, but V2 pp. 50–51 says port 5. Confirm the intended sensor interface rather than treating either as a universally applicable value.

A **provisioning interface** receives installation/configuration traffic. These extra links disappear from the operational topology. In contrast, the firewall's port-7 internal trunk and all MGMT links persist.

## Remote WAN and capture links

In the Wiring Guide p. 7 staging example, Hunt 2 firewall port 8 connects to temporary WAN switch port 4; Hunt 3 port 8 connects to temporary WAN switch port 5. V2 pp. 50–51 instead uses temporary-switch ports 6 for Hunt 2 and 4 for Hunt 3. A staging switch may allow equivalent access ports, but equivalence is not established by these documents. In operations, each port 8 connects to its assigned mission partner transport. The firewall carries the site's WireGuard path to Hunt 1.

The capture-output pattern is the same at each site:

| Sensor | TAP tool output | Monitor input |
| --- | --- | --- |
| Large SN 9000 | T1A | Port 3 |
| Large SN 9000 | T1B | Port 4 |
| Small SN 3100 | T1A | Port 8 |
| Small SN 3100 | T1B | Port 7 |

The small-sensor capture table above follows WIRE pp. 33/36. V2 pp. 83/84 reverses those two sensor inputs, mapping T1A to 7 and T1B to 8; confirm direction labels in the approved plan.

The Wiring Guide describes a CAT6/transceiver example. Actual TAP media and monitored source depend on the mission; SPAN may be an alternative. **Monitor** here means receiving copied network data, not the server's hardware-management port. Correct directionality and loss measurements matter; see [[JCHK Sensor Health and Packet Loss]].

## Analyst Site connections

The Analyst Site is the user-access location in these diagrams. It contains laptops 2–9, a QSW-M3216R-8S8T switch, and a Netgate 6100 MAX firewall. Laptop 1 is the deployment laptop shown at Hunt Site 1. Eight analyst laptops plus that deployment laptop account for the nine kit laptops in this layout.

| From | To | Purpose |
| --- | --- | --- |
| Laptop 2 through laptop 9 Ethernet | Corresponding switch ports 2 through 9 | Analyst access over CAT6 |
| Netgate LAN1 | Analyst switch port 16 | Internal LAN connection over CAT6 |
| Netgate WAN3, staging | Temporary WAN switch port 1 | Staging intersite transport |
| Netgate WAN3, operational | Assigned mission partner network port | Operational intersite transport |

The guide specifies an RJ45 transceiver at switch port 9 and switch port 16, plus one in Netgate WAN3 for the shown copper WAN connection. LAN1 itself is an RJ45 port. The Analyst switch's firewall uplink carries VLAN 5 (analyst) and VLAN 10 (infrastructure). A laptop access connection and the firewall trunk are not interchangeable configurations.

The pre- and post-deployment Analyst PNGs keep the laptop-to-switch and switch-to-firewall connections unchanged. The principal visual difference is the WAN destination: temporary switch versus MPN cloud.

## A physical walk-through

For an analyst at laptop 7, follow the cable to Analyst switch port 7, then the configured VLAN path to Netgate LAN1, then the firewall's WAN tunnel to Hunt 1 when the destination is a central service. The application server does not move to the Analyst Site merely because its web page appears there.

For a remote sensor, trace the TAP monitor input separately from its infrastructure and MGMT ports. If the analyst can log in to its hardware controller but sees no captured traffic, the management cable may be correct while the ingest source, monitor selection, or capture pipeline still needs attention.

## Sources and related notes

[[JCHK Sources#WIRE|Wiring Guide]], PDF pp. 5, 7–8, 16–24, 31–36; [[JCHK Sources#APP|Appendices]], PDF pp. 22–24; [[JCHK Sources#PRE23|Hunt 2/3 pre-deployment diagram]], [[JCHK Sources#POST23|Hunt 2/3 post-deployment diagram]], [[JCHK Sources#PREA|Analyst pre-deployment diagram]], [[JCHK Sources#POSTA|Analyst post-deployment diagram]]. Related: [[JCHK Source Discrepancies]], [[JCHK Firewalls and WireGuard]], [[JCHK Hunt Site 1 Cabling]].
