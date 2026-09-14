# Data Center and Virtualization — MCQ Bank
**Subject:** Computer Science / IT Infrastructure · **Files:** 2 · **Total questions:** 90
**Target:** BCS Preliminary / Bank IT Officer / NTRCA / GATE / IBPS SO IT

| File | Type | Questions | Added |
|---|---|---|---|
| [mcq_data-center-and-virtualization.md](mcq_data-center-and-virtualization.md) | Complete bank, part 1 (Q1–Q40) | 40 | 2026-09-14 |
| [mcq_data-center-and-virtualization_part2.md](mcq_data-center-and-virtualization_part2.md) | Complete bank, part 2 (Q41–Q90) | 50 | 2026-09-14 |

Both files were written in a single run. The split is a file-size decision, not a deferral — the chapter is
covered end to end, virtualization first (Sections 1–5) then data center infrastructure (Sections 6–10).

**Verified on delivery.** Answer letters A/B/C/D = **22/22/23/23** (near-uniform; drift was fixed by
reordering options, never by changing which fact is correct). Difficulty: **Core 42 · Applied 41 · Trap 7**.
Worked calculations: **8 of 90** carry an explicit computed/verified numeric figure (PUE/DCIE, RAID usable
capacity, tier availability↔downtime conversion, VXLAN's ID-space math). Inventory coverage **71/71, 0
empty** (concept ids span C1–C75; four ids were folded into neighboring concepts while drafting, so the id
range is wider than the item count — every id that does appear in the Concept Index is fully drilled).
Structure complete on all 90 questions; zero "All of the above" / "None of the above" options.

**Cross-reference, not a gap.** IP addressing, subnet arithmetic and general LAN switching/routing
fundamentals are covered by the sibling **[Data Communication and Networking](mcq_data-communication-and-networking.md)**
and **[Subnetting](mcq_subnetting.md)** chapters. This chapter treats networking only where it is
specifically data-center-scale — spine-leaf/Clos fabrics, top-of-rack design, and VXLAN's role in
multi-tenant scale — without re-deriving OSI layers or subnetting math those chapters already own.

## Concepts covered

**Section 1 — Virtualization Fundamentals & Hypervisor Types (Q1–10):** the definition of virtualization ·
hypervisors/VMMs · Type 1 (bare-metal) vs Type 2 (hosted) hypervisors and real product examples · host vs
guest OS · full virtualization and binary translation vs paravirtualization and hypercalls · hardware-
assisted virtualization (Intel VT-x / AMD-V) · emulation vs virtualization · OS-level virtualization as the
bridge into containers.

**Section 2 — Hypervisor & VM Lifecycle Management (Q11–19):** snapshots vs clones · live migration and its
preconditions (shared storage, network, CPU compatibility) · memory ballooning and overcommitment · thin vs
thick provisioning · P2V conversion · nested virtualization · high availability vs fault tolerance · VM
sprawl as a governance failure.

**Section 3 — Containers & OS-Level Virtualization (Q20–25):** containers vs VMs (shared kernel vs per-guest
kernel) · Docker images vs containers · Linux namespaces and cgroups · why container orchestration
(Kubernetes) exists · microservices as the architectural style that pairs with containers.

**Section 4 — Storage, Network, Desktop & Application Virtualization (Q26–30):** storage virtualization as
pooling/abstraction · network virtualization decoupling logical from physical topology · VXLAN's 24-bit ID
space versus VLAN's 12-bit ceiling · VDI centralizing whole desktop OSes · application virtualization as a
narrower, single-app alternative to VDI.

**Section 5 — Cloud Computing Service & Deployment Models (Q31–40):** IaaS/PaaS/SaaS and who manages which
layer · public, private, hybrid and community deployment models · elasticity vs scalability · multi-tenancy
· how cloud computing (a delivery model) relates to, but is not synonymous with, virtualization (an enabling
technology).

**Section 6 — Data Center Architecture & Network Design (Q41–49):** the traditional three-tier
(core/aggregation/access) architecture · spine-leaf (Clos) fabrics and why east-west traffic growth drove
their adoption · top-of-rack switching · scale-up vs scale-out · the rack unit (U) standard · colocation vs
on-premises vs cloud · modular/containerized physical data centers.

**Section 7 — Data Center Tiers, Redundancy & Availability (Q50–58):** the Uptime Institute's Tier I–IV
classification and their availability/downtime figures · N / N+1 / 2N / 2N+1 redundancy notation ·
converting availability percentages to annual downtime and back · TIA-942 versus the Uptime Institute.

**Section 8 — Power & Cooling Infrastructure (Q59–67):** UPS versus generator+ATS roles across the outage
timeline · PDUs · CRAC/CRAH precision cooling · hot/cold aisle containment · raised floors as an air/cable
distribution plenum · the PUE formula, its ideal value, DCIE as its reciprocal, and worked PUE calculations.

**Section 9 — Data Center Storage Systems & Protocols (Q68–76):** DAS vs NAS vs SAN by access level and
shareability · iSCSI, Fibre Channel and FCoE as SAN transports · RAID levels 0/1/5/6/10 with worked usable-
capacity and fault-tolerance calculations.

**Section 10 — Security, Standards, Disaster Recovery & Modern Trends (Q77–90):** physical security layering
(mantraps, biometrics) · clean-agent fire suppression for electronics · RTO vs RPO and applying both
together · disaster recovery as a subset of business continuity · converged vs hyper-converged
infrastructure · the software-defined data center · edge data centers · closing questions that tie
virtualization's technical demands directly to the data-center design choices that support them.

## Available as reformatting

- `/mcq Data Center and Virtualization --hard` → trap-tier and harder applied questions only.
- `/mcq Data Center and Virtualization --practice` → all 90 questions first, explanations at the end.
