---
tags: [jchk, lifecycle, readiness, validation]
source_version: "0.6.1"
---
# JCHK Post-Deployment Readiness

Finishing [[JCHK JAKD Deployment]] means the kit has been installed. **Operational readiness** means its components are connected in the intended mission layout, users can access services securely, clocks agree, required licenses and first-run settings are present, and collection can be verified.

Use this note after the deployment playbooks succeed, or as a checklist when investigating why a newly built kit appears partly functional. Begin at [[JCHK Start Here]] for the overall design.

## Complete the Analyst Site firewall integration

The Analyst Site uses a Netgate 6100 MAX running **pfSense**, the firewall/router software. The deployment guide treats its initial setup as manual work: reset the appliance for a clean build, import `config-fw-site-0.xml`, finish the WireGuard relationship with Hunt Site 1, establish routing, and disable the temporary LAN4 setup interface. **XML** is a structured text format used by the configuration file.

The reset and import change the device's configuration and access paths. In the described sequence, initial access is through LAN1 at `https://192.168.1.1`, with the laptop using DHCP. After import, LAN1 takes its operational role, and temporary access to that setup address moves to LAN4. Later, access from the Analyst Site's operational network is confirmed before LAN4 is disabled. Do not disable the only working management path prematurely. [[JCHK Sources#V2|Volume 2]], PDF pp. 64–67, 75–76.

**WireGuard** creates an encrypted VPN, or virtual private network, connection between sites. A **public key** identifies one side to its peer; its matching **private key** must remain secret. A **preshared key** is additional secret material shared by both ends. Generating a new key pair replaces the previous identity; the opposite peer must then receive the correct new public key. A tunnel can be defined yet unusable if keys, allowed networks, routes, or external endpoints disagree.

Volume 2 describes creating Hunt Site 1's tunnel, assigning its virtual interface, adding a peer, creating a gateway/static route, and exchanging the final public keys and preshared key with the Analyst Site. A **static route** is an explicitly configured instruction for how to reach another network. The source briefly uses a placeholder public-key value while constructing the peer; that placeholder is not the final working key. The detailed addressing varies across the source package, so resolve the actual baseline through [[JCHK Source Discrepancies]] rather than mixing tables. [[JCHK Sources#V2|Volume 2]], PDF pp. 68–75.

## Change from temporary deployment cabling to mission cabling

Pre-deployment connections let the Provisioner reach systems before their final site services exist. The operational layout removes the documented temporary provisioning connections and temporary links between Hunt Site 1 and Sites 2/3. Firewall WAN connections then use the mission partner network. **WAN**, wide area network, describes the external connectivity used to reach other sites.

After the temporary links are removed, manually enable the IPMI-network DHCP services on Hunt Sites 2 and 3 as documented. **IPMI**, Intelligent Platform Management Interface, is hardware management separate from the host OS; **DHCP** supplies addresses automatically. During deployment, the Provisioner had served those networks across the temporary connections. Enabling local DHCP too soon can leave multiple DHCP servers on one **broadcast domain**, the group of interfaces that receive the same local broadcast traffic.

The sequencing rule is clear even though address values conflict elsewhere: remove the temporary links first, then enable the correct site's local DHCP service with its validated pool. A **DHCP pool** is the range of addresses the server may hand out. [[JCHK Sources#V2|Volume 2]], PDF pp. 76–84, 95–97.

## Establish certificate trust on every analyst laptop

A **digital certificate** links a service's identity to a public key. A **certificate authority**, or CA, signs certificates so clients can decide which identities to trust. In this kit, the FreeIPA CA certificate must be trusted by analyst laptops.

Volume 2 downloads `ca.crt` from the Provisioner and imports it into Windows's machine-wide trusted-root store. The source workflow uses administrative PowerShell:

```powershell
Invoke-WebRequest http://provisioner.kit1.jchk/ipa/config/ca.crt -OutFile "C:\Users\Analyst\Desktop\ipa.crt"
Import-Certificate -FilePath "C:\Users\Analyst\Desktop\ipa.crt" -CertStoreLocation "Cert:\LocalMachine\Root"
```

The first command downloads a file; the second changes trust for the whole computer. The hostname and file location shown are the source defaults, so adapt them only to the approved configuration. Because importing a root certificate grants substantial trust, establish that this is the intended kit's CA using the approved provisioning process; do not import an arbitrary certificate merely to silence a browser warning. The source's download uses HTTP rather than HTTPS, making the known, controlled provisioning network particularly relevant. [[JCHK Sources#V2|Volume 2]], PDF pp. 84–85.

## Synchronize clocks

**NTP**, Network Time Protocol, lets systems agree on time. Accurate time makes event timelines meaningful and supports authentication/certificate checks. Volume 2 configures Windows Time to use the two kit time servers, starts it automatically, and requests immediate synchronization:

```powershell
w32tm /config /manualpeerlist:"10.1.16.4 10.1.16.5" /syncfromflags:manual /update
Set-Service -Name w32time -StartupType Automatic
Start-Service w32time
w32tm /resync
```

These commands change the time-service configuration and may adjust the clock. The addresses are Volume 2's baseline examples, not universal addresses for every kit. Straight quotation marks above normalize the typographic quotation marks printed in the PDF. Confirm the actual kit's time sources and verify synchronization rather than assuming the commands succeeded. [[JCHK Sources#V2|Volume 2]], PDF pp. 85–86.

## Licenses and first-run application setup

Security Onion Pro and Elastic licensing are separate tasks. The guide imports the Security Onion license through **Administration → License Key**, then the Elastic license through Kibana's **Stack Management → License Management**. **Kibana** is the web interface used to explore and manage Elastic data. A reachable login page does not prove that licensed features are enabled. Use the allocated license material without copying keys into general notes. [[JCHK Sources#V2|Volume 2]], PDF pp. 86–89.

Mattermost needs an initial administrative account using the generated access information from JAKD, an organization name, and completion of its setup flow. The guide skips internet tool integrations because this kit is air-gapped. The six Gigamon ASF01 TAPs—devices that duplicate network traffic for observation—can also receive optional management IP settings for heartbeat/SNMP monitoring. **SNMP**, Simple Network Management Protocol, reports device-management information; the ASF01 configuration path is its serial console, not SSH. A **serial console** is a direct terminal connection to a device's management port. [[JCHK Sources#V2|Volume 2]], PDF pp. 89–95.

## The readiness gate

Confirm Provisioner DNS/DHCP service health; FreeIPA replica reachability; laptop CA trust; enabled firewall interfaces and established tunnels; and Security Onion access with sensors reporting. A **replica** is another instance holding copied service data. Finally test the intended collection chain and verify recent data, not merely a green deployment checkmark. [[JCHK Sources#V2|Volume 2]], PDF p. 97; [[JCHK Sources#SG|Student guide]], PDF p. 16.

Related: [[JCHK Troubleshooting and Recovery]] · [[JCHK Startup]] · [[JCHK Shutdown and Pack-Out]]
