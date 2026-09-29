---
tags: [jchk, sensors, troubleshooting]
baseline: v0.6.1 training snapshot
---
# JCHK Sensor Health and Packet Loss

[[JCHK Start Here]] · [[JCHK Security Onion Architecture]]

A sensor is useful only if it can receive traffic, process it, retain the necessary evidence, and make records available to analysts. These are different success conditions. A green link light establishes a physical connection; it does not establish complete capture or successful central indexing.

## Read the Grid

The Security Onion Console's **Grid** summarizes node health. Lesson 9 identifies these fields:

| Field | Meaning and investigation value |
|---|---|
| ID / Address | Which host is being examined and its management address |
| Connection Status | Whether the node is connected to the grid |
| Last Heard From | When the node last checked in; distinguishes current data from stale status |
| OS Uptime | Time since operating-system boot; unexpected resets may explain evidence gaps |
| Earliest PCAP | Oldest packet capture presently available on that sensor |
| PCAP Retention | How much capture history is retained |
| I/O Wait | Time the system spends waiting on input/output, often relevant to storage pressure |
| Capture / Zeek / Suricata loss | Loss indicators at different stages of collection and analysis |

The lesson also names Stenographer loss as a displayed field, while its baseline descriptions assign full capture to Suricata. Treat UI field lists as version-dependent; do not infer that every named capture engine is deployed.

**InfluxDB** is the time-series platform described for monitoring. A **time series** is a sequence of measurements associated with times. Its dashboards help show when load, drops, or resource pressure changed. It is an operational-health tool, not a substitute for packet evidence or endpoint records.

## Use read-only checks before changing configuration

From the appropriate Security Onion host, authorized administrators can use the lesson's commands:

```bash
sudo so-status
sudo so-zeek-stats
sudo so-suricata-stats
```

`sudo` runs a command with elevated privileges. `so-status` checks service/container health; the other two report engine statistics including loss/performance. Record which host and time each result represents. A sensor's statistics and a Manager's service status answer different questions.

Two other lesson commands are **configuration changes**, not harmless status checks: `so-allow` changes host-firewall access, and `so-monitor-add <interface>` adds a monitoring interface. Use them only when the diagnosed problem and approved configuration require them; the text `<interface>` is a placeholder for the verified operating-system interface name.

## Separate three common failure patterns

**No packets on the sensor:** trace the observation path. Was traffic generated? Was the right link tapped or mirrored? Are both directions connected? Are transceivers and negotiated speeds compatible? Is the cable on a monitoring interface rather than management? Check link state and receive counters before changing application rules.

**Packets arrive but analysis is incomplete:** compare engine health and loss statistics, processor/memory pressure, and storage behavior. A capture link's nominal speed is not a guarantee that the sensor can analyze every traffic mix at that rate. Many small packets can create different processing pressure than fewer large packets at the same bit rate.

**Local evidence exists but central searches are empty:** check the sensor role, time filter, network/VPN connectivity, queueing/ingestion, and Search-node health. A Heavy node stores searchable data locally; a Forward node forwards metadata. A central search failure does not prove local capture failed.

## Understand throughput and retention

**Throughput** is the measured amount of data processed per unit time. **Gbps** means billions of bits per second; **GB** describes billions of bytes, with eight bits per byte. As a simplified calculation, a sustained 1 Gbps is 125 MB/s before accounting for capture format and other overheads. This is a teaching conversion, not a sensor performance or retention specification.

Retention depends on actual captured volume, allocated capacity, indexing/storage overheads, and rollover settings. A system can continue collecting while overwriting its oldest packets. Export needed evidence before expiry and record the applicable retention boundary.

When troubleshooting under load, collect a baseline, change one controlled variable, observe again, and record results. The training provides no universal acceptable loss threshold. Assess loss against the mission's evidence needs and validated platform expectations; do not invent a “passing” percentage.

## Related notes

[[JCHK TAPs and Traffic Visibility]] · [[JCHK Security Onion Investigation Workflow]] · [[JCHK Mission Workflow]] · [[JCHK Capstone Practice]]

## Sources

- [[JCHK Sources#L09|Lesson 9]], PDF pp.17–22: Grid fields, InfluxDB, commands.
- [[JCHK Sources#L10|Lesson 10]], PDF pp.4–6: health under load and packet-drop assessment.
- [[JCHK Sources#V3|Volume 3]], PDF pp.79–83: symptom/cause/validation/action model and operational troubleshooting.
