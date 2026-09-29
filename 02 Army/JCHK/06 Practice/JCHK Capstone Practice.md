---
tags: [jchk, practice, capstone]
baseline: v0.6.1 training snapshot
---
# JCHK Capstone Practice

[[JCHK Start Here]] · [[JCHK Mission Workflow]]

This practice guide turns the supplied capstone objectives into questions, observable checks, and explanations. All activity belongs in the authorized training environment. The answers are reasoning checks, not fabricated measurements from a running kit.

## The supplied scenario

Lesson 10 describes three Hunt Sites, each with a Cisco TRex traffic generator simulating target-network activity. Site 1 may reach **10 Gbps**, Site 2 should remain below **3 Gbps**, and Site 3's volume is unknown. **Traffic generation** creates controlled network activity for testing. These are scenario conditions, not guarantees that a particular sensor will process every workload losslessly.

The team must reach the core tools, validate readiness, connect the TAPs, confirm ingest, identify traffic, pivot to supporting packets, and monitor system health under changing load.

## Exercise A — Explain the system before connecting it

Write one sentence for each component: TAP, sensor, Heavy node, Forward node, Manager, Search, Fleet, Velociraptor, and analyst laptop. Draw two paths: copied network traffic to an analyst finding, and endpoint telemetry to an analyst finding.

**Answer check:** the TAP copies traffic; the sensor captures/processes it; packet files remain local in the documented design; Forward-node metadata follows the central pipeline; Heavy-node metadata is indexed locally and queried across clusters. Fleet handles Elastic Agents; Velociraptor is a separate endpoint-forensics service. The analyst laptop accesses and examines evidence rather than replacing these roles.

## Exercise B — Prove ingest at each site

Record the site, sensor hostname/model, monitoring interface, tapped link, direction mapping, and observation interval. Identify a known generated conversation and find its corresponding records. Retrieve supporting PCAP if available.

**Answer check:** successful proof connects the known generated activity to the correct sensor and time. A link light, ping, or open SOC page alone is insufficient. If only requests or responses appear, check directional TAP feeds and coverage. Do not invent traffic classifications or “actionable alerts” without inspecting the actual results.

## Exercise C — Compare health under load

Capture baseline Grid and engine statistics. Increase traffic only within the exercise plan and collect comparable observations from each site. Record measured throughput, interval, packet/drop counters, percentages where actually provided, CPU/storage pressure, and central-search freshness.

**Answer check:** compare like-for-like intervals and distinguish physical arrival, packet capture, protocol analysis, and central indexing. A higher link speed is not proof of higher usable analysis capacity. Site 3 requires measurement; its “unknown” traffic volume is not permission to assume zero or select an arbitrary value. The slides provide no universal passing drop threshold.

## Exercise D — Diagnose a gap

Scenario 1: the SOC opens but no traffic appears. Scenario 2: local packets exist but central searches are stale. Scenario 3: one sensor reports loss under load.

**Answer check:** first establish scope/time and source traffic, then inspect the appropriate stage. For Scenario 1, check observation feed, interface, engine health, and query filters. For Scenario 2, consider sensor role, connectivity, queues, and Search health. For Scenario 3, examine receive/engine loss and resources before making changes. Record a hypothesis and a confirming check before restarting or reconfiguring components.

## Exercise E — Endpoint agents

The Student Guide's lab uses Windows/Linux training VMs and a simulated partner network. Verify the reachable kit service, correct name/certificate, package/configuration, administrative privileges, and installed service. In Fleet, confirm a healthy agent and an expected benign event. In Velociraptor, confirm the correct client checks in and a small approved collection returns the intended data.

**Source correction:** Student Guide p.48 describes `192.168.1.0/24` and `192.168.1.104`; p.49 uses `192.168.10.104` in install examples. Paths also contain spacing errors and the Velociraptor executable is misspelled. Its Elastic examples disable certificate verification. Treat these as imperfect training examples, not ready-to-run mission commands. Reconcile the active endpoint, trust configuration, binary names, and installer version rather than copying them or bypassing certificate checks.

## Exercise F — The 100 Mbps SPAN question

The worksheet asks which sensor/interface can accept a 100 Mbps mirror feed and asks the student to add it as a monitoring interface.

**Answer check:** select an available physical interface whose supported link modes include 100 Mbps, verify the real adapter and operating-system name, and then use the approved monitoring-interface procedure. The SN 3100's unused copper ports are a candidate to investigate; the worksheet alone does not prove their negotiated modes. The ASF01's documented 1/10GbE ports should not be assumed to accept a 100 Mbps link. Validate with actual capabilities instead of equating “faster port” with universal backward compatibility.

## Evidence worksheet

| Item | Record |
|---|---|
| Site, sensor, role, interface | Actual observed configuration |
| Time interval and zone | Beginning/end of test |
| Known test activity | Source, destination, expected protocol |
| Measured ingest and loss | Values plus measurement source |
| Supporting evidence | Queries, events, PCAP/export references |
| Health and gaps | Missing sources, stale data, errors |
| Conclusion | What the test proves and does not prove |

## Related notes

[[JCHK TAPs and Traffic Visibility]] · [[JCHK Sensor Health and Packet Loss]] · [[JCHK Elastic Agent and Fleet]] · [[JCHK Velociraptor Endpoint Forensics]]

## Sources

- [[JCHK Sources#L10|Lesson 10]], PDF pp.2–9: scenario, tasks, tapping diagrams.
- [[JCHK Sources#SG|Student Guide]], PDF pp.48–51: agent lab and capstone worksheet.
- [[JCHK Sources#L09|Lesson 9]], PDF pp.6–13, 17–21, 25–37: interfaces, diagnostics, and agents.
- [[JCHK Sources#V3|Volume 3]], PDF pp.48–54: agent configuration and verification. Exercise answer checks are explanatory synthesis.
