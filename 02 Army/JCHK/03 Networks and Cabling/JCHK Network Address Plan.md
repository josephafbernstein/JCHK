---
tags: [jchk, guide]
source_version: "0.6.1"
---
# JCHK Network Address Plan

Up: [[JCHK Start Here]] · Prerequisite: [[JCHK Network Fundamentals]]

## How to read this reference

The network plan is a map of functions and documented example addresses. It is not a discovery of your currently running kit. The supplied v0.6.1 sources disagree, including within Appendix C itself. The main subnet table below explicitly follows **Lesson 4**. The host table follows **Appendix C's host tables**. Resolve discrepancies using the actual deployment inventory and approved configuration before changing a device.

**INFRA** means infrastructure management; **IPMI** means hardware management; **TOOLS** hosts virtual tool services; **K8S** is the Kubernetes backend; **DMZ** is the controlled service network facing mission endpoints; **PROV** is temporary provisioning. See [[JCHK Source Discrepancies]] for the full comparison.

## Address hierarchy

| Block | Source-defined purpose |
| --- | --- |
| `10.1.0.0/16` | Routable kit resources |
| `10.1.0.0/20` | Analyst Site allocation |
| `10.1.16.0/20` | Hunt Site 1 allocation |
| `10.1.32.0/20` | Hunt Site 2 allocation |
| `10.1.48.0/20` | Hunt Site 3 allocation |
| `10.2.0.0/16` | Kubernetes private service overlay |
| `10.3.0.0/16` | Kubernetes private pod overlay |

An **overlay** is a virtual network implemented on top of underlying network connections. A Kubernetes **Service** supplies a stable way to reach an application; a **Pod** is Kubernetes' basic unit of running containers. These two overlay blocks are not additional physical sites or ordinary analyst subnets. Source: Appendix C, PDF p. 16.

## Subnets as shown in Lesson 4

Every subnet in this table is `/24`. A DHCP range shows only the last octet. “Not listed” means Lesson 4 shows N/A, not proof that DHCP is disabled in your deployment.

| Site | VLAN | Role | Subnet | Gateway | DHCP, per Lesson 4 |
| --- | ---: | --- | --- | --- | --- |
| Analyst | 5 | ANALYST | 10.1.0.0/24 | 10.1.0.1 | 10–240 |
| Analyst | 10 | INFRA | 10.1.1.0/24 | 10.1.1.1 | 150–240 |
| Hunt 1 | 5 | ANALYST | 10.1.16.0/24 | 10.1.16.1 | 10–240 |
| Hunt 1 | 10 | INFRA | 10.1.17.0/24 | 10.1.17.1 | Not listed |
| Hunt 1 | 20 | IPMI | 10.1.18.0/24 | 10.1.18.1 | 150–240 |
| Hunt 1 | 30 | TOOLS | 10.1.19.0/24 | 10.1.19.1 | Not listed |
| Hunt 1 | 40 | K8S | 10.1.20.0/24 | Not listed | Not listed |
| Hunt 1 | 50 | DMZ | 10.1.21.0/24 | 10.1.21.1 | Not listed |
| Hunt 1 | 150 | PROV | 10.1.31.0/24 | 10.1.31.1 | 150–240 |
| Hunt 2 | 10 | INFRA | 10.1.33.0/24 | 10.1.33.1 | Not listed |
| Hunt 2 | 20 | IPMI | 10.1.34.0/24 | 10.1.34.1 | 150–240 |
| Hunt 3 | 10 | INFRA | 10.1.49.0/24 | 10.1.49.1 | Not listed |
| Hunt 3 | 20 | IPMI | 10.1.50.0/24 | 10.1.50.1 | 150–240 |

Lesson 4 PDF p. 10 says an address written `10.1.48.1/24` is intentionally skipped. A `/24` network identifier would be `10.1.48.0/24`; retain the concept of a reserved site offset without copying the malformed notation as a network address.

## Default host examples from Appendix C

| Role | Documented address or pattern | Notes |
| --- | --- | --- |
| Analyst firewall INFRA | 10.1.1.1 | Analyst-side management |
| Analyst switch | 10.1.1.241 | In-band switch management |
| Hunt 1 firewall INFRA | 10.1.17.1 | Hub firewall |
| Provisioner | 10.1.17.4, 10.1.18.4, 10.1.31.4 | Separate INFRA, IPMI, and PROV interfaces |
| FreeIPA replica | 10.1.17.5 | Identity/DNS replica VM |
| JCRS-D nodes 1–3 INFRA | 10.1.17.11–13 | `jcrsd-1` through `jcrsd-3` |
| JCRS-D nodes 1–3 K8S | 10.1.20.11–13 | Private backend interfaces |
| Security Onion manager | 10.1.19.20 | `so-manager.kit1.jchk` |
| Security Onion receivers 1–3 | 10.1.19.21–23 | Receive logs/metadata |
| Security Onion search 1–6 | 10.1.19.31–36 | Searchable indexed data |
| Security Onion Fleet | 10.1.21.30 | DMZ endpoint-facing service |
| Velociraptor | 10.1.21.70 | DMZ endpoint-facing service |
| Hunt 1 large / small sensor | 10.1.17.41 / 10.1.17.51 | Operating-system management |
| Hunt 2 large / small sensor | 10.1.33.41 / 10.1.33.51 | Operating-system management |
| Hunt 3 large / small sensor | 10.1.49.41 / 10.1.49.51 | Operating-system management |
| Hunt 1 switches 1–4 | 10.1.17.241–244 | Site-local switch numbering |
| Hunt 2 / Hunt 3 switch | 10.1.33.241 / 10.1.49.241 | Each is called `switch-1` at its own site |

These are baseline examples, not a complete discovery inventory. IPMI controller addresses may be leased; do not substitute the operating-system address for the controller. Firewall hostname spelling varies across sources (`fw-site1` versus `fw-site-1`); use the deployed DNS name.

## Conflicting transport examples

| Item | Lesson 4 | Appendix C | Volume 3 |
| --- | --- | --- | --- |
| Temporary WAN | 10.1.250.0/24, p. 19 | 10.1.254.0/28, p. 18 | 10.1.250.0/28, p. 29 |
| Tunnel address family | Three 10.1.254.x/30 networks, pp. 25, 28 | 10.1.255.0/30, .4/30, .8/30, p. 18 | Inspect deployed firewall configuration |

Do not combine a WAN mask from one column with tunnel endpoints from another and assume the result is the kit baseline. Similarly, Appendix C network tables put Hunt 2 INFRA/IPMI at 10.1.32/33 and Hunt 3 at 10.1.48/49, while its host tables align with Lesson 4's 33/34 and 49/50 layout. Appendix C also lists DHCP for infrastructure networks where Lesson 4 says N/A.

## Practical use

Record the actual site's subnet, gateway, DNS, DHCP scope, and management addresses in a mission-specific configuration record. Compare this map with the automation inventory and firewall interface pages. Validate that the chosen kit blocks do not overlap partner networks or other deployed kits. Do not change live addressing simply to make it resemble a training table.

## Sources and related notes

[[JCHK Sources#L04|Lesson 4]], PDF pp. 6–10, 15, 19, 25–28; [[JCHK Sources#APP|Appendices]], PDF pp. 16–24; [[JCHK Sources#V3|Volume 3]], PDF p. 29. Related: [[JCHK Firewalls and WireGuard]], [[JCHK Mission Partner Connectivity]], [[JCHK Service Portal and Identity]].
