---
tags: [jchk, hardware, storage]
source_version: "0.6.1"
---

# JCHK Storage Capacity and Retention

Storage answers two different questions: **how much data can the Kit hold**, and **how long will a particular kind of evidence remain available**? The first is a capacity question; the second is a retention question. Neither can be answered reliably using a headline petabyte figure alone.

## The three storage jobs

1. **Operating-system storage:** holds the software that starts and runs each computer. An **operating system (OS)** manages the computer's hardware and programs.
2. **Sensor evidence storage:** holds **packet capture (PCAP)**, the recorded packet bytes, and on the large sensor also locally indexed logs/metadata. **Metadata** describes observations; an **index** organizes them for efficient search.
3. **Analytics-cluster storage:** holds data and virtual disks used by central applications on the SN 7100 cluster. A **cluster** is a group of cooperating computers; a **virtual disk** is storage presented to a virtual machine as if it were a physical drive.

These storage pools are not automatically interchangeable. A large amount of cluster free space does not prove that a remote sensor has sufficient packet-capture capacity.

## Raw capacity from the listed drive counts

**Raw capacity** is the sum of drive capacities before protection, formatting, reserved space, and application allocation. In the calculations below, one **terabyte (TB)** is one trillion bytes and one **petabyte (PB)** is 1,000 TB. A **byte** is eight bits; network rates are usually stated in bits per second, while drive capacities are stated in bytes.

| Storage group | Documented drive arrangement | Derived raw bulk capacity |
|---|---|---:|
| One SN 7100 | 3 × 61.44 TB | 184.32 TB |
| Three SN 7100s | 9 × 61.44 TB | 552.96 TB |
| One ON-DoWIN SN 9000 | 6 × 61.44 TB | 368.64 TB |
| One OFF-DoWIN SN 9000 | 14 × 61.44 TB | 860.16 TB |
| One SN 3100 | 1 × 30.72 TB | 30.72 TB |
| Three large + three small sensors, ON-DoWIN | 3 × (368.64 + 30.72) TB | 1,198.08 TB, approximately 1.20 PB |
| Three large + three small sensors, OFF-DoWIN | 3 × (860.16 + 30.72) TB | 2,672.64 TB, approximately 2.67 PB |
| Analytics bulk + sensor bulk, ON-DoWIN | 552.96 + 1,198.08 TB | 1,751.04 TB |
| Analytics bulk + sensor bulk, OFF-DoWIN | 552.96 + 2,672.64 TB | 3,225.60 TB |

These are arithmetic derived from the supplied specifications, excluding OS drives, laptop drives, flex-server drives, and external media. They are not measured usable capacity. The source itself notes that delivered storage may vary.

## Why usable capacity is smaller

**RAID**, Redundant Array of Independent Disks, organizes multiple drives for capacity, performance, and/or failure protection. **RAID 1** mirrors data onto another drive: two 1.92 TB OS drives provide approximately one drive's capacity before formatting. **RAID 6** uses distributed parity equivalent to two drives' capacity to tolerate two drive failures within a correctly functioning array. **Parity** is redundant information used to reconstruct missing data. **JBOD**, Just a Bunch of Disks, exposes drives without combining them into a protective RAID arrangement at that layer.

Volume 2 recommends RAID 6 for SN 9000 bulk storage. Under that specific equal-drive arrangement, an illustrative calculation is:

- ON-DoWIN: `(6 − 2) × 61.44 = 245.76 TB` before formatting and other overhead.
- OFF-DoWIN: `(14 − 2) × 61.44 = 737.28 TB` before formatting and other overhead.

These examples do not establish the current array state. Changing RAID configuration can destroy existing data; these notes explain the capacity consequence, not a procedure to reconfigure storage.

The SN 7100 cluster uses **Ceph**, distributed storage software, managed through **Rook**, a Kubernetes operator that automates Ceph management. An **operator** is software that watches a desired configuration and manages its application's lifecycle. Ceph's data-protection policy can also consume capacity. Do not divide the raw total by an assumed replication count unless the actual pool configuration confirms it. This general distinction is supported by the [Rook project overview](https://rook.io/docs/rook/latest-release/Getting-Started/intro/); the Kit's use of Rook/Ceph is documented in Lesson 6.

## Estimating packet retention

**Retention** is how long data stays available under the configured policy. A simple estimate is:

`retention time ≈ usable bytes allocated to capture ÷ average bytes written per second`

For illustration only, a constant recorded rate of 10 gigabits per second equals 1.25 gigabytes per second, or 108 decimal TB per day. At that simplified rate, 245.76 TB lasts about 2.28 days and 737.28 TB about 6.83 days, before further overhead or reserved space. This assumes that 10 Gbps is the total stored input, not 10 Gbps in each direction.

Actual retention varies with average traffic, capture filtering, packet truncation settings, bidirectional load, file/index overhead, storage policy, and the allocation shared with other data. **Full duplex** means a link can carry traffic simultaneously in both directions; the speed printed on a port alone does not tell you the combined recorded rate.

Lesson 1 advertises approximately 360/860 TB FPCAP per stack and three/seven days at 10 Gbps. Those headline estimates should not be treated as a guaranteed retention period after RAID and other allocations. [[JCHK Source Discrepancies]] retains the original claims alongside their limitations.

## Performance and endurance are separate limits

**Sequential throughput** measures reading/writing a stream of data. **IOPS**, input/output operations per second, measures the rate of individual storage operations. **Latency** is the time to complete an operation. A drive's maximum sequential speed alone does not establish application performance.

**TBW**, terabytes written, is a drive-endurance rating for total writes. **DWPD**, drive writes per day, expresses daily writes relative to the drive's size over a stated period. These are endurance ratings, not a guarantee of exactly when a drive will fail. Volume 1 lists both for the supplied drive sizes. Monitoring capacity and health is therefore different from merely verifying that the drives are present.

Redundancy preserves service through certain failures; a **backup** is a separately recoverable copy. Mirroring or replicating an accidental deletion is not the same as retaining a recoverable historical copy. Evidence-preservation policy remains a separate operational responsibility.

## Sources

- [[JCHK Sources#V1|Volume 1]], PDF pp. 27–30, 32–35: storage counts, raw-capacity caveats, endurance metrics.
- [[JCHK Sources#V2|Volume 2]], PDF p. 126: recommended SN 9000 RAID 6 configurations and destructive-change warning.
- [[JCHK Sources#L01|Lesson 1]], PDF pp. 6–8: headline capacity and retention figures.
- [[JCHK Sources#L06|Lesson 6]], PDF pp. 26, 31: Rook/Ceph storage in JCRS-D Edge.
- [Rook overview](https://rook.io/docs/rook/latest-release/Getting-Started/intro/): general Ceph/Rook roles, consulted to clarify terminology rather than change the v0.6.1 baseline.

## Related notes

[[JCHK Start Here]] · [[JCHK Servers Sensors and Flex Nodes]] · [[JCHK Data Journey and Resilience]] · [[JCHK JCRS-D and Kubernetes]] · [[JCHK Sensor Health and Packet Loss]]
