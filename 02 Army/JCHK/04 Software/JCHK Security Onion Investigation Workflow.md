---
tags: [jchk, security-onion, investigation]
baseline: v0.6.1 training snapshot
---
# JCHK Security Onion Investigation Workflow

[[JCHK Start Here]] · [[JCHK Security Onion Architecture]]

A useful investigation connects a question to observable evidence and then states a conclusion with its limits. Security Onion supplies alerts, protocol records, endpoint logs, packet captures, and extracted artifacts. The workflow below is a teaching synthesis of the training objectives; it is not a claim that the supplied slides contain a complete incident-response playbook.

## Establish what you can see

Before searching, record the site, sensor, monitored link, time interval, and time zone. Check that the sensor was operating during the interval and that the relevant data is still retained. A **retention window** is the period for which a system still holds data before overwriting or deleting it.

Separate the questions “Was there no matching activity?” and “Was the activity outside collection coverage?” A missing result could reflect an absent agent, wrong sensor, wrong clock, wrong query, expired PCAP, packet loss, or delayed indexing. The **Grid** and service-health checks in [[JCHK Sensor Health and Packet Loss]] help establish these limits.

## A repeatable sequence

1. **State a question.** Example: “Did the training workstation contact the approved test server during the exercise interval?” Avoid beginning with “Prove this host is compromised.”
2. **Select the right source.** Network connection/protocol metadata answers communication questions. Endpoint logs answer process and user questions. Packet contents can support protocol-level reconstruction when captured and readable.
3. **Constrain time and identity.** Use a narrow interval, known source/destination addresses, hostnames, or other identifiers. Record the search terms and the displayed time zone.
4. **Inspect the event.** Note sensor/site, timestamps, addresses, protocol, ports, alert/rule name, and available flow identifiers. A **port** is a numerical communication endpoint used by transport protocols; its number suggests but does not prove the application.
5. **Pivot to related records.** A **pivot** means using one event's identifiers to retrieve related evidence. Check neighboring connections, relevant DNS records, and endpoint activity rather than relying on a single alert.
6. **Retrieve supporting packets when useful.** Use SOC's packet/artifact access for the matching sensor and interval. Open exported PCAP in Wireshark for detailed inspection. Preserve the original export and document its origin.
7. **Test competing explanations.** An unusual connection could be a planned scan, software update, lab exercise, or unwanted activity. Correlate asset role and approved changes with technical evidence.
8. **Write the finding.** State observation, interpretation, confidence, impact, and remaining questions separately. Link the case to the evidence and recommend a next step supported by that evidence.

## What each evidence type can and cannot establish

| Evidence | Useful for | Important limit |
|---|---|---|
| Suricata alert | Finding traffic that matched a configured detection | A match alone does not establish successful exploitation |
| Zeek record | Understanding connection/protocol behavior | Usually summarizes activity rather than retaining every payload byte |
| PCAP | Inspecting captured packet headers and available payload | Only covers observed, retained packets; encryption can conceal content |
| Strelka result | File metadata and scanner/rule matches | Depends on extraction, supported file types, and configured analysis |
| Endpoint log | Connecting activity to a host, process, account, or event | Depends on agent configuration and available log sources |

**Correlation** means relating records by time, identifiers, and context. It strengthens an explanation but does not itself establish causation. **False positive** means a detection flagged benign activity; **false negative** means relevant activity was not detected. An absence of alerts cannot prove an absence of compromise.

## Worked training example

Assume an authorized exercise generates a DNS lookup followed by a web connection from `training-client` to `training-server`. These names are illustrative, not kit defaults.

- Observation: the correct sensor has DNS and connection records within the planned interval.
- Supporting evidence: the DNS response address matches the later connection destination; captured packets show a request/response exchange if the protocol content is readable.
- Endpoint corroboration: an available process event associates the connection with the expected test application.
- Defensible conclusion: “The sensor observed the planned exchange during this interval, and endpoint evidence is consistent with the test.”
- Unsupported conclusion: “The whole network is healthy and uncompromised.” One successful exchange says nothing about unseen segments or other times.

Use [[JCHK Collaboration and Reporting]] to turn this reasoning into an auditable case record.

## Related notes

[[JCHK Analyst Tools and Evidence Handling]] · [[JCHK Elastic Agent and Fleet]] · [[JCHK Velociraptor Endpoint Forensics]] · [[JCHK Capstone Practice]]

## Sources

- [[JCHK Sources#V3|Volume 3]], PDF pp.46–51: SOC functions and endpoint visibility.
- [[JCHK Sources#L09|Lesson 9]], PDF pp.14–22: Console, Grid, health, hunting interfaces.
- [[JCHK Sources#L10|Lesson 10]], PDF pp.2–6: analyze traffic, identify logs, and pivot to supporting PCAP.
- [[JCHK Sources#L03|Lesson 3]], PDF pp.20–24, 29, 32: component and tool roles. The step-by-step reasoning and example above are explanatory synthesis.
