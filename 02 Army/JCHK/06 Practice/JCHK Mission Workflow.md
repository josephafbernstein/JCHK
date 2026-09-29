---
tags: [jchk, mission, workflow]
baseline: v0.6.1 training snapshot
---
# JCHK Mission Workflow

[[JCHK Start Here]] · [[JCHK Software Map and Baseline]]

The JCHK mission cycle connects preparation, reliable collection, analysis, communication, and an orderly close. The sequence below combines the operational guide and course objectives into a beginner-friendly workflow. It does not replace the approved deployment, network-change, or evidence procedures for an actual mission.

## 1. Define the observation problem

Document the partner's question, the systems and links in scope, the permitted activities, required evidence, and the intended output. A **scope** is the boundary of the work: which assets, times, data, and actions are included. A **hypothesis** is a proposed explanation that can be tested against observations.

For example, “Determine whether the designated training workstations contacted the exercise server during this interval” is testable. “Prove everything is safe” is not a bounded technical question.

Decide what requires network evidence, what requires endpoint evidence, and where the kit must observe it. TAP placement controls network coverage; agent enrollment controls endpoint coverage. The Analyst Site provides access, the Primary Hunt Site provides central analytics, and Secondary Hunt Sites collect nearer their observed segments.

## 2. Prepare and validate the platform

Complete the applicable physical setup, deployment, post-deployment cabling, and prescribed power-up sequence. Then validate prerequisites in dependency order: physical power/link state, switching and routing, site connectivity, DNS/time, central services, sensors, and analyst access.

Use JAKD's Kit Operations view to reach the documented services. Open the relevant interfaces and inspect actual service state. A ping response is only a reachability check. Confirm that the expected software is deployed; the v0.6.1 inventories and roadmap do not establish a live kit's contents.

Record a baseline before ingest starts: nodes present, current health, clocks, available storage/retention, sensor roles, and any known gaps. A **baseline** is a recorded reference state used for later comparison.

## 3. Connect sources and prove the path

For network collection, attach the correct TAP tool feeds or authorized SPAN source to verified monitoring interfaces. Ensure both directions and the intended link speed are supported. For endpoints, prepare mission connectivity, correct server names/certificates, and approved agents.

Generate or identify a benign, known exchange in the approved test environment. Verify its arrival at the intended sensor, corresponding metadata, accessible packets, and any expected endpoint event. This is **end-to-end validation**: testing from the source through collection and processing to the analyst's evidence view.

A local packet counter, central dashboard, and healthy endpoint agent are useful but separate checks. The strongest validation correlates a known activity across the stages needed for the mission.

## 4. Monitor collection while investigating

Watch sensor loss, service health, storage/retention, and site connectivity throughout the operation. Traffic can change after an initial successful test. Record interruption start/end times and the affected scope so that later searches are interpreted correctly.

Investigate using [[JCHK Security Onion Investigation Workflow]]. Preserve useful packets/artifacts before retention expires. Request focused endpoint collection when it answers an unresolved question. Active scans and response actions should be separately recorded because they can alter endpoints and create traffic that later appears in the evidence.

## 5. Communicate findings and decisions

Maintain a case with observation, evidence references, interpretation, alternative explanations, confidence, impact, and next actions. Use collaboration tools for coordination and link back to the case. A **handoff** transfers the state of work to another person/team; it should include ownership, open questions, and evidence expiry risks.

Do not confuse “no relevant result in the searched data” with “the event did not happen.” State which sources, hosts, sites, and times the search actually covered. Explain material packet loss, missing agents, inaccessible Heavy nodes, or absent logs.

## 6. Close the mission deliberately

Preserve required exports and the collection record, account for endpoint agents and approved removal, document changes and remaining issues, and verify the storage destination. Then follow the documented shutdown/pack-out sequence. Do not remove power before dependent services and storage have been shut down according to the kit procedure.

Useful closeout questions are: Can another analyst reproduce the important result? Are original evidence exports preserved? Is the disposition of every installed agent known? Are configuration changes documented? Are outstanding problems and collection gaps handed over?

## Related notes

[[JCHK Service Portal and Identity]] · [[JCHK TAPs and Traffic Visibility]] · [[JCHK Sensor Health and Packet Loss]] · [[JCHK Collaboration and Reporting]] · [[JCHK Capstone Practice]]

## Sources

- [[JCHK Sources#V3|Volume 3]], PDF pp.9, 11–13, 22–23, 74–75, 79–83: operational scope, readiness, maintenance, troubleshooting.
- [[JCHK Sources#L10|Lesson 10]], PDF pp.2–6: integrated operational tasks.
- [[JCHK Sources#L09|Lesson 9]], PDF pp.4, 17–22, 25–37: visibility, health, endpoint collection.
- [[JCHK Sources#APP|Appendices]], PDF pp.25–26, Appendix D: prescribed power sequencing. The mission-level sequence is explanatory synthesis.
