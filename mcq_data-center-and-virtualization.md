# Data Center and Virtualization — MCQ Question Bank
**Subject:** Computer Science / IT Infrastructure · **Target:** BCS Preliminary / Bank IT Officer / NTRCA / GATE / IBPS SO IT
**Questions:** 90 (Q1–Q90, across two files) · **Concepts covered:** 71/71 · **Complete in one run**

> How to use: attempt the question first, then read the explanation. The wrong
> options matter more than the right one — that's what the examiner is testing.

**This is the whole chapter.** Virtualization fundamentals and hypervisor types · the VM lifecycle
(snapshots, cloning, live migration, overcommitment, ballooning, provisioning, nested virtualization) ·
containers and OS-level virtualization · storage, network, desktop and application virtualization · cloud
service and deployment models · data center network architecture · tiering, redundancy and availability
math · power and cooling infrastructure · data center storage systems and protocols · and security,
standards, disaster recovery and modern converged/edge trends. Nothing is held back for a later batch.

**One deliberate cross-reference.** IP addressing, VLAN numbering and general LAN switching/routing
fundamentals are covered in this Bank's **[Data Communication and Networking](mcq_data-communication-and-networking.md)**
and **[Subnetting](mcq_subnetting.md)** chapters. This chapter treats networking only where it is
data-center-specific — spine-leaf/Clos fabrics, VXLAN's role in multi-tenant scale, and top-of-rack design —
without re-deriving OSI layers or subnet arithmetic.

**Where the marks are.** Virtualization is now a standard professional-knowledge topic in BCS, NTRCA and
bank IT sets (hypervisor types, benefits of virtualization, cloud service models) and appears in GATE
mainly through operating-systems-adjacent questions on virtual machines. Data center topics — tiers, RAID,
PUE, DAS/NAS/SAN — are drawn from bank IT officer and system-administration professional-knowledge banks,
where they are asked as applied/definitional recall rather than heavy calculation. Sections are ordered so
virtualization (the more universally examined half) comes first, then data center infrastructure, with the
numeric material (PUE, RAID capacity, availability percentages, RTO/RPO) placed once its vocabulary is
established.

**8 of the 90 questions carry an explicit worked calculation** (PUE/DCIE, RAID usable capacity, tier
availability/downtime conversion, VXLAN's ID-space math), and 41 more are `[Applied]` reasoning questions.
Every numeric figure — RAID capacities, PUE/DCIE relationships, Uptime Institute tier downtime figures,
VXLAN's 24-bit ID space — was independently recomputed or cross-checked against its defining formula before
being written.

---

## Section 1 — Virtualization Fundamentals & Hypervisor Types

**Q1.** What is virtualization? `[Core]`

- **A)** Creating a software-based version of a physical resource — compute, storage, or network — that
  behaves like the real thing
- **B)** Running two operating systems on two separate physical machines connected by a network
- **C)** Compressing data so it takes less physical storage space
- **D)** Replacing a mechanical hard disk with a solid-state drive

**Answer: A) Creating a software-based version of a physical resource — compute, storage, or network — that behaves like the real thing**

**Trace / Why:** virtualization inserts a software abstraction layer between physical hardware and the
systems that use it, so one physical resource can present as several logical ones (or several physical
resources can present as one). It applies equally to compute, storage and network resources.

**📘 CONCEPT — C1 · Virtualization is abstraction, not duplication or compression**
> Virtualization creates a logical resource that behaves like a physical one, backed by software rather
> than dedicated hardware. It is resource-type-agnostic: the same idea produces virtual machines (compute),
> virtual disks/pools (storage) and virtual LANs/switches (network).
>
> **Applies when** the stem asks for the core definition of virtualization, in any resource type.
>
> **Boundary:** virtualization is not emulation (C9) — virtualization typically presents the *same*
> underlying architecture in logical form; emulation reproduces a *different* architecture in software.

**Wrong traces:** B = physically separate machines on a network is the opposite of abstraction over shared
hardware · C = describes compression, an unrelated storage technique · D = a hardware swap, not an
abstraction layer.

---

**Q2.** The software layer that creates, runs and manages virtual machines by allocating physical CPU,
memory and I/O among them is called the: `[Core]`

- **A)** device driver
- **B)** BIOS/UEFI firmware
- **C)** hypervisor, or virtual machine monitor (VMM)
- **D)** kernel scheduler

**Answer: C) hypervisor, or virtual machine monitor (VMM)**

**Trace / Why:** the hypervisor sits between physical hardware and one or more guest virtual machines,
partitioning and scheduling CPU, memory and I/O so each VM believes it has dedicated hardware.

**📘 CONCEPT — C2 · Hypervisor (VMM) is the resource broker for whole VMs**
> The hypervisor's job is to multiplex physical hardware across independent virtual machines, each with
> its own guest OS, in isolation from the others.
>
> **Applies when** the stem asks who creates, runs or allocates resources to VMs specifically.
>
> **Boundary:** a kernel scheduler multiplexes *processes* within one OS instance; a hypervisor multiplexes
> *entire virtual machines*, each potentially running its own independent OS — a broader unit of isolation.

**Wrong traces:** A = mediates one specific hardware device for one OS, far narrower scope · B = firmware
that initializes hardware before any hypervisor or OS loads · D = manages processes/threads inside a single
OS, not whole VMs.

---

**Q3.** A Type 1 (bare-metal) hypervisor is distinguished from a Type 2 (hosted) hypervisor because: `[Core]`

- **A)** Type 1 runs only Linux guests, Type 2 only Windows guests
- **B)** Type 1 is always free of charge, Type 2 is always commercial
- **C)** Type 1 cannot run more than one virtual machine at a time
- **D)** Type 1 runs directly on the physical hardware, while Type 2 runs as an application on top of a
  conventional host operating system

**Answer: D) Type 1 runs directly on the physical hardware, while Type 2 runs as an application on top of a conventional host operating system**

**Trace / Why:** Type 1 hypervisors (VMware ESXi, Microsoft Hyper-V, Xen, KVM) are the first software layer
on the hardware; Type 2 hypervisors (VMware Workstation, Oracle VirtualBox, Parallels Desktop) install like
any other application inside an existing OS.

**📘 CONCEPT — C3 · Type 1 vs Type 2 hypervisors**
> | | Type 1 (bare-metal) | Type 2 (hosted) |
> |---|---|---|
> | Runs on | hardware directly | a host OS, as an app |
> | Examples | ESXi, Hyper-V, Xen, KVM | VMware Workstation, VirtualBox, Parallels |
> | Typical use | servers, data centers | desktops, development/test |
> | Overhead | lower — no host OS competing for resources | higher — shares host OS with other apps |
>
> **Applies when** the stem asks for the defining difference between the two hypervisor types, or asks
> which type a named product is.
>
> **Boundary:** the split is about *what the hypervisor runs on*, not guest OS support, licensing cost, or
> how many VMs it can host — all of which vary independently within each type.

**Wrong traces:** A = guest OS support is a product feature, not the type-defining criterion · B = both
types include free and commercial products (KVM/VirtualBox are free; ESXi/Workstation have paid tiers) ·
C = Type 1 hypervisors are built precisely to host many concurrent VMs.

---

**Q4.** Which pairing correctly matches each hypervisor to its type? `[Applied]`

- **A)** KVM — Type 2; Xen — Type 2
- **B)** VMware ESXi — Type 2; Oracle VirtualBox — Type 1
- **C)** Microsoft Hyper-V — Type 1; VMware Workstation — Type 2
- **D)** VirtualBox — Type 1; Hyper-V — Type 2

**Answer: C) Microsoft Hyper-V — Type 1; VMware Workstation — Type 2**

**Trace / Why:** Hyper-V installs as the first, bare-metal layer on Windows Server hardware (the "host OS"
you see afterward runs as a special management partition on top of it); VMware Workstation is a conventional
hosted application.

**📘 CONCEPT — C3 (see Concept Index)** › Hyper-V is the classic exam trap here, since Windows still appears
to boot normally — but Hyper-V's own kernel is what actually owns the hardware once enabled, with the
visible Windows instance running as a privileged guest partition.

**Wrong traces:** A = both KVM and Xen are bare-metal/Type-1-style hypervisors, not Type 2 · B = reversed —
ESXi is Type 1, VirtualBox is Type 2 · D = reversed — VirtualBox is Type 2, Hyper-V is Type 1.

---

**Q5.** In a Type 2 hypervisor deployment, the term "host OS" refers to: `[Core]`

- **A)** the operating system installed and running inside a virtual machine
- **B)** a backup image of the guest operating system
- **C)** the hypervisor's own web-based management console
- **D)** the conventional operating system on which the hypervisor itself is installed and runs as an
  application

**Answer: D) the conventional operating system on which the hypervisor itself is installed and runs as an application**

**Trace / Why:** "host" and "guest" describe layering: the host OS is what the hypervisor runs on top of;
the guest OS is what runs inside each VM the hypervisor creates.

**📘 CONCEPT — C4 · Host OS vs guest OS**
> The host OS underlies (Type 2) or is entirely absent from (Type 1, where the hypervisor itself is the
> lowest layer); the guest OS is whatever runs inside a given VM, independent of the host.
>
> **Applies when** a question uses "host" and "guest" and asks which is which.
>
> **Boundary:** a Type 1 hypervisor has no separate host OS underneath it at all — the hypervisor *is* the
> first software layer, which is precisely what "bare-metal" means (C3).

**Wrong traces:** A = that is the guest OS, not the host · B = unrelated to backup imaging · C = a
management tool, not the underlying operating system.

---

**Q6.** A Type 1 hypervisor: `[Trap]`

- **A)** always performs worse than a Type 2 hypervisor, because it lacks a host operating system's support
- **B)** requires a full general-purpose operating system to be installed underneath it first
- **C)** has no separate host operating system beneath it — it is itself the first software layer on the
  hardware
- **D)** is used only for desktop virtualization, never for servers

**Answer: C) has no separate host operating system beneath it — it is itself the first software layer on the hardware**

**Trace / Why:** "bare-metal" specifically means there is nothing — no general-purpose OS — between the
hypervisor and the physical hardware.

**📘 CONCEPT — C3 (see Concept Index)** › The trap here inverts the real performance relationship: Type 1
typically has *lower* overhead than Type 2, precisely because there is no host OS competing for CPU,
memory and I/O with the hypervisor.

**Wrong traces:** A = reverses the actual overhead relationship — Type 1 is generally more efficient ·
B = contradicts the bare-metal definition directly · D = Type 1 hypervisors (ESXi, Hyper-V, XenServer,
KVM) are the standard choice specifically for servers and data centers.

---

**Q7.** Full virtualization allows an unmodified guest operating system to run correctly because: `[Core]`

- **A)** the hypervisor uses binary translation to intercept and safely handle privileged instructions the
  guest OS issues
- **B)** the guest OS's kernel source code is edited to call the hypervisor directly
- **C)** the guest OS must be identical to the host OS
- **D)** the CPU physically prevents the guest from ever executing privileged instructions

**Answer: A) the hypervisor uses binary translation to intercept and safely handle privileged instructions the guest OS issues**

**Trace / Why:** full virtualization's defining trait is that the guest OS needs **no** modification — the
hypervisor transparently traps and translates the guest's privileged instructions so they execute safely
against virtualized, not real, hardware state.

**📘 CONCEPT — C5 · Full virtualization vs paravirtualization**
> | | Full virtualization | Paravirtualization |
> |---|---|---|
> | Guest OS | unmodified | modified to call the hypervisor directly |
> | Mechanism | binary translation of privileged instructions | explicit "hypercalls" |
> | Overhead source | trap-and-translate cost | requires custom/patched guest kernel |
>
> **Applies when** the stem contrasts whether the guest OS needed changes to run virtualized.
>
> **Boundary:** hardware-assisted virtualization (C6) can remove most of full virtualization's translation
> overhead without requiring paravirtualization's guest modifications — the two techniques for reducing
> overhead are independent of each other.

**Wrong traces:** B = describes paravirtualization, not full virtualization · C = guest and host OS can
differ completely, that is the point of virtualization · D = the CPU still executes the instructions —
the hypervisor intercepts and handles them, it doesn't simply block them.

---

**Q8.** Paravirtualization differs from full virtualization mainly because paravirtualization: `[Applied]`

- **A)** eliminates the need for any hypervisor
- **B)** requires the guest OS's kernel to be modified so it issues hypercalls directly to the hypervisor,
  avoiding the cost of trapping and translating privileged instructions
- **C)** only applies to storage virtualization, never to compute
- **D)** requires special guest hardware rather than any software changes

**Answer: B) requires the guest OS's kernel to be modified so it issues hypercalls directly to the hypervisor, avoiding the cost of trapping and translating privileged instructions**

**Trace / Why:** paravirtualization (the classic Xen model) trades guest-kernel portability for lower
overhead: because the guest already knows it is virtualized, it can ask the hypervisor for what it needs
instead of the hypervisor having to detect and translate.

**📘 CONCEPT — C5 (see Concept Index)** › The tradeoff is portability versus overhead: paravirtualized
guests need a hypervisor-aware kernel build, which is a real deployment constraint full virtualization
doesn't have.

**Wrong traces:** A = a hypervisor is still required, just called differently · C = it is a compute
(guest-OS) virtualization technique, not a storage one · D = the change is to guest *software* (the
kernel), not hardware.

---

**Q9.** Intel VT-x and AMD-V are: `[Core]`

- **A)** RAID controller firmware standards
- **B)** disk encryption algorithms
- **C)** network virtualization protocols
- **D)** CPU instruction-set extensions that add hardware-assisted virtualization support

**Answer: D) CPU instruction-set extensions that add hardware-assisted virtualization support**

**Trace / Why:** both are vendor-specific CPU extensions that let privileged guest instructions trap
directly into hardware, so the hypervisor no longer needs pure software binary translation to intercept
them.

**📘 CONCEPT — C6 · Hardware-assisted virtualization**
> VT-x (Intel) and AMD-V (AMD) add a CPU privilege mode specifically for hypervisors, letting sensitive
> guest instructions trap to the hypervisor in hardware rather than being caught and rewritten in software.
>
> **Applies when** the stem names VT-x/AMD-V or asks how modern CPUs reduce virtualization overhead.
>
> **Boundary:** this reduces *translation* overhead for full virtualization; it does not add memory or
> storage capacity, and it does not make paravirtualization obsolete — paravirtual I/O drivers are still
> used with hardware-assisted CPUs for further efficiency.

**Wrong traces:** A = unrelated to storage controllers · B = an unrelated security feature · C = these are
CPU extensions, not networking technology.

---

**Q10.** Emulation differs from virtualization in that emulation: `[Trap]`

- **A)** requires no software at all
- **B)** applies only to storage systems, never to compute
- **C)** always runs faster than virtualization because it accesses hardware more directly
- **D)** reproduces a completely different CPU architecture in software — e.g., running ARM binaries on an
  x86 host — which is inherently slower than virtualizing the same architecture

**Answer: D) reproduces a completely different CPU architecture in software — e.g., running ARM binaries on an x86 host — which is inherently slower than virtualizing the same architecture**

**Trace / Why:** an emulator must translate every instruction from one instruction set to another, which
costs far more than virtualization's job of multiplexing hardware that already matches the guest's
architecture.

**📘 CONCEPT — C7 · Emulation vs virtualization**
> Virtualization presents the **same** architecture as several logical copies (near-native speed).
> Emulation reproduces a **different** architecture entirely in software (much slower, but supports guest
> binaries that could never run on the host CPU natively).
>
> **Applies when** the stem mentions running software built for a different CPU architecture than the host.
>
> **Boundary:** the split is architecture match, not resource type — you can emulate compute (a CPU) just
> as you can virtualize compute; the difference is whether the underlying instruction set is preserved.

**Wrong traces:** A = emulation is entirely software-implemented, not hardware-only · B = emulation applies
to whole-system/CPU reproduction generally, not narrowly storage · C = emulation is slower than
virtualization precisely because of per-instruction translation across architectures.

---

## Section 2 — Hypervisor & VM Lifecycle Management

**Q11.** A virtual machine **snapshot** is best described as: `[Core]`

- **A)** a fully independent copy of a VM that can be run separately from the original
- **B)** a point-in-time capture of a VM's disk and memory state that the VM can be reverted to, without
  producing an independent, separately runnable copy
- **C)** a scheduled backup written to physical tape
- **D)** a compressed version of the VM's installer files

**Answer: B) a point-in-time capture of a VM's disk and memory state that the VM can be reverted to, without producing an independent, separately runnable copy**

**Trace / Why:** a snapshot records delta changes against the parent disk so the VM can roll back to that
point; it stays tied to the original VM's disk chain rather than becoming a standalone machine.

**📘 CONCEPT — C8 · Snapshot vs clone**
> | | Snapshot | Clone |
> |---|---|---|
> | Independence | tied to the original VM's disk chain | fully independent VM |
> | Purpose | quick rollback point | duplication for separate, parallel use |
> | Runs standalone? | no | yes |
>
> **Applies when** the stem asks for a rollback point versus a duplicate, separately usable machine.
>
> **Boundary:** long snapshot chains grow the backing disk and degrade performance — snapshots are a
> short-lived safety net, not a long-term versioning or backup strategy.

**Wrong traces:** A = describes a clone, not a snapshot · C = unrelated to tape backup scheduling ·
D = unrelated to installer compression.

---

**Q12.** A VM **clone**, unlike a snapshot: `[Core]`

- **A)** can only be created while the source VM is powered off and never again afterward
- **B)** shares the exact same virtual disk file as the source, with no duplication
- **C)** is a separate, independently runnable copy of a VM, with its own disk and identity
- **D)** automatically deletes the source VM once created

**Answer: C) is a separate, independently runnable copy of a VM, with its own disk and identity**

**Trace / Why:** cloning duplicates the VM's disk (and configuration) into a new, independent VM that can
run concurrently with, and entirely separately from, the original.

**📘 CONCEPT — C8 (see Concept Index)** › Cloning is used for rapid deployment of identical VMs (e.g., a
template for a pool of application servers); snapshotting is used for rollback safety around a risky change
on one specific VM.

**Wrong traces:** A = clones can typically be made from a running VM too, depending on the platform ·
B = a clone gets its own disk copy, that is what makes it independent · D = the source VM is untouched by
cloning.

---

**Q13.** **Live migration** (e.g., VMware vMotion, Hyper-V Live Migration) moves a running VM between
physical hosts with minimal or no downtime. This generally requires: `[Applied]`

- **A)** powering off the VM for the duration of the transfer
- **B)** the VM to have no network interface at all during the move
- **C)** both hosts to run entirely different hypervisor vendors
- **D)** shared storage reachable by both hosts, network connectivity between them, and broadly compatible
  CPU features

**Answer: D) shared storage reachable by both hosts, network connectivity between them, and broadly compatible CPU features**

**Trace / Why:** without shared storage the VM's disk would have to be copied wholesale; without compatible
CPU features, a running VM's in-flight instructions could reference CPU capabilities the destination host
lacks — both break the "live," no-downtime property.

**📘 CONCEPT — C9 · Live migration preconditions**
> Live migration copies a VM's live memory state to a new host while it keeps running, then briefly
> switches execution over. It depends on: (1) storage both hosts can already see, (2) sufficient network
> bandwidth to transfer memory state before it changes too much, and (3) CPU compatibility so mid-flight
> instructions remain valid.
>
> **Applies when** the stem asks what live migration needs, or why a migration between mismatched hosts
> fails.
>
> **Boundary:** migrating a **powered-off** VM has none of these live constraints — it is just a file copy,
> which is a different (and much simpler) operation than live migration.

**Wrong traces:** A = powering off defeats the definition of *live* migration · B = the VM keeps running
with its network interface active throughout · C = live migration generally requires hosts on the *same*
hypervisor platform (cross-vendor migration is the unusual, specially-tooled case).

---

**Q14.** **Memory ballooning** is a hypervisor technique used to: `[Applied]`

- **A)** physically add more RAM modules to a running server
- **B)** compress a VM's virtual disk file
- **C)** reclaim unused memory from a guest VM, by having an in-guest driver "inflate" and pressure the
  guest OS into releasing memory pages back to the hypervisor
- **D)** permanently delete idle virtual machines

**Answer: C) reclaim unused memory from a guest VM, by having an in-guest driver "inflate" and pressure the guest OS into releasing memory pages back to the hypervisor**

**Trace / Why:** the balloon driver, installed inside the guest, claims memory on the hypervisor's behalf;
the guest OS's own memory manager then reclaims/pages out its least-needed pages to satisfy the balloon's
demand, and the hypervisor reassigns the freed physical memory elsewhere.

**📘 CONCEPT — C10 · Memory overcommitment and ballooning**
> Hypervisors often let VMs' *configured* memory exceed physical RAM (overcommitment), betting that not all
> VMs need their full allocation simultaneously. Ballooning is the polite way to reclaim memory from a VM
> that is currently over-provisioned relative to its actual need, using the guest's own paging logic rather
> than the hypervisor guessing from outside.
>
> **Applies when** the stem describes an in-guest driver reclaiming memory, or asks how overcommitment is
> managed safely.
>
> **Boundary:** ballooning cooperates with the guest OS; it differs from hypervisor-level swapping, which
> reclaims memory without the guest's cooperation and is much more disruptive to performance.

**Wrong traces:** A = a physical hardware action, unrelated to a software technique · B = disk compression
is a separate concern from memory reclamation · D = ballooning reclaims memory from running VMs, it does
not delete them.

---

**Q15.** **Thin provisioning** of a virtual disk means: `[Core]`

- **A)** the full disk size is reserved and consumed on the physical storage immediately at creation
- **B)** the VM cannot write any data until the administrator manually provisions space
- **C)** the virtual disk can never be resized after creation
- **D)** storage is allocated on the physical backing store only as data is actually written, even though the
  VM sees the full configured size

**Answer: D) storage is allocated on the physical backing store only as data is actually written, even though the VM sees the full configured size**

**Trace / Why:** thin provisioning defers physical allocation until the VM actually writes data, letting
several thin disks be over-subscribed against physical capacity that assumes they won't all fill up at once.

**📘 CONCEPT — C11 · Thin vs thick provisioning**
> | | Thin provisioning | Thick provisioning |
> |---|---|---|
> | Physical space at creation | minimal, grows with use | fully reserved upfront |
> | Storage efficiency | higher (until disks fill) | lower, but predictable |
> | Risk | backing store can run out if over-subscribed | none — space is guaranteed |
>
> **Applies when** the stem contrasts "allocated on demand" versus "reserved fully upfront" storage.
>
> **Boundary:** thin provisioning's efficiency comes with a real risk — if too many thin disks are
> over-subscribed and all grow simultaneously, the physical datastore can run out of space even though
> individual VMs still show free space.

**Wrong traces:** A = describes thick provisioning, the opposite approach · B = the VM can write
immediately; thin provisioning only defers *physical* allocation, not permission to write · C =
provisioning type does not by itself prevent resizing.

---

**Q16.** **P2V (physical-to-virtual) conversion** refers to: `[Core]`

- **A)** converting an existing physical server's OS, applications and data into a virtual machine image
- **B)** converting a VM back into a bare physical server
- **C)** upgrading a physical server's firmware
- **D)** migrating a VM from one hypervisor vendor's format to another

**Answer: A) converting an existing physical server's OS, applications and data into a virtual machine image**

**Trace / Why:** P2V tools capture a running physical machine's disk image and repackage it as a VM disk
that a hypervisor can boot, typically as the first step of a server-consolidation project.

**📘 CONCEPT — C12 · P2V direction**
> P2V moves **physical → virtual**. The reverse direction (virtual → physical) is V2P, used far less often
> and mainly for performance-critical or licensing-constrained workloads that must run on bare metal.
>
> **Applies when** the stem asks about migrating an *existing* physical server into a virtualized
> environment.
>
> **Boundary:** don't confuse P2V with cross-hypervisor VM format conversion (e.g., VMDK to VHD) — that
> moves between two *already-virtual* formats, not from physical hardware.

**Wrong traces:** B = describes V2P, the reverse direction · C = unrelated to firmware · D = a
virtual-to-virtual format conversion, not P2V.

---

**Q17.** **Nested virtualization** means: `[Applied]`

- **A)** running a hypervisor inside a virtual machine that itself runs on another hypervisor
- **B)** running two unrelated VMs side by side on the same host
- **C)** stacking physical servers in a single rack
- **D)** a VM with two virtual CPUs instead of one

**Answer: A) running a hypervisor inside a virtual machine that itself runs on another hypervisor**

**Trace / Why:** nested virtualization exposes virtualization extensions (or emulates them) inside a guest
VM so that guest can itself act as a hypervisor and run its own child VMs — useful for testing
virtualization software itself, or running one vendor's hypervisor lab environment inside another's cloud VM.

**📘 CONCEPT — C13 · Nested virtualization**
> A VM normally just runs an OS; nested virtualization lets that guest OS *itself* be a hypervisor hosting
> further VMs, adding a layer of indirection (and CPU-instruction-trapping overhead) at each level.
>
> **Applies when** the stem describes "a hypervisor running inside a VM" or "VMs inside VMs."
>
> **Boundary:** ordinary VMs running side by side on one physical host (sibling VMs) are not nested — nesting
> specifically requires one VM to itself host further VMs.

**Wrong traces:** B = sibling VMs on one host, not nesting · C = physical rack density, unrelated to
virtualization layering · D = describes multi-vCPU allocation, an unrelated VM configuration setting.

---

**Q18.** In a virtualized cluster, **high availability (HA)** for VMs primarily means: `[Applied]`

- **A)** every VM is manually restarted by an administrator after any host failure
- **B)** VMs are permanently duplicated in real time with zero interruption on any failure
- **C)** if a physical host fails, the VMs it was running are automatically restarted on a surviving host in
  the cluster
- **D)** a single host is prevented from ever failing

**Answer: C) if a physical host fails, the VMs it was running are automatically restarted on a surviving host in the cluster**

**Trace / Why:** HA clustering monitors host health and, on failure, automatically powers the affected VMs
back on elsewhere in the cluster — accepting a brief restart (and loss of in-memory state), unlike fault
tolerance.

**📘 CONCEPT — C14 · High availability vs fault tolerance**
> HA restarts a VM elsewhere after a failure is detected — there is a short outage while the VM boots on the
> new host. **Fault tolerance (FT)** keeps a live, lockstep-synchronized secondary VM running continuously,
> so failover is instantaneous with no state loss, at a much higher resource cost.
>
> **Applies when** the stem describes automatic recovery *after* a failure (HA) versus continuous,
> zero-interruption duplication (FT).
>
> **Boundary:** HA does not prevent failures or guarantee zero downtime — it minimizes downtime by
> automating recovery, which is a materially weaker guarantee than fault tolerance.

**Wrong traces:** A = manual restart is what HA specifically automates away · B = describes fault tolerance,
a stronger and costlier guarantee · D = no clustering technology prevents hardware failure itself.

---

**Q19.** **VM sprawl** refers to: `[Trap]`

- **A)** a single VM configured with too many virtual CPUs
- **B)** the deliberate, planned scaling out of an application across many VMs
- **C)** a VM that has outgrown its allocated disk size
- **D)** the uncontrolled, poorly tracked proliferation of virtual machines across an environment, wasting
  compute, storage and licensing resources

**Answer: D) the uncontrolled, poorly tracked proliferation of virtual machines across an environment, wasting compute, storage and licensing resources**

**Trace / Why:** because virtual machines are so cheap to create compared to physical servers, environments
without governance accumulate forgotten, idle or duplicate VMs that still consume resources and licenses —
this is VM sprawl, a management/governance failure rather than a technical one.

**📘 CONCEPT — C15 · VM sprawl is a governance problem**
> Virtualization's ease of provisioning is also its risk: without lifecycle tracking (who owns a VM, when it
> expires), the population of VMs grows unmanaged.
>
> **Applies when** the stem describes "too many forgotten/idle VMs" or resource waste from unmanaged
> virtualization.
>
> **Boundary:** deliberate horizontal scale-out (many VMs on purpose, to handle load) is the *opposite* of
> sprawl — the distinguishing factor is whether the VMs are tracked and justified, not the raw count.

**Wrong traces:** A = a per-VM sizing issue, not a population-wide sprawl problem · B = intentional,
governed scaling is not sprawl by definition · C = a capacity issue on one VM, unrelated to sprawl.

---

## Section 3 — Containers & OS-Level Virtualization

**Q20.** **OS-level virtualization** (as used by containers) differs from hardware-level virtualization
(VMs) because: `[Core]`

- **A)** it is identical to Type 1 hypervisor virtualization
- **B)** it can run only one container at a time per host
- **C)** all containers on a host share one underlying OS kernel, instead of each running its own separate
  guest OS
- **D)** it requires dedicated hardware per container

**Answer: C) all containers on a host share one underlying OS kernel, instead of each running its own separate guest OS**

**Trace / Why:** a container packages an application with its libraries and dependencies but relies on the
host's single kernel for process isolation (namespaces) and resource limiting (cgroups), rather than
booting an independent guest kernel per instance.

**📘 CONCEPT — C16 · Containers vs virtual machines**
> | | VM | Container |
> |---|---|---|
> | Isolation unit | full guest OS + kernel | process, isolated by kernel namespaces/cgroups |
> | Kernel | one per VM | shared host kernel |
> | Startup time | minutes (boots an OS) | seconds (starts a process) |
> | Overhead | higher (duplicate OS per VM) | lower (no duplicate OS) |
>
> **Applies when** the stem contrasts container and VM isolation, overhead, or startup speed.
>
> **Boundary:** sharing one kernel means a container is *not* isolated from a kernel-level vulnerability the
> way a VM is — a VM's isolation boundary is stronger because each guest has its own kernel.

**Wrong traces:** A = fundamentally different — no hypervisor or per-guest kernel is involved ·
B = a single host commonly runs many containers concurrently, that's the point of the lightweight model ·
D = no dedicated hardware is needed per container, that would defeat the lightweight sharing model.

---

**Q21.** A Docker **image**, as distinct from a Docker **container**, is: `[Core]`

- **A)** a running instance with its own writable filesystem layer and process
- **B)** a read-only template — filesystem plus metadata — from which one or more containers are launched
- **C)** the same thing, just two names for one object
- **D)** a physical disk image of an entire virtual machine

**Answer: B) a read-only template — filesystem plus metadata — from which one or more containers are launched**

**Trace / Why:** the image is the static blueprint (layers of filesystem changes plus configuration); a
container is a running instance created from that image, with its own thin writable layer on top.

**📘 CONCEPT — C17 · Image vs container**
> An image is to a container roughly what a class is to an object: one image can be instantiated into many
> running containers, each independent at runtime but sharing the same read-only image layers underneath.
>
> **Applies when** the stem distinguishes a static template from a running instance in container terminology.
>
> **Boundary:** a Docker image is unrelated in scope to a full VM disk image — an image here is
> application-and-dependency-level, not a whole-OS disk capture.

**Wrong traces:** A = describes a container, not an image · C = they are distinct — template versus running
instance · D = confuses container images with VM-level disk images, a much larger and different artifact.

---

**Q22.** The Linux kernel mechanisms that underpin container isolation and resource limiting are: `[Applied]`

- **A)** VLANs and subnetting
- **B)** BIOS and UEFI
- **C)** RAID and LVM
- **D)** namespaces (isolating what a process can see) and cgroups (limiting what resources it can consume)

**Answer: D) namespaces (isolating what a process can see) and cgroups (limiting what resources it can consume)**

**Trace / Why:** namespaces give each container its own view of process IDs, network interfaces, mount
points and hostnames; cgroups (control groups) cap and account for CPU, memory and I/O each container's
processes may use — together these two kernel features are what makes containers isolated without a
separate kernel per instance.

**📘 CONCEPT — C18 · Namespaces + cgroups = container isolation**
> Namespaces answer "what can this process see?" (isolation); cgroups answer "how much can it use?"
> (resource limiting). A container is, at the kernel level, just a set of ordinary processes wrapped in
> both.
>
> **Applies when** the stem asks what Linux feature actually implements container isolation, underneath
> tools like Docker.
>
> **Boundary:** neither mechanism creates a separate kernel — that structural fact is exactly why
> containers boot faster than VMs but share more fate with the host kernel than a VM does.

**Wrong traces:** A = network addressing concepts, unrelated to process-level isolation · B = firmware,
unrelated to OS-level isolation · C = storage management technologies, unrelated to process isolation.

---

**Q23.** Compared to virtual machines, containers are generally preferred when: `[Applied]`

- **A)** the workload must run a different kernel or OS family than the host
- **B)** the strongest possible isolation between tenants is required, even at higher resource cost
- **C)** fast startup, high density and efficient resource use matter more than running a different kernel
  from the host
- **D)** the application must run directly on bare metal with no abstraction at all

**Answer: C) fast startup, high density and efficient resource use matter more than running a different kernel from the host**

**Trace / Why:** containers trade the stronger, kernel-level isolation of VMs for much lower overhead and
near-instant startup, which suits microservices and horizontally-scaled stateless workloads especially well.

**📘 CONCEPT — C16 (see Concept Index)** › The real-world choice is rarely absolute — many production
systems run containers *inside* VMs, using VMs for strong tenant isolation at the host level and containers
inside each VM for density and fast deployment of application instances.

**Wrong traces:** A = a container needs the host's kernel, so a different kernel/OS family requires a VM,
not a container · B = VMs give stronger isolation; containers are chosen despite, not because of, weaker
isolation · D = both containers and VMs are abstractions — "no abstraction" describes bare-metal
installation, neither option.

---

**Q24.** **Container orchestration** platforms such as Kubernetes primarily exist to: `[Applied]`

- **A)** replace the need for container images entirely
- **B)** automate deployment, scaling, networking and failure-recovery of many containers across a cluster
  of hosts
- **C)** provide a Type 1 hypervisor for running virtual machines
- **D)** encrypt data at rest inside a single container

**Answer: B) automate deployment, scaling, networking and failure-recovery of many containers across a cluster of hosts**

**Trace / Why:** as container counts grow into the hundreds or thousands across many hosts, an
orchestrator schedules where each container runs, restarts failed ones, load-balances traffic to them, and
scales their count up or down automatically.

**📘 CONCEPT — C19 · Why orchestration is needed at scale**
> A single container is easy to manage by hand; hundreds spread across many hosts are not. Orchestration
> automates the scheduling and lifecycle decisions that would otherwise require constant manual intervention.
>
> **Applies when** the stem names Kubernetes/orchestration and asks what problem it solves.
>
> **Boundary:** orchestration operates *above* individual containers — it does not replace the container
> runtime or the images those containers are built from, it manages them at fleet scale.

**Wrong traces:** A = images remain the deployable unit; orchestration schedules them, it doesn't replace
them · C = orchestrators manage containers, not VMs, and are not hypervisors · D = encryption is a separate
security concern, not orchestration's purpose.

---

**Q25.** A **microservice** architecture, commonly deployed via containers, is best described as: `[Core]`

- **A)** one large, single-process application containing all functionality
- **B)** an application decomposed into small, independently deployable services that communicate over the
  network
- **C)** a virtual machine running multiple unrelated operating systems simultaneously
- **D)** a hypervisor feature for consolidating hardware

**Answer: B) an application decomposed into small, independently deployable services that communicate over the network**

**Trace / Why:** microservices split what would otherwise be one monolithic application into many small,
independently deployable and scalable services — a natural fit for containers' fast startup and per-service
resource isolation.

**📘 CONCEPT — C20 · Microservices and containers pair naturally**
> Containers make it cheap to package and independently deploy each small service, and orchestration makes
> it practical to run and scale the resulting large number of services — the architectural style and the
> packaging technology reinforce each other, though neither strictly requires the other.
>
> **Applies when** the stem describes decomposing one application into many independently deployed pieces.
>
> **Boundary:** the opposite style — one large, tightly-coupled process — is called a **monolith**;
> microservices are a design choice about application structure, not a virtualization technology in
> themselves.

**Wrong traces:** A = describes a monolith, the opposite architectural style · C = describes multi-OS VM
usage, unrelated to application architecture · D = a hardware-consolidation benefit of virtualization, not
an application architecture.

---

## Section 4 — Storage, Network, Desktop & Application Virtualization

**Q26.** **Storage virtualization** refers to: `[Core]`

- **A)** compressing files to save disk space
- **B)** running a database inside a virtual machine
- **C)** encrypting data before it is written to disk
- **D)** pooling physical storage from one or more devices into a single logical storage resource, managed
  independently of the underlying physical hardware

**Answer: D) pooling physical storage from one or more devices into a single logical storage resource, managed independently of the underlying physical hardware**

**Trace / Why:** storage virtualization abstracts physical disks/arrays behind a logical layer, so capacity
can be provisioned, migrated or resized without the consuming system needing to know which physical devices
actually hold the data.

**📘 CONCEPT — C21 · Storage virtualization is pooling + abstraction**
> The consumer sees logical volumes; the storage layer decides which physical disks actually back them,
> and can move data between physical devices transparently (e.g., for tiering or maintenance).
>
> **Applies when** the stem describes storage being presented independently of its physical backing.
>
> **Boundary:** this is a different concept from RAID (C-numbers in Section 9) — RAID is one specific
> technique *within* storage systems for redundancy/performance; storage virtualization is the broader
> abstraction layer that can sit above RAID arrays, SANs, or mixed hardware.

**Wrong traces:** A = compression is unrelated to pooling/abstraction · B = running a workload inside a VM
is compute virtualization, not storage virtualization · C = encryption is a security feature, not
virtualization.

---

**Q27.** **Network virtualization** — for example, VXLAN or software-defined virtual switches — primarily
allows: `[Applied]`

- **A)** physical cables to be eliminated entirely from a data center
- **B)** logically isolated virtual networks to be created and moved independently of the physical network
  topology underneath them
- **C)** all traffic to bypass any switch or router
- **D)** IP addresses to be assigned without any addressing scheme

**Answer: B) logically isolated virtual networks to be created and moved independently of the physical network topology underneath them**

**Trace / Why:** network virtualization decouples logical network segments (and their policies) from the
physical wiring and switches, so virtual networks can be created, torn down or migrated (following VMs) in
software.

**📘 CONCEPT — C22 · Network virtualization decouples logical from physical topology**
> A virtual network's logical structure is defined in software and encapsulated over the physical network
> — this is what lets a VM's network identity follow it during live migration to a different physical
> host/rack.
>
> **Applies when** the stem describes virtual/logical networks independent of physical wiring, especially
> in the context of moving VMs.
>
> **Boundary:** VXLAN specifically (C23) is one mechanism for this; it solves a scale limit that plain
> VLANs hit in large multi-tenant environments.

**Wrong traces:** A = physical infrastructure still exists underneath; virtualization is a logical layer on
top of it, not a replacement for cabling · C = traffic still traverses physical switches/routers, just
under virtualized logical policy · D = addressing schemes are still required; virtualization changes how
segments are isolated, not whether addressing exists.

---

**Q28.** **VXLAN** was introduced mainly to solve which limitation of traditional VLANs in large,
multi-tenant data centers? `[Trap]`

- **A)** VLANs cannot carry any Ethernet traffic at Layer 2
- **B)** VLANs require Fibre Channel hardware
- **C)** the 12-bit VLAN ID field allows only 4,094 usable segments, too few for large-scale multi-tenant
  isolation
- **D)** VLANs cannot be configured on any modern switch

**Answer: C) the 12-bit VLAN ID field allows only 4,094 usable segments, too few for large-scale multi-tenant isolation**

**Trace / Why:** a standard 802.1Q VLAN tag has a 12-bit ID field (4,096 values, with 0 and 4095 reserved,
leaving 4,094 usable), which is easily exhausted in a cloud provider hosting many thousands of tenants.
VXLAN's 24-bit VNI supports over 16 million logical segments instead.

```
VLAN:  12-bit ID  → 2^12 = 4,096 total, 4,094 usable
VXLAN: 24-bit VNI → 2^24 = 16,777,216 total segments
```

**📘 CONCEPT — C23 · VXLAN's scale advantage over VLANs**
> VXLAN encapsulates Ethernet frames inside UDP/IP packets, tagging each with a 24-bit VXLAN Network
> Identifier (VNI) instead of a 12-bit VLAN ID — this both raises the segment ceiling by orders of magnitude
> and lets Layer 2 segments span routed (Layer 3) infrastructure between data centers or racks.
>
> **Applies when** the stem asks why VXLAN exists, or names its ID field size.
>
> **Boundary:** VXLAN is not a security or performance feature by itself — it is a scaling and
> Layer-2-over-Layer-3 extension mechanism; the numeric ceiling (4,094 vs 16 million) is exactly what
> distinguishes the two.

**Wrong traces:** A = VLANs do carry Ethernet traffic; the issue is the ID-space ceiling, not capability ·
B = VLANs are an Ethernet/802.1Q feature, unrelated to Fibre Channel · D = VLANs are widely supported;
the limitation is scale of the ID space, not configurability.

---

**Q29.** **Desktop virtualization (VDI — Virtual Desktop Infrastructure)** means: `[Core]`

- **A)** installing a second physical monitor on a desktop
- **B)** running two applications side by side on one desktop OS
- **C)** replacing a desktop's hard drive with a virtual one that has no physical backing at all
- **D)** hosting a user's desktop operating system as a VM in a central data center, accessed remotely from
  a thin client or any endpoint device

**Answer: D) hosting a user's desktop operating system as a VM in a central data center, accessed remotely from a thin client or any endpoint device**

**Trace / Why:** VDI centralizes desktop OS instances as VMs on data-center hardware; the endpoint device
just displays the remote session, which simplifies patching, backup and security management for large
user populations.

**📘 CONCEPT — C24 · VDI centralizes desktop management**
> Because the actual desktop OS runs centrally as a VM, IT can patch, back up and secure it uniformly, and
> users can access their same desktop from different physical endpoints.
>
> **Applies when** the stem describes a centrally hosted, remotely accessed desktop OS.
>
> **Boundary:** VDI is compute virtualization (a full guest desktop OS as a VM) applied to end-user
> desktops specifically — it is not the same as application virtualization (C25), which streams or
> isolates a single app without virtualizing the whole desktop OS.

**Wrong traces:** A = a physical hardware add-on, unrelated to virtualization · B = describes ordinary
multitasking, not virtualization · C = a virtual disk still has physical backing somewhere in the data
center's storage.

---

**Q30.** **Application virtualization** differs from desktop virtualization (VDI) in that application
virtualization: `[Applied]`

- **A)** isolates and delivers a single application (with its dependencies) to run on an endpoint's existing
  OS, without virtualizing the entire desktop operating system
- **B)** requires an entire guest OS to be provisioned per application
- **C)** is only usable for antivirus software
- **D)** is a synonym for desktop virtualization

**Answer: A) isolates and delivers a single application (with its dependencies) to run on an endpoint's existing OS, without virtualizing the entire desktop operating system**

**Trace / Why:** application virtualization packages just the app and what it needs, streaming or
sandbox-running it on top of whatever OS the endpoint already has — a lighter-weight alternative to
virtualizing the whole desktop when only specific applications need isolation or centralized management.

**📘 CONCEPT — C25 · Application virtualization is narrower than VDI**
> VDI virtualizes the whole desktop OS; application virtualization virtualizes just one application's
> runtime environment on top of an existing, un-virtualized OS.
>
> **Applies when** the stem describes isolating or streaming a single application rather than a whole
> desktop.
>
> **Boundary:** the two are complementary, not exclusive — a VDI-hosted desktop can itself also run
> virtualized applications inside it.

**Wrong traces:** B = that would be VDI, not application virtualization — the whole point here is *no*
per-app guest OS · C = it applies broadly to any application needing isolated/portable delivery, not just
antivirus · D = they are distinct techniques operating at different scopes (whole desktop vs single app).

---

## Section 5 — Cloud Computing Service & Deployment Models

**Q31.** In the cloud service model **IaaS (Infrastructure as a Service)**, the cloud provider is responsible
for: `[Core]`

- **A)** the customer's application code only
- **B)** the physical hardware, virtualization layer, and providing raw compute, storage and network
  resources (typically as VMs) that the customer then configures with their own OS and applications
- **C)** nothing — IaaS is a purely on-premises model
- **D)** the customer's entire finished application, with no infrastructure exposed at all

**Answer: B) the physical hardware, virtualization layer, and providing raw compute, storage and network resources (typically as VMs) that the customer then configures with their own OS and applications**

**Trace / Why:** IaaS (e.g., AWS EC2, Azure VMs) hands the customer virtualized infrastructure — they still
install and manage the guest OS, middleware and application themselves.

**📘 CONCEPT — C26 · IaaS / PaaS / SaaS — who manages what**
> | Layer | IaaS | PaaS | SaaS |
> |---|---|---|---|
> | App code | customer | customer | provider |
> | Runtime/middleware | customer | provider | provider |
> | OS | customer | provider | provider |
> | Virtualization/hardware | provider | provider | provider |
>
> **Applies when** the stem asks which party manages the OS, runtime, or hardware in a named cloud model.
>
> **Boundary:** the boundary moves upward from IaaS to SaaS — customer responsibility strictly decreases,
> and provider responsibility strictly increases, as you go IaaS → PaaS → SaaS.

**Wrong traces:** A = describes a much higher abstraction than IaaS actually provides · C = IaaS is
specifically a cloud model, not on-premises · D = describes SaaS, the opposite end of the spectrum.

---

**Q32.** **PaaS (Platform as a Service)** is best distinguished from IaaS by: `[Applied]`

- **A)** PaaS gives the customer a ready-made runtime/platform (e.g., a managed application server and
  database) to deploy code onto, without the customer managing the underlying OS
- **B)** PaaS never involves any virtualization
- **C)** PaaS provides only raw virtual machines with no operating system installed
- **D)** PaaS is identical to SaaS

**Answer: A) PaaS gives the customer a ready-made runtime/platform (e.g., a managed application server and database) to deploy code onto, without the customer managing the underlying OS**

**Trace / Why:** PaaS (e.g., Heroku, Google App Engine) moves the OS and runtime management to the provider,
leaving the customer to focus only on their application code and configuration.

**📘 CONCEPT — C26 (see Concept Index)** › The exam-common trap is confusing PaaS with SaaS: PaaS customers
still write and deploy their *own* application; SaaS customers just use a finished application someone else
wrote.

**Wrong traces:** B = PaaS still runs on a virtualized (or containerized) infrastructure underneath, just
hidden from the customer · C = describes IaaS, not PaaS · D = they differ — SaaS delivers a finished
application, PaaS delivers a platform to build on.

---

**Q33.** **SaaS (Software as a Service)** — such as a web-based email or CRM system — means the customer:
`[Core]`

- **A)** manages the underlying virtual machines themselves
- **B)** installs and patches the operating system
- **C)** writes and deploys their own custom backend code onto the platform
- **D)** simply uses a fully finished application over the network, with the provider managing everything
  beneath it

**Answer: D) simply uses a fully finished application over the network, with the provider managing everything beneath it**

**Trace / Why:** SaaS is the highest abstraction level — the customer only interacts with the finished
application (typically via browser), with infrastructure, OS, runtime and application code all managed by
the provider.

**📘 CONCEPT — C26 (see Concept Index)** › SaaS sits at the top of the stack in the IaaS/PaaS/SaaS table —
customer responsibility is at its minimum here.

**Wrong traces:** A = infrastructure management is entirely hidden from a SaaS customer · B = the OS is
managed by the provider in SaaS · C = writing custom backend code describes PaaS use, not SaaS.

---

**Q34.** A **public cloud** deployment model means: `[Core]`

- **A)** infrastructure and services owned by a third-party provider and shared across many customers over
  the public internet
- **B)** infrastructure dedicated entirely to a single organization, hosted on that organization's own
  premises
- **C)** a network with no internet connectivity at all
- **D)** a cloud usable only by government agencies

**Answer: A) infrastructure and services owned by a third-party provider and shared across many customers over the public internet**

**Trace / Why:** public cloud (AWS, Azure, GCP) is multi-tenant by design — many customers share the same
underlying physical infrastructure, logically isolated from one another, with the provider owning and
operating everything.

**📘 CONCEPT — C27 · Cloud deployment models**
> | Model | Ownership | Tenancy |
> |---|---|---|
> | Public | third-party provider | shared across many customers |
> | Private | one organization (or dedicated for it) | single-tenant |
> | Hybrid | mixed | both, integrated together |
> | Community | shared group of organizations | shared among that group only |
>
> **Applies when** the stem asks who owns the infrastructure and how many organizations share it.
>
> **Boundary:** "public" describes shared multi-tenant ownership, not literally unrestricted/insecure
> access — public cloud tenants are still logically isolated from one another.

**Wrong traces:** B = describes a private cloud, the opposite ownership/tenancy model · C = public cloud is
specifically internet-delivered · D = government-only clouds are a private/community cloud use case, not
what "public" means here.

---

**Q35.** A **hybrid cloud** is: `[Applied]`

- **A)** a public cloud that has been renamed
- **B)** two unrelated private clouds with no connection between them
- **C)** a combination of private and public cloud infrastructure, integrated so that workloads or data can
  move between the two as needed
- **D)** a cloud provider that only offers SaaS applications

**Answer: C) a combination of private and public cloud infrastructure, integrated so that workloads or data can move between the two as needed**

**Trace / Why:** hybrid cloud connects private infrastructure (for sensitive/regulated workloads or
predictable base load) with public cloud (for burst capacity or specific services), managed as one
coordinated environment rather than two isolated silos.

**📘 CONCEPT — C27 (see Concept Index)** › The defining feature is *integration* — orchestration,
networking, and often shared identity/security — not merely owning both a private data center and a public
cloud account with no connection between them.

**Wrong traces:** A = a distinct model, not a renaming of public cloud · B = two disconnected private
clouds lack the integration that defines hybrid · D = describes a SaaS-only vendor relationship, unrelated
to deployment-model ownership/integration.

---

**Q36.** **Elasticity**, as a cloud computing characteristic, differs from plain **scalability** because
elasticity specifically implies: `[Trap]`

- **A)** resources can only ever be scaled up, never back down
- **B)** manual, scheduled capacity planning done months in advance
- **C)** automatic, rapid scaling of resources up **and** down in near real time to match actual, fluctuating
  demand
- **D)** scalability and elasticity are strictly identical concepts with no distinction

**Answer: C) automatic, rapid scaling of resources up and down in near real time to match actual, fluctuating demand**

**Trace / Why:** scalability just means a system *can* grow to handle more load (often manually, and often
a one-way, planned expansion); elasticity adds the automatic, bidirectional, on-demand quality that lets
cloud resources shrink back down (and stop being billed) once demand drops.

**📘 CONCEPT — C28 · Scalability vs elasticity**
> Scalability = *capable of* growing with load. Elasticity = grows **and shrinks**, **automatically**, in
> close to real time. Every elastic system is scalable, but not every scalable system is elastic (a
> manually-provisioned bigger server is scalable but not elastic).
>
> **Applies when** the stem contrasts "can grow" with "grows and shrinks automatically," or asks for
> cloud-specific terminology.
>
> **Boundary:** this is the classic paired-terms trap — many candidates treat the words as synonyms, but
> the automatic bidirectional aspect is exactly what the exam tests.

**Wrong traces:** A = elasticity explicitly includes scaling back down, not just up · B = manual, long-lead
planning is the opposite of elasticity's real-time automation · D = the two terms are related but distinct,
as the table shows.

---

**Q37.** **Multi-tenancy** in cloud computing means: `[Core]`

- **A)** each customer requires entirely separate physical hardware, with no sharing at all
- **B)** tenants share the same login credentials for security
- **C)** a single customer occupies multiple, geographically separate data centers
- **D)** multiple customers (tenants) share the same underlying physical infrastructure while remaining
  logically isolated from one another

**Answer: D) multiple customers (tenants) share the same underlying physical infrastructure while remaining logically isolated from one another**

**Trace / Why:** multi-tenancy is what makes public cloud economically viable — many customers' workloads
run on shared physical resources, kept apart by virtualization, network isolation and access controls
rather than dedicated hardware per customer.

**📘 CONCEPT — C29 · Multi-tenancy = shared infrastructure, isolated logically**
> Isolation here is enforced in software (VM boundaries, virtual networks, access control), not by
> physically separate hardware per tenant — that distinction is what makes cloud economics work.
>
> **Applies when** the stem describes many customers sharing infrastructure while remaining separated.
>
> **Boundary:** a **single-tenant** (private) deployment is the alternative — one customer per physical
> environment — trading efficiency for stronger physical isolation.

**Wrong traces:** A = describes single-tenant/dedicated infrastructure, the opposite of multi-tenancy ·
B = shared credentials would break tenant isolation entirely, not describe it · C = describes geographic
distribution, an unrelated concept.

---

**Q38.** A **community cloud** is best described as: `[Applied]`

- **A)** infrastructure shared exclusively by the general public with no restrictions
- **B)** infrastructure shared among several organizations with common concerns (e.g., compliance
  requirements, mission), but not open to the general public
- **C)** a cloud used by exactly one organization only
- **D)** another name for a hybrid cloud

**Answer: B) infrastructure shared among several organizations with common concerns (e.g., compliance requirements, mission), but not open to the general public**

**Trace / Why:** community cloud sits between private (one org) and public (anyone) — it is shared, but only
among a defined group with aligned needs, such as several government agencies under the same regulatory
regime.

**📘 CONCEPT — C27 (see Concept Index)** › Community cloud is the model most often left out when candidates
try to recall "the four deployment models" from memory — it's easy to forget beside the more commonly
discussed public/private/hybrid trio.

**Wrong traces:** A = describes public cloud, unrestricted by definition · C = describes private cloud,
single-tenant · D = hybrid describes integration between private and public, a different axis from
community's "shared among a defined group" model.

---

**Q39.** In the shared-responsibility model common to cloud providers, moving from IaaS toward SaaS: `[Applied]`

- **A)** shifts management responsibility away from the customer and toward the provider
- **B)** has no effect on who manages what
- **C)** shifts all responsibility, including application code, onto the customer
- **D)** eliminates the provider's responsibility entirely

**Answer: A) shifts management responsibility away from the customer and toward the provider**

**Trace / Why:** as shown in the IaaS/PaaS/SaaS table (C26), each step up the stack hands more of the
technology stack — OS, runtime, eventually the application itself — to the provider to manage.

**📘 CONCEPT — C26 (see Concept Index)** › This question restates the same table from a "direction of
travel" angle, which is exactly how exams often re-ask a concept from a second perspective.

**Wrong traces:** B = responsibility allocation changes materially across the three models · C = the
opposite direction — SaaS shifts responsibility *away* from the customer · D = the provider's
responsibility increases, not disappears, moving toward SaaS.

---

**Q40.** Which is the most accurate one-line description of **cloud computing** itself, as distinct from
virtualization alone? `[Core]`

- **A)** cloud computing is simply another name for having a hypervisor installed
- **B)** cloud computing is on-demand delivery of computing resources over a network, typically
  self-service, metered, and elastic — virtualization is one common underlying enabling technology, not a
  synonym for it
- **C)** cloud computing requires that no virtualization be used at all
- **D)** cloud computing only refers to storing files on someone else's server

**Answer: B) cloud computing is on-demand delivery of computing resources over a network, typically self-service, metered, and elastic — virtualization is one common underlying enabling technology, not a synonym for it**

**Trace / Why:** virtualization is a technique (abstracting hardware); cloud computing is a delivery and
consumption *model* (on-demand, self-service, metered, elastic) that is commonly, but not necessarily,
built on virtualization underneath.

**📘 CONCEPT — C30 · Cloud computing is a delivery model, not a synonym for virtualization**
> A private data center can be virtualized without being "cloud" (no self-service or metering); conversely,
> some cloud services are built on bare-metal or container technology rather than traditional VM
> virtualization. The two ideas overlap heavily in practice but answer different questions.
>
> **Applies when** the stem asks for cloud computing's defining characteristics versus virtualization's.
>
> **Boundary:** the exam-safe framing is: virtualization is a common *enabler* of cloud computing, not an
> equivalent term for it.

**Wrong traces:** A = conflates one enabling technology with the whole delivery model · C = many clouds do
use virtualization, though it is not strictly mandatory by definition · D = describes storage/file hosting
only, a narrow special case, not the general definition.

---

*(Sections 6–10, the Concept Index and the Status summary continue in Part 2 —
[mcq_data-center-and-virtualization_part2.md](mcq_data-center-and-virtualization_part2.md))*
