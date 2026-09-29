---
tags: [jchk, collaboration, reporting]
baseline: v0.6.1 training snapshot
---
# JCHK Collaboration and Reporting

[[JCHK Start Here]] · [[JCHK Security Onion Investigation Workflow]]

Collection becomes useful when the team can understand what was observed, how it was interpreted, and what should happen next. JCHK's software categories include incident case management, mission coordination, and document production. The supplied materials describe Security Onion Console case functionality and Mattermost collaboration, plus laptop productivity tools.

## Cases, messages, and reports serve different purposes

A **case record** organizes an investigation's evidence, timeline, analysis, and decisions. A chat message coordinates work quickly. A report communicates conclusions to the intended audience. A chat assertion without linked evidence is a weak substitute for a case record.

Use these structures together:

- **Case:** the durable technical record and links to evidence.
- **Coordination channel:** ownership, status changes, assistance requests, and links back to the case.
- **Report or briefing:** clear findings, impact, confidence, and recommended next actions.

A dashboard helps visualize data, but its appearance depends on the selected source, query, time interval, and filters. Record these when a dashboard supports a finding. A screenshot without those details may be impossible to reproduce.

## Mattermost in the supplied guide

**Mattermost** is a self-hosted collaboration application for chat and file sharing. **Self-hosted** means the organization runs the service in its own environment. The operational guide places the server in a container within JCRS-D Edge and provides `https://mattermost.kit1.jchk` as its local address. Analyst laptops can use a browser or the Mattermost desktop client.

The guide's desktop-client sequence is: open the client, choose **Get Started**, supply the server URL and a meaningful display name, choose notification permissions, and sign in with an existing account. Its invitation workflow uses **Invite people → Copy invite link** from a logged-in account. These describe application behavior; no invitations are being sent by these notes.

**Air-gapped** means separated from external networks. A locally hosted collaboration service can operate without Internet access if its kit dependencies are available. This does not mean every tool, license activation, or external update source will work offline.

Mattermost's deployment status is inconsistent across sources: Lesson 3 flags it as not integrated in some slides, while Volumes 2 and 3 describe installation and use. First verify whether the service exists in the actual kit. The same caution applies to Office/Visio, Samba file sharing, and optional tools: do not build a mission process around an unverified inventory item.

## A finding template

The template below is a teaching aid, not a supplied program form:

```text
Case / finding identifier:
Question investigated:
Scope: sites, hosts, observed links, time interval and time zone
Coverage limits: missing sources, retention boundary, packet loss
Observation: what the evidence directly shows
Evidence references: sensor/host, event IDs, query, export names
Interpretation: what the observations may mean
Alternatives considered:
Confidence and remaining uncertainty:
Operational impact:
Recommended next action and owner:
Actions already taken, by whom, and when:
```

**Confidence** describes how strongly evidence supports the conclusion. It should depend on corroboration, completeness, and alternative explanations, not on how severe an alert label sounds. **Severity** concerns potential impact; a high-severity detection can still have low evidentiary confidence.

## Keep the record reproducible

Use a consistent time zone and explicit timestamps. Record actual host identity alongside changing network addresses. Include the sensor/site because different sites may observe different parts of a conversation. Preserve the query and filters so another analyst can repeat the search. Where exported evidence is used, link its collection record and integrity information.

An effective handoff states what is known, what remains uncertain, who owns the next action, and when evidence might expire. Avoid pasting credentials or unnecessary sensitive data into broad channels. Current credentials belong in the authorized credential process, not an investigation narrative.

At mission close, account for open cases, retained exports, agent removal decisions, configurations changed, and any monitoring gaps. The technical conclusion and the kit's operational status are separate: “test capture succeeded” and “no actionable threat found within this scope” are different claims.

## Related notes

[[JCHK Analyst Tools and Evidence Handling]] · [[JCHK Mission Workflow]] · [[JCHK Capstone Practice]] · [[JCHK Service Portal and Identity]]

## Sources

- [[JCHK Sources#V3|Volume 3]], PDF pp.54–57: Mattermost server, invitations, desktop connection.
- [[JCHK Sources#L03|Lesson 3]], PDF pp.11, 32, 35–36: mission coordination/category placement and conflicting availability flags.
- [[JCHK Sources#L09|Lesson 9]], PDF pp.17–22: dashboards and operational context.
- [[JCHK Sources#APP|Appendices]], PDF pp.10, 12–15: collaboration/productivity inventory. Reporting template and reasoning guidance are explanatory synthesis.
