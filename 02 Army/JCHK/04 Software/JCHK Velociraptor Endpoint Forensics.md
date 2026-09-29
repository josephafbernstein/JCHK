---
tags: [jchk, endpoint, velociraptor, forensics]
baseline: v0.6.1 training snapshot
---
# JCHK Velociraptor Endpoint Forensics

[[JCHK Start Here]] · [[JCHK Elastic Agent and Fleet]]

**Velociraptor** is JCHK's separate platform for endpoint monitoring, digital forensics, and incident response. It uses a server and installed endpoint clients. **Digital forensics** is the methodical collection and examination of digital evidence. An **artifact** is a useful trace of activity, such as a log entry, file, configuration record, or process observation. In Velociraptor terminology an artifact can also mean a defined collection/query procedure; distinguish the collection definition from its returned evidence.

## When to use it

Elastic Agent/Fleet supplies ongoing visibility integrated into Security Onion. Velociraptor supports focused collection and investigation across enrolled systems. Lesson 9 also describes response capabilities, including quarantine and remediation. Those are changes to an endpoint, distinct from observing it; choose the least disruptive action sufficient for the approved task.

For example, a Security Onion event may identify a workstation of interest. A targeted Velociraptor collection can help identify corresponding host activity. The collection result becomes additional evidence; it does not automatically validate the initial alert or establish attribution.

## Server access versus client communication

The documented analyst web interface is `https://velociraptor.kit1.jchk:8889`, with credentials found in JAKD. **8889 is the analyst interface port in this source**, not the port to expose for every client. Volume 3's mission-network forwarding table separately identifies TCP **8000** for client communications, with example target `10.1.21.70`.

The client must have a reachable server URL and a compatible configuration. A **URL** identifies a service location, including scheme, hostname, optional port, and path. An **FQDN** is the full hostname/domain portion. A working analyst web session does not prove an endpoint can reach the client service from a different network.

## Client configuration and packages

The server provides `client.root.config.yaml` through its home page under **Current orgs**. **YAML** is a structured text format used for configuration. The client configuration contains the connection information used by the installed client; preserve it as configuration material rather than treating it as an arbitrary example file.

For Linux, the materials describe building a package that bundles the executable, configuration, and service setup. **DEB** packages target Debian-family systems; **RPM** packages target Red Hat-family systems. A **daemon** is a service that runs in the background. For Windows, the source describes an executable plus YAML installed as a Windows service; it also identifies MSI package support.

Use the actual deployed binary and version. The source's commands mix `/opt/container/` and `/opt/containers/`, use different example versions, and misspell the Windows executable as `velocirator`. These are reasons to verify files and paths before preparing an installation command, not to copy the printed strings verbatim.

## DNS, NAT, and validation

Mission endpoints may not resolve `velociraptor.kit1.jchk`. The documented options are a mission DNS entry or a hosts-file mapping to the mission-reachable Hunt Site firewall address. DNS is name-to-address resolution; NAT forwards that reachable address to the internal client service. They solve different parts of the path.

Lesson 9 also says to update `server_urls` in the YAML. Keep the selected URL, certificate identity, name resolution, network path, and package configuration consistent. A hosts-file edit alone cannot correct a wrong service port or incompatible certificate.

A useful validation sequence is:

1. Record the intended endpoint and authorized collection scope.
2. Confirm the package/configuration comes from the intended JCHK server.
3. Install using the required administrative privileges and inspect the resulting service state.
4. Verify that the expected endpoint checks in to the intended server with a recent timestamp.
5. Run a small approved collection and verify its result identifies the correct host and time.
6. Record collection parameters and preserve the returned evidence before broadening the task.

## Interpret and preserve results

A collection is a view of what the selected procedure could obtain at that time. A missing record can mean absence, insufficient privileges, unavailable data, collection failure, or expiry. Record failures alongside successful results; do not silently treat incomplete endpoints as clean.

Document endpoint identity, collection definition, parameters, start/end times, tool version, errors, export location, and integrity information where appropriate. Preserve original exports and work from copies. At mission end, coordinate client removal, confirm removal, and document the end of monitoring. The source provides removal examples, but package names, quoting, and paths should be checked against the installed version.

## Related notes

[[JCHK Security Onion Investigation Workflow]] · [[JCHK Analyst Tools and Evidence Handling]] · [[JCHK Service Portal and Identity]]

## Sources

- [[JCHK Sources#V3|Volume 3]], PDF pp.32, 52–54: port distinction, server access, packages/configuration, DNS, removal.
- [[JCHK Sources#L09|Lesson 9]], PDF pp.25–30: Elastic/Velociraptor distinction and client workflow.
- [[JCHK Sources#L03|Lesson 3]], PDF p.24: platform purpose.
