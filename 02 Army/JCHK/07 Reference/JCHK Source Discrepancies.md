---
tags: [jchk, guide]
source_version: "0.6.1"
---
# JCHK Source Discrepancies

Up: [[JCHK Start Here]] · Related: [[JCHK Sources]]

## Why this note exists

The supplied documents are provisional and sometimes disagree with one another or with their own tables. This record separates **what the source says** from **what can be concluded**. It is a reading aid, not an errata release from the program and not a substitute for the actual kit's approved configuration. A source repeated in several places can be better supported without proving what is installed on a particular kit.

For a deployment, first identify the actual baseline and inventory; compare the device's physical labels and current configuration with the applicable approved revision; record the resolution and the evidence. Do not merge conflicting values into a new configuration by guesswork. PDF page numbers below use the convention in [[JCHK Sources]].

## Network addressing and names

| Issue | Conflicting evidence | How the notes handle it |
| --- | --- | --- |
| Hunt 2 infrastructure/IPMI | APP PDF p. 17 subnet table says 10.1.32/33; APP p. 21 host table and L04 p. 9 say 10.1.33/34 | Main subnet map explicitly follows Lesson 4; both versions remain documented |
| Hunt 3 infrastructure/IPMI | APP pp. 17–18 subnet table says 10.1.48/49; APP pp. 21–22 host table and L04 p. 10 say 10.1.49/50 | Same approach; don't confuse source choice with live discovery |
| Infrastructure DHCP | APP pp. 17–18 gives 150–240; L04 pp. 8–10 marks Hunt infrastructure DHCP N/A | Label each source; inspect actual scopes |
| Temporary WAN | L04 p. 19: 10.1.250.0/24; V3 p. 29: 10.1.250.0/28; APP p. 18: 10.1.254.0/28 | No single unqualified WAN default asserted |
| WireGuard tunnel space | L04 pp. 25/28 uses 10.1.254.x/30 and next-hop example 10.1.254.5; APP p. 18 uses 10.1.255.0/30, .4/30, .8/30 | Do not combine peer addresses from different plans |
| Firewall DNS spelling | L04 p. 15 uses fw-site-1 style; APP pp. 18–21 uses fw-site1 style | Treat these as source spellings; actual DNS/inventory is authoritative for access |
| FreeIPA access address | V3 p. 43 gives 10.1.16.4; L08 p. 14 gives 10.1.17.4; APP p. 19 lists provisioner10.1.17.4 and replica10.1.17.5 | Verify service link and DNS through the deployment, not an inferred replacement address |
| “Skipped network” wording | L04 p. 10 writes 10.1.48.1/24 as a network | Explain that the aligned /24 network address would end in .0; retain the reservation idea |

## Cabling and device labels

| Issue | Conflicting evidence | How to interpret it |
| --- | --- | --- |
| Remote small sensor infrastructure | WIRE pre-deployment pp. 18/21 says switch port3; post-deployment pp. 32/35 says port1 | Both are in APP infrastructure access range; intended move remains unconfirmed |
| Hunt 1 small sensor provisioning | WIRE p. 11 says sensor port5; V2 p. 47 says port7 and p. 48 also assigns that port to capture. WIRE remote pp. 17/19 says port1; V2 pp. 50–51 says port5 | Verify the device role mapping before connection; don't silently apply site2's value to site1 |
| WAN versus MISSION | PNGs show WAN-to-MPN; L04 p. 30 and V3 p. 29 separately require Hunt1 port6/ix1 for MISSION services | These are different purposes, not contradictory definitions of one port |
| Post-deployment step headings | WIRE p. 23 headings still say connect to spare QNAP, but detailed steps say provided MPN port | Notes describe the detailed operational destination and preserve the heading error here |
| Analyst port ranges | APP p. 22 lists RJ45 1–8 and SFP+8–15, overlapping8; V1 p. 39 hardware ports are RJ45 1–8 and SFP+9–16 | Use actual physical layout; don't interpret port8 as two different cages |
| QSW-M3216 port labeling | L02 p. 30 labels QSFP28/SFP28; V1 p. 39 describes RJ45 and SFP+ for this model | Model-specific hardware evidence governs explanation; avoid importing 7308 port types |
| Small-sensor port8 | V2 p. 48 contains management/capture-role ambiguity | Cabling notes distinguish MGMT, infrastructure port6, and monitor ports7/8; verify approved mapping |
| SN9100 reference | SG p. 22 lists SN9100 in lab prerequisites; surrounding kit sources use SN9000 | Treat as an apparent naming error, not another kit model |
| Analyst laptop placement | V2 p. 77 brings laptops to Analyst Site for final configuration; PNGs show laptop1 as Hunt1 deployment laptop and2–9 as analysts | These can describe different phases; notes state the diagram's operating layout |
| Small-sensor TAP direction | WIRE pp. 31/33/36 and SG pp. 30/33/36 map T1A→8, T1B→7; V2 pp. 81/83/84 maps T1A→7, T1B→8 | Notes label WIRE values; verify the intended direction mapping |
| Temporary WAN switch ports | WIRE p. 7 uses Hunt2→4, Hunt3→5; V2 pp. 50–51 uses Hunt2→6, Hunt3→4 | Verify staging-switch settings and cable labels; no undocumented equivalence assumed |

## Architecture and terminology

| Issue | Evidence | Treatment |
| --- | --- | --- |
| Where central analytics run | APP glossary p. 51 calls Analyst Site the hub hosting analytics; V1 pp. 21/28 and supplied diagrams place SN7100s at Hunt1 | Distinguish analyst user access from physical server hosting at Hunt1 |
| BMC VLAN number | APP glossary p. 51 says VLAN101; APP pp. 17/23–24 specifies IPMI VLAN20 | Teach role and explicit network-table baseline; flag glossary number |
| Kubernetes PVC scope | L06 p. 21 says PersistentVolumeClaim is not namespaced | Correct using Kubernetes documentation: PV is cluster-scoped; PVC is namespaced; see [[JCHK JCRS-D and Kubernetes]] |
| PCAP expansion | APP acronym table p. 49 says “Packet Capture Protocol” | Explain PCAP as packet capture and capture-file formats, not a distinct network protocol |
| KVM ambiguity | APP p. 49 expands Keyboard, Video, Mouse; virtualization uses the same acronym for Kernel-based Virtual Machine | Glossary defines both and uses context |
| Sensor forward/heavy roles | Some sources describe forward or heavy nodes; deployed role determines local processing and storage | Notes teach the role distinction without assuming every sensor is a heavy/search node |

## Hardware and electrical planning

| Issue | Evidence | Treatment |
| --- | --- | --- |
| Total circuit recommendation | V1 p. 13 summary3×20A or4×15A; same page warning4×20A or6×15A for9540W/79.5A | Do not offer one quoted circuit count as validated engineering; follow qualified site power planning |
| Analyst peak versus circuit | V1 p. 14 gives2629W/about22A but one15A circuit | Explicit inconsistency; do not load a circuit from that recommendation alone |
| FW3100 RAM | V1 p. 36 says32GB; L02 p. 25 says64GB | Flag source-dependent specification; verify installed unit |
| MS100 storage | V1 p. 36 says NVMe; L02 p. 23 says SATA | Do not combine into a single verified specification |
| Analyst growth count | V1 p. 26 supports15 laptops; L01 p. 10 says14 | Default supplied layout is nine total kit laptops, eight shown at Analyst Site; expansion limit unresolved |
| Fiber designation | V1 p. 47 identifies OS2, while L02 p. 36 / V1 p. 46 references OS1 | Verify actual fiber and approved optics; generic single-mode concept remains clear |
| Power extension length | V1 p. 51 says6ft; APP p. 8/case lists1ft | Inventory physically; don't substitute by assumed length |
| Warranty sanitization citation | L02 p. 45 cites NIST800-80 in destruction/warranty context | Don't treat that citation as validated sanitization guidance; use approved applicable process |

## Software, deployment, and lab examples

| Issue | Evidence | Treatment |
| --- | --- | --- |
| Mattermost availability | L03 pp. 32/35/36 says not in0.6.1; V3 pp. 54–57 documents operation and V2 includes deployment | Document purpose, mark release availability unconfirmed, check actual deployment |
| Playbook numbering | V2 p. 60 prose starts01/core01–03; table includes initialization00, core01–03, and optional tool stages04–05 | Use the numbered table as a reading sequence, while checking the actual deployer's release-specific dependencies |
| Number of switches | V2 p. 37 discusses three7308s but says repeat for all six | Distinguish three7308 models from six3216 models |
| Switch setup connection | V2 p. 38 mentions rear MGMT then port1 | Inspect correct model's management procedure and physical labels |
| Switch3 case location | V2 p. 38 assigns Hunt1 switch3 to Stack2 Case1 | Check inventory/diagram association with the third SN7100; flag apparent copy error |
| TAP count in procedure | V2 p. 92 says six total; p. 95 says repeat for three | Inventory six; do not assume a three-device instruction covers all kit TAPs |
| Firmware recovery completeness | V2 p. 106 promises SR-IOV/NVMe configuration; p. 107 only describes restoring defaults | Missing settings are not invented; obtain baseline-specific recovery instructions |
| Elastic external name example | V3 p. 51 varies so-fleet-1/so-fleet-01 and mixes internal IP into a NAT explanation | Require agreement among external DNS/address, certificate names, and actual agent settings |
| Capstone partner address | SG p. 48 scenario says192.168.1.104; p. 49 agent URL says192.168.10.104 | Example is not execution-ready; use the assigned lab/mission address |
| Capstone command typos | SG pp. 48–49 includes a spaced directory path and “velocirator” executable spelling | Teach install/verify workflow; do not provide a copy-paste reconstruction as verified |
| Example credentials and TLS bypass | SG pp. 48–49/52 has training passwords and insecure install flags | No passwords copied; lab shortcuts are not universal operating instructions |

## What counts as resolving an issue

A resolution should name the deployed release, device/site, verified value, evidence source, responsible owner, and date. Distinguish a documentation correction from a live configuration change. The fact that both disputed switch ports are in the same VLAN, for example, can explain why two variants might work but does not prove which cable labeling standard the program intended.

## Additional release and reference caveats

- **Software availability markings:** Appendix B's asterisk identifies open-source licensing, whereas Lesson 3 uses an asterisk for tools not integrated into the current release. Read each legend rather than carrying the meaning across documents. See [[JCHK Software Map and Baseline]].
- **Windows edition:** V3 PDF p. 24 and APP p. 11 refer to Professional; L03 p. 40 and L08 p. 44 identify Enterprise LTSC. Verify the installed image and license. See [[JCHK Analyst Workstations and Accessories]].
- **Virtualized tool availability:** L07 PDF p. 6 marks FreeIPA replica, NP-View, and Rsyslog absent from the current version, while V1 pp. 58/60 includes some in role descriptions. Architecture lists do not prove a deployed tool is present.
- **Security Onion manager address:** L09 PDF p. 33 uses 10.1.18.20; V3 p. 48 and APP p. 20 give 10.1.19.20. Use the verified deployment name/address, not a blended example.
- **Velociraptor paths:** V3 PDF p. 53 varies `/opt/container` and `/opt/containers`, executable spelling, and package versions. Check the actual release files; see [[JCHK Velociraptor Endpoint Forensics]].
- **Kali and vMotion:** V3 PDF p. 58 describes Kali using a VMware vMotion-network phrase. Kali is a Linux tool environment; this text does not establish vMotion as a required JCHK component.
- **TAP console baud:** V1 PDF p. 42 and V3 p. 37 say 11520; L02 p. 31, L08 p. 31, and V3 p. 69 say 115200. A baud rate is a serial-signaling rate. Verify the device's supported console settings before connection.
- **Legacy capture labels:** L09 PDF p. 17 names Stenographer loss in a user-interface example, while the documented main capture role uses Suricata. A dashboard label is not proof that every referenced engine runs in that baseline.
- **TAP management boilerplate:** V3 PDF p. 82 mentions GigaVUE-FM/licensing; do not infer that the simple ASF01 workflow requires the complete packet-broker management stack.
- **Drawer location:** V1 PDF p. 30 places the drawer in the second case; APP pp. 29–30, 35–36, 41–42 and L02 p. 10 place it in Case3. Use inventory and physical labels.
- **Inventory completeness:** Appendix A's summary omits some Netgate/MS100 equipment appearing in Appendix E and V1 p. 29. Distinguish a summary list from a full packing checklist.
- **Optics labels and reach:** V1 PDF p. 49 has 10GBASE-LX/SX wording beside 1000BASE/1GbE information; APP pp. 30/35/41 lists 550m for 10GBASE-SR versus 300m in V1 p. 49/L02 p. 39. Verify actual optic and fiber specifications instead of generalizing a mismatched label.
- **Storage headlines:** L01 PDF p. 8 gives approximate capacity/retention figures. These are not the same as usable RAID capacity or measured retention at a site's actual traffic rate; see [[JCHK Storage Capacity and Retention]].
