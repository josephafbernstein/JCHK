---
tags: [jchk, software, identity]
baseline: v0.6.1 training snapshot
---
# JCHK Service Portal and Identity

[[JCHK Start Here]] · [[JCHK Software Map and Baseline]]

The **Provisioner** is the JCHK's installation and administration hub. Its web application, **JAKD** (Joint Automated Kit Deployer), provides a practical starting point for reaching deployed services. The materials also call the deployer Zepharis or ZKD; these names reflect an in-progress rebranding.

## Begin with the portal

In the kit's configured network, open `https://jakd/` and use the assigned credentials. This is a kit-local address: it depends on the kit's naming and network services and is not a public Internet website. After deployment, select **Kit Operations**.

| View | What it provides | When to use it |
|---|---|---|
| Category View | Cards for major services, with tool URL and credential controls | Opening the main application for an analyst task |
| Table View | Searchable records of hosts, services, URLs, and credentials | Finding a specific component or administrative login |

The portal also shows a heartbeat or ping check. A **ping** tests a form of network reachability; it does not prove that a web application, authentication service, or data pipeline is healthy. Follow the link, authenticate, and check actual functionality.

The remaining tabs have separate purposes: **Hardware Configuration** describes kit equipment; **Network Layout** shows connectivity; **Software Layout** configures software; **Deploy Software** runs automation; **Documentation** provides local references; and **Settings** manages JAKD users/configurations. Do not confuse opening a tool with redeploying it.

The notes intentionally omit sample/default passwords. Retrieve current credentials from the authorized kit records. The variable name `so_web_password` identifies a generated Security Onion secret; it is not a literal password to type. A web-interface account and a host's Secure Shell account may have different usernames and credentials.

## What FreeIPA does

**FreeIPA** combines identity and infrastructure services. Identity is how a user or machine is named and represented. **Authentication** establishes who it is; **authorization** decides what it can do. These are separate checks: a valid login need not have administrator privileges.

- **DNS (Domain Name System)** maps hostnames such as `so-manager.kit1.jchk` to network addresses.
- **NTP (Network Time Protocol)** supports clock synchronization. Consistent time is essential when matching events across sensors and endpoints.
- **LDAP (Lightweight Directory Access Protocol)** accesses a directory of identities and attributes. **LDAPS** secures LDAP communication with encryption.
- **Kerberos** uses centrally issued tickets for authentication rather than repeatedly sending a password to each service.
- A **certificate authority (CA)** issues digital certificates that bind identities to cryptographic keys. A certificate helps a client establish that it reached the intended service.

Not every host uses every FreeIPA function. Lesson 8 specifically distinguishes systems using only DNS/time from those using full directory integration. **Citadel** is described separately as JCRS-D's identity/authentication solution; do not assume all interfaces share one sign-on simply because they belong to the kit.

## Infrastructure dependencies explain common failures

A **fully qualified domain name (FQDN)** identifies a host together with its domain, for example `velociraptor.kit1.jchk`. The shorter name `velociraptor` may depend on a configured search domain. If a short name fails, that does not alone establish a service failure.

If many unrelated tools fail simultaneously, look for shared dependencies:

1. Can the workstation reach its local network and the kit route?
2. Does the exact service name resolve to the expected address?
3. Are client/server clocks aligned?
4. Is the service running, and is its access port reachable?
5. Does the certificate match the name being used and a trusted issuer?
6. Does the correct account have the required access?

Record the error before changing anything. Replacing a certificate, changing DNS, or restarting an identity service affects multiple users and should follow the kit's approved operational process.

## Address discrepancy to resolve

Volume 3, PDF p.43, gives the primary FreeIPA address as `10.1.16.4`; Lesson 8, PDF p.14, gives `10.1.17.4` and `provisioner.kit1.jchk/ipa/ui/`. These cannot be silently treated as equivalent. Use the deployed JAKD record, active configuration, and network baseline to verify the address. A proposed FreeIPA replica is marked absent from v0.6.1 in Lesson 8; do not assume identity redundancy.

## Related notes

[[JCHK Elastic Agent and Fleet]] · [[JCHK Velociraptor Endpoint Forensics]] · [[JCHK Mission Workflow]]

## Sources

- [[JCHK Sources#V3|Volume 3]], PDF pp. 40–43: portal, credentials, heartbeat, FreeIPA.
- [[JCHK Sources#L08|Lesson 8]], PDF pp. 8–14: Provisioner, portal tabs, FreeIPA functions and differing address.
- [[JCHK Sources#L03|Lesson 3]], PDF pp. 14–18, 42–43: naming, identity roles, Provisioner services.
- [[JCHK Sources#APP|Appendices]], PDF p.14: Provisioner services.
