---
tags: [jchk, endpoint, elastic]
baseline: v0.6.1 training snapshot
---
# JCHK Elastic Agent and Fleet

[[JCHK Start Here]] · [[JCHK Security Onion Architecture]]

**Elastic Agent** is software installed on a monitored endpoint to collect telemetry such as host logs, process activity, system information, and security events. An **endpoint** is a device such as a workstation or server. **Fleet** centrally enrolls and manages these agents. In JCHK, Fleet is integrated with Security Onion and runs in the DMZ.

**EDR**, endpoint detection and response, uses endpoint observations to support detecting and investigating threats. The sources describe Elastic Agent/Fleet for continuous endpoint visibility and Velociraptor for focused forensic investigation. They do not identify Wazuh as part of this baseline.

## Network observation and endpoint observation complement each other

A sensor may see a connection between two addresses without knowing which process or user caused it. An endpoint agent can supply that context when the required data source is enabled. Examples from Lesson 8 include Windows event logs, PowerShell logs, Sysmon if installed, and Linux authentication logs.

**Sysmon** is a Windows system-monitoring component that can generate detailed event records. Installing Elastic Agent does not automatically prove that Sysmon or every desired logging source is present. Verify a representative event, not only agent enrollment.

## The documented connection path

```text
Mission endpoint with Elastic Agent
  -> mission-reachable Hunt Site 1 firewall address
  -> permitted/NAT-forwarded service ports
  -> Fleet host in the DMZ
  -> Security Onion ingestion and searchable records
```

**NAT (Network Address Translation)** changes addressing between networks. A **port forward** directs traffic arriving at a firewall address/port to a specific internal service. The v0.6.1 procedure uses TCP ports **8220** and **5055**, redirected to the Fleet host; Volume 3's example target is `10.1.21.30`. These are documented baseline values, not confirmation of a live installation. Coordinate firewall details with the kit network configuration.

Opening the edge firewall is only one requirement. Fleet's own host firewall must also allow the approved endpoint source range. Lesson 9 illustrates allowing every address with `0.0.0.0/0`; that notation means all IPv4 addresses. It is an example, not a necessary deployment choice. Use the source ranges required by the actual mission.

## Enrollment sequence and validation

1. Confirm the mission's permitted endpoints, supported operating systems, logging needs, and installation privileges.
2. Confirm the name/address agents can actually reach from the mission network. Internal kit DNS names may not resolve there.
3. Reconcile Fleet's advertised endpoint with routing, NAT, DNS, and the server certificate.
4. In SOC configuration, the supplied procedure identifies `elasticfleet → config → server → custom_fqdn` for the external endpoint setting and `firewall → hostgroups → elastic_agent_endpoint` for source access.
5. Allow configuration propagation and obtain the newly generated installer from SOC **Downloads**. Volume 3 says changes take about 15 minutes to populate; verify the installer reflects the change rather than relying solely on elapsed time.
6. Install the appropriate Windows/Linux/macOS package with Administrator/root access from a writable location. The guide warns against running it from a mounted read-only CD/DVD. Review the installation log.
7. Open **Elastic Fleet → Agents** and verify the endpoint is healthy. Then confirm an expected benign test event becomes searchable with the correct host and time.

## Important certificate and naming discrepancy

A **TLS certificate** helps the agent verify the server's identity. It must be valid for the name/address the client actually uses; reaching the right port is not enough.

Volume 3 instructs operators to configure an external NAT address, then discusses internal-name certificates. Its host-file example uses `so-fleet-01` while surrounding text uses `so-fleet-1`, and it shows an internal address in a section calling for an external address. Lesson 9 also gives a different example Manager address. These examples are not safe to copy mechanically.

A **hosts file** maps names to addresses locally; it changes name resolution but does not add certificate identities. Verify the active Fleet name, certificate identities, external reachable address, and generated agent configuration together. Do not bypass certificate verification to hide a mismatch.

## Ending collection

Agent removal is a separate, planned task after evidence preservation and mission coordination. Volume 3 supplies OS-specific uninstall procedures and cautions that the Elastic uninstall command must be run outside its installation directory. Use the installed version's validated procedure, confirm the service is removed, and record the end of coverage.

## Related notes

[[JCHK Service Portal and Identity]] · [[JCHK Velociraptor Endpoint Forensics]] · [[JCHK Security Onion Investigation Workflow]]

## Sources

- [[JCHK Sources#V3|Volume 3]], PDF pp.32, 48–51: port forwards, Fleet configuration, installers, naming issues, verification/removal.
- [[JCHK Sources#L08|Lesson 8]], PDF p.39: Fleet role and example log sources.
- [[JCHK Sources#L09|Lesson 9]], PDF pp.25–26, 31–37: EDR distinction and agent workflow.
