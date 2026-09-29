---
tags: [jchk, taps, sensors, network]
baseline: v0.6.1 training snapshot
---
# JCHK TAPs and Traffic Visibility

[[JCHK Start Here]] · [[JCHK Security Onion Architecture]]

A **network TAP** is a device that copies traffic from a network link to monitoring equipment. The JCHK includes six Gigamon ASF01 inline TAPs. **Inline** means the TAP is physically inserted into the observed communication path. Its network ports carry the original traffic; its tool ports supply copies to a sensor.

The source diagrams distinguish this observation path from the normal management network. A sensor can be manageable over its infrastructure interface while its capture interfaces receive no traffic. Conversely, a sensor may capture locally while a management or VPN connection is unavailable.

## The four ports and two directions

The ASF01 monitors one link using two network ports, **N1A/N1B**, and two tool ports, **T1A/T1B**.

```text
Direction A: incoming N1A -> forwarded out N1B
                         -> copied out T1A to sensor

Direction B: incoming N1B -> forwarded out N1A
                         -> copied out T1B to sensor
```

**Bidirectional** means traffic travels both ways. **Full duplex** means both directions can operate at once. Connecting only one tool output can leave the analyst with one side of a conversation: requests without responses, for example. The two tool feeds are complementary directions, not automatically redundant copies of the entire exchange.

Lesson 9 describes the ASF01's battery backup as approximately one hour. This is a training statement, not proof of current battery health or guaranteed runtime. Inserting or moving an inline device alters the physical path, so it belongs in the coordinated setup plan.

## Speed and medium are different

Each ASF01 port supports 1/10GbE SFP/SFP+ transceivers. **GbE** names an Ethernet link rate in gigabits per second. A **transceiver** converts the port's signal to the chosen physical medium. **Fiber** carries optical signals; copper Ethernet cabling carries electrical signals.

The source permits changing the medium between the network and tool sides while preserving speed. For example, 10GbE fiber on the network side can feed 10GbE copper toward the sensor. A 1GbE network-side link feeding 10GbE tool-side optics is explicitly unsupported in the training description. Both network ports must use the same transceiver type; the tool-side transceivers must support the same speed.

A sensor's 25GbE-capable port does not turn a 10GbE TAP into a 25GbE observation source. Port capability, installed transceiver capability, and negotiated/required link rate all matter.

## TAP versus SPAN

**SPAN (Switched Port Analyzer)**, also called port mirroring, is a switch feature that copies selected traffic to a monitoring port. The Security Onion sensors can consume TAP or SPAN feeds. TAP placement and SPAN selection determine coverage: neither reveals traffic that never crosses the monitored link or selected switch source.

A mirrored destination can become oversubscribed when selected traffic exceeds its output capacity. A **packet broker** is a separate function that may filter, combine, or distribute traffic for tools. The basic ASF01 is not documented as a configurable aggregation/brokering platform. Do not transfer features from a glossary entry for other Gigamon products to this TAP.

## Sensors turn copied traffic into evidence

In the documented baseline, the SN 9000 is a Heavy sensor and the SN 3100 a Forward sensor. Both run Zeek, Suricata, and Strelka and retain packet capture locally. The SN 9000 has four default monitoring interfaces; the SN 3100 has two. Their management links have a separate purpose.

The sources disagree about the SN 3100 infrastructure port: Lesson 9 p.12 says port 5/`eno5`; Lesson 8 p.19 and Volume 3's post-deployment tables identify port 6. Do not use this concept note as the deciding physical port map; verify the active image and approved cabling baseline.

## Prove visibility rather than assuming it

Use an approved training exchange with a known time, source, destination, and protocol. Confirm both directions reach the expected sensor, verify matching metadata and packet evidence, then check loss and retention. If a direction is missing, investigate the feed before drawing conclusions about application behavior.

Encrypted traffic remains encrypted merely because it was copied. Sensors may still expose useful addresses, timing, volume, and some protocol metadata, but plaintext content requires visibility that the TAP alone does not provide.

## Related notes

[[JCHK Sensor Health and Packet Loss]] · [[JCHK Security Onion Investigation Workflow]] · [[JCHK Capstone Practice]]

## Sources

- [[JCHK Sources#L09|Lesson 9]], PDF pp.6–13: TAP direction mapping, speed/medium, management, sensor roles/ports.
- [[JCHK Sources#V3|Volume 3]], PDF pp.35–37: monitoring interfaces, port roles, compatibility.
- [[JCHK Sources#L08|Lesson 8]], PDF p.19: conflicting SN 3100 management-port statement.
- [[JCHK Sources#APP|Appendices]], PDF pp.11, 53–54: sensor roles and packet-brokering terminology. General coverage limits are explanatory networking context.
