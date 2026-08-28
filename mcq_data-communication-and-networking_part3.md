# Data Communication and Networking — MCQ Question Bank · Part 3
**Subject:** Computer Networks · **Target:** BCS Preliminary / Bank IT Officer / NTRCA / GATE / IBPS SO IT
**Questions:** 60 (Q141–Q200) · **Batch:** Part 3 of 3, written in the same run as Parts 1 and 2

> How to use: attempt the question first, then read the explanation. The wrong
> options matter more than the right one — that's what the examiner is testing.

**Completes the chapter.** Parts 1 and 2 covered fundamentals and performance, topologies and switching,
the OSI and TCP/IP models, transmission media, line coding, modulation, multiplexing, and the data link
layer's framing, error control and flow control. This file covers **medium access control and Ethernet, LAN
devices, the network layer, the transport layer, the application layer and network security**, then closes
with the answer key, coverage report, revision sheet and concept index for all 200 questions. Concept ids
continue from Part 2, starting at **C83**.

**Reminder on scope.** IP addressing, subnet masks, CIDR, VLSM and supernetting are covered in this Bank's
**[Subnetting chapter](../subnetting/mcq_subnetting.md)** (130 questions). Section 8 below therefore covers
the network layer's *other* material — header fields, fragmentation, ICMP, ARP, routing algorithms and
congestion control — without re-asking addressing arithmetic.

---

## Section 7 — Medium Access Control, Ethernet & LAN Devices

**Q141.** The **MAC** sublayer of the data link layer is responsible for `[Core]`

- **A)** converting bits into electrical signals on the medium
- **B)** routing packets between different networks
- **C)** encrypting frames before transmission
- **D)** deciding which station may transmit when the medium is shared

**Answer: D) deciding which station may transmit when the medium is shared**

**Trace / Why:** the data link layer splits into **LLC** (upper — framing, flow and error control) and
**MAC** (lower — medium access). MAC exists only because a shared medium creates contention: without a rule
for taking turns, simultaneous transmissions would collide and both would be lost.

**📘 CONCEPT — C83 · The three families of medium-access protocol**
> | Family | Rule | Examples |
> |---|---|---|
> | **Random access** | transmit when you judge it safe; collisions possible | ALOHA, **CSMA, CSMA/CD, CSMA/CA** |
> | **Controlled access** | a station must be granted permission | **token passing**, polling, reservation |
> | **Channelisation** | the channel is divided in advance | **FDMA, TDMA, CDMA** |
>
> **Applies when** the stem names an access protocol, or asks how stations share a medium.
>
> **Boundary:** MAC is needed **only on a multipoint (shared) medium** (C21) — a point-to-point full-duplex
> link has no contention, which is exactly why switched full-duplex Ethernet turns CSMA/CD off (Q158).
> Note the trade across families: random access is simple and efficient at low load but degrades as load
> rises, while controlled access has overhead at low load and behaves predictably at high load.

**Wrong traces:** A = the physical layer (C28) · B = the network layer (C29) · C = the presentation layer in
OSI terms (C32).

---

**Q142.** The maximum channel utilisation of **pure ALOHA** is approximately `[Applied]` `[Asked: GATE-style]`

- **A)** 9.2%
- **B)** 12.5%
- **C)** 18.4%
- **D)** 36.8%

**Answer: C) 18.4%**

**Trace / Why:** in pure ALOHA a station transmits whenever it has a frame. A frame is destroyed if any other
transmission begins within one frame-time **before or after** it — a vulnerable period of **2 frame-times**.
Maximising throughput S = G·e^(−2G) gives

```
S_max = 1 / (2e) = 0.184 = 18.4%   (at G = 0.5)
```

So more than 80% of the channel's capacity is lost to collisions.

**📘 CONCEPT — C84 · ALOHA: the vulnerable period sets the efficiency**
> | | Pure ALOHA | Slotted ALOHA |
> |---|---|---|
> | Transmit | any time | only at slot boundaries |
> | Vulnerable period | **2 frame-times** | **1 frame-time** |
> | Throughput | S = G·e^(−2G) | S = G·e^(−G) |
> | Maximum | **1/(2e) = 18.4%** at G = 0.5 | **1/e = 36.8%** at G = 1 |
>
> **Applies when** the stem names ALOHA, or asks for maximum throughput or the vulnerable period.
>
> **Boundary:** slotting **halves the vulnerable period and therefore doubles the throughput** — the whole
> gain comes from removing partial overlaps, since a frame can now only collide with one that starts in the
> *same* slot. Note that both figures are dismal compared with CSMA, because ALOHA stations never listen
> before transmitting (Q144).

**Wrong traces:** A = half the correct value · B = 1/8, no derivation · D = the **slotted** ALOHA maximum,
the intended confusion.

---

**Q143.** The maximum channel utilisation of **slotted ALOHA** is approximately `[Applied]`

- **A)** 18.4%
- **B)** 36.8%
- **C)** 50.0%
- **D)** 100%

**Answer: B) 36.8%**

**Trace / Why:** slotting forces every transmission to begin at a slot boundary, so two frames either
collide completely or not at all — the vulnerable period falls to **one** frame-time.

```
S = G·e^(−G),  maximised at G = 1
S_max = 1/e = 0.368 = 36.8%
```

**📘 CONCEPT — C84 (see Concept Index)** › The two figures **1/(2e) ≈ 18.4%** and **1/e ≈ 36.8%** are asked
verbatim and are worth memorising as a pair, along with the loads at which they occur (**G = 0.5** and
**G = 1** respectively). The interpretation of G is offered traffic in frames per frame-time, so G = 1 means
the channel is offered exactly its own capacity — and even then slotted ALOHA delivers only 37% of it, which
is the argument for listening before transmitting.

**Wrong traces:** A = the **pure** ALOHA maximum · C and D = far above what random access without carrier
sensing can achieve.

---

**Q144.** In **CSMA**, a station about to transmit first `[Core]`

- **A)** senses the medium and defers if another transmission is already in progress
- **B)** requests permission from a central controller
- **C)** waits for a token to arrive
- **D)** transmits immediately and checks afterwards whether the frame survived

**Answer: A) senses the medium and defers if another transmission is already in progress**

**Trace / Why:** Carrier Sense Multiple Access means **listen before talk**. Sensing eliminates the
collisions that ALOHA suffers from starting on top of a transmission already under way, which is why CSMA's
utilisation is far higher.

**📘 CONCEPT — C85 · CSMA persistence strategies: what to do when the medium is busy**
> | Strategy | On finding the medium busy | On finding it idle |
> |---|---|---|
> | **1-persistent** | keep sensing continuously | transmit **immediately** |
> | **Non-persistent** | wait a random time, then sense again | transmit immediately |
> | **p-persistent** | (slotted) sense each slot | transmit with probability **p** |
>
> **Applies when** the stem describes a station's behaviour on a busy channel, or names a persistence method.
>
> **Boundary:** sensing cannot eliminate collisions entirely, because of **propagation delay** — two stations
> can both sense idle and both begin transmitting before either's signal reaches the other. That residual
> window is exactly what **collision detection** (Q145) is added to handle, and its length is what sets the
> minimum frame size (Q147). Note also that **1-persistent is the greediest and most collision-prone**, since
> all waiting stations pounce the instant the medium clears.

**Wrong traces:** B = polling, a controlled-access method · C = token passing, also controlled access ·
D = ALOHA's behaviour, with no sensing at all.

---

**Q145.** In **CSMA/CD**, when a station detects a collision while transmitting it `[Core]`

- **A)** completes the frame, then retransmits it immediately
- **B)** switches to a different frequency band and continues
- **C)** waits for a token before trying again
- **D)** aborts the transmission, sends a jam signal, and retries after a random backoff

**Answer: D) aborts the transmission, sends a jam signal, and retries after a random backoff**

**Trace / Why:** continuing to send a frame already known to be destroyed wastes the medium, so the station
aborts at once. A short **jam signal** ensures every other station also detects the collision. Both then wait
a **random** interval before retrying — random, so they do not collide again immediately.

**📘 CONCEPT — C86 · CSMA/CD and binary exponential backoff**
> After the *n*-th successive collision, a station waits a random multiple of the slot time drawn from
> **0 … 2ⁿ − 1**:
>
> | Collision | Random range |
> |---|---|
> | 1st | 0–1 |
> | 2nd | 0–3 |
> | 3rd | 0–7 |
> | 10th | 0–1023 (the range stops doubling here) |
> | 16th | give up and report failure |
>
> **Applies when** the stem mentions backoff, jam signals, or repeated collisions.
>
> **Boundary:** the **doubling** is what makes the scheme stable: as contention rises, stations spread
> themselves over a wider interval automatically, so the collision rate does not run away. A fixed random
> range would collapse under load. Note the ceiling at 10 collisions and the abandonment at 16 — both are
> asked as specific figures.

**Wrong traces:** A = wastes the remaining transmission and guarantees a second collision · B = frequency
hopping is not part of CSMA/CD · C = tokens belong to controlled access.

---

**Q146.** Wireless LANs use **CSMA/CA** rather than CSMA/CD because `[Trap]`

- **A)** wireless links are never subject to collisions
- **B)** collisions are cheaper to tolerate on wireless media
- **C)** the IEEE requires a different acronym for wireless standards
- **D)** a wireless station cannot reliably detect a collision while transmitting, so collisions must be avoided rather than detected

**Answer: D) a wireless station cannot reliably detect a collision while transmitting, so collisions must be avoided rather than detected**

**Trace / Why:** three problems make detection impractical on radio. A transmitting station's own signal
overwhelms its receiver, so it cannot hear anything else. The **hidden-terminal** problem means two stations
may be unable to hear each other while both reach the access point. And signal strength varies so much that
"louder than expected" is not a reliable collision indicator. So 802.11 **avoids** collisions instead —
random backoff *before* transmitting, plus explicit acknowledgements and optional RTS/CTS.

**📘 CONCEPT — C87 · CSMA/CD vs CSMA/CA: detection needs a medium where you can listen while talking**
> | | CSMA/CD | CSMA/CA |
> |---|---|---|
> | Medium | wired (Ethernet) | **wireless** (802.11) |
> | Strategy | detect collisions, then abort and retry | **avoid** collisions before they happen |
> | Backoff timing | **after** a collision | **before** transmitting |
> | Acknowledgements | none at the MAC layer | **every frame is ACKed** |
> | Extras | jam signal | RTS/CTS, IFS intervals |
>
> **Applies when** the stem contrasts wired and wireless access, or mentions hidden terminals or RTS/CTS.
>
> **Boundary:** because a wireless sender cannot tell a collision from any other loss, **the absence of an
> acknowledgement is the only evidence of failure** — which is why 802.11 acknowledges at the MAC layer while
> Ethernet does not. That difference also makes wireless retransmission much slower to trigger, since a
> timeout must expire rather than a collision being sensed within microseconds.

**Wrong traces:** A = wireless collisions are common · B = they are more expensive, being detected only by a
timeout · C = the naming reflects a genuine mechanism difference, not a convention.

---

**Q147.** A CSMA/CD network runs at **10 Mbps** over a 2,000 m cable with a propagation speed of
2 × 10⁸ m/s. The minimum frame size required for reliable collision detection is `[Applied]` `[Asked: GATE-style]`

- **A)** 100 bits
- **B)** 200 bits
- **C)** 400 bits
- **D)** 500 bits

**Answer: B) 200 bits**

**Trace / Why:** a station must still be transmitting when the worst-case collision signal returns — one
propagation delay out and one back.

```
Tp        = 2,000 / (2 × 10^8) = 10 µs
round trip = 2 × Tp = 20 µs
minimum frame = bandwidth × 2Tp
              = 10 × 10^6 × 20 × 10^-6
              = 200 bits   (= 25 bytes)
```

If the frame were shorter the sender would finish and consider it delivered before learning of the
collision.

**📘 CONCEPT — C88 · Minimum frame size = 2 × Tp × bandwidth**
> ```
> minimum frame (bits) = 2 × propagation delay × data rate
> ```
> The frame's transmission time must be at least the round-trip propagation time. Two consequences follow
> directly:
> - **faster** links need **larger** minimum frames, or shorter cables;
> - **longer** cables need larger minimum frames, or slower links.
>
> **Applies when** the stem gives a distance, a rate and a speed, or asks why fast Ethernet limits cable
> length.
>
> **Boundary:** this is why **Gigabit Ethernet** could not simply reuse Ethernet's 64-byte minimum over
> 200 m — at 1 Gbps the required minimum would be far larger, so the standard added **carrier extension**
> (padding the slot time artificially) for half-duplex operation, and in practice moved to full duplex where
> the whole issue disappears (Q158). The trade between rate, distance and frame size is unavoidable in
> CSMA/CD.

**Wrong traces:** A = uses one-way Tp instead of the round trip — the intended trap · C = doubles the round
trip · D = uses 2,500 m.

---

**Q148.** For standard Ethernet, the **minimum** frame size and the **maximum** payload (MTU) are
respectively `[Trap]`

- **A)** 46 bytes and 1,518 bytes
- **B)** 64 bytes and 1,518 bytes
- **C)** 46 bytes and 1,500 bytes
- **D)** 64 bytes and 1,500 bytes

**Answer: D) 64 bytes and 1,500 bytes**

**Trace / Why:** the minimum **frame** is 64 bytes (18 bytes of header and trailer plus a 46-byte minimum
payload, padded if necessary). The maximum **payload** — the MTU — is 1,500 bytes, giving a maximum frame of
1,518 bytes.

```
minimum frame  =  14 header + 46 payload +  4 FCS =   64 bytes
maximum frame  =  14 header + 1500 payload + 4 FCS = 1518 bytes
```

**📘 CONCEPT — C89 · The Ethernet frame, and the four sizes that get confused**
> | Field | Bytes |
> |---|---|
> | Preamble + SFD | 8 (not counted in frame size) |
> | Destination MAC | 6 |
> | Source MAC | 6 |
> | Type / Length | 2 |
> | **Payload** | **46 – 1,500** |
> | FCS (CRC-32) | 4 |
>
> **Minimum frame 64 B · minimum payload 46 B · maximum payload (MTU) 1,500 B · maximum frame 1,518 B.**
>
> **Applies when** the stem asks for any Ethernet size, or gives an MTU for fragmentation (C91).
>
> **Boundary:** the 64-byte minimum exists for **collision detection** (C88), not for any header requirement
> — which is why a short payload is **padded** rather than rejected. And note the frequently-repeated error
> that "the minimum frame size in CSMA/CD is 1500 bytes": 1,500 is the **maximum payload**, and confusing the
> two is exactly what this question tests. All four options here are real Ethernet numbers; only one pairing
> is correct.

**Wrong traces:** A = minimum **payload** paired with maximum **frame** · B = minimum frame paired with
maximum frame · C = minimum payload paired with maximum payload.

---

**Q149.** A MAC address is `[Core]`

- **A)** 16 bits long, assigned by the operating system
- **B)** 32 bits long, assigned by the network administrator
- **C)** 64 bits long, derived from the IP address
- **D)** 48 bits long, normally burned into the network interface by the manufacturer

**Answer: D) 48 bits long, normally burned into the network interface by the manufacturer**

**Trace / Why:** 48 bits, written as six hexadecimal pairs such as `00:1A:2B:3C:4D:5E`. The first three bytes
are the **OUI**, identifying the manufacturer; the last three are assigned by that manufacturer, making the
address globally unique.

**📘 CONCEPT — C90 · MAC addresses: flat, globally unique, and therefore not routable**
> Structure: **24-bit OUI** (vendor) + 24-bit device serial. Special forms: the broadcast address
> **FF:FF:FF:FF:FF:FF**, and multicast addresses identified by the least significant bit of the first byte
> being 1.
>
> **Applies when** the stem asks for an address length, format, or how uniqueness is achieved.
>
> **Boundary:** MAC addresses are **flat** — they carry no location information whatsoever, so a router
> cannot aggregate or summarise them. That is precisely why IP addresses exist alongside them (C40): IP is
> hierarchical and therefore routable, MAC is flat and therefore only usable within one link. It is also why
> a MAC address is meaningful only on its own segment and never appears in a routing table.

**Wrong traces:** A = 16 bits is a port number (C41) · B = 32 bits is an IPv4 address · C = 64 bits is the
IPv6 interface identifier, which may be *derived from* a MAC but is not one.

---

**Q150.** Which field of an Ethernet frame is used to detect transmission errors? `[Core]`

- **A)** the Frame Check Sequence, holding a CRC-32 value
- **B)** the Preamble
- **C)** the Type/Length field
- **D)** the Source MAC address

**Answer: A) the Frame Check Sequence, holding a CRC-32 value**

**Trace / Why:** the 4-byte **FCS** trailer holds a CRC-32 computed over the whole frame from the destination
address onward (C73). The receiver recomputes it; a mismatch means the frame is discarded silently — Ethernet
does **not** retransmit, leaving recovery to the layers above.

**📘 CONCEPT — C89 (see Concept Index)** › The FCS is a **trailer** rather than a header field precisely
because it must cover everything that precedes it (C42), and it is the reason the data link layer is the one
layer that adds both a header and a trailer. Note what Ethernet does *not* do on failure: there is **no
acknowledgement and no retransmission** at the Ethernet layer, so a corrupted frame is simply dropped and
TCP's end-to-end machinery must notice (C35). The **preamble**, by contrast, is a 7-byte alternating pattern
for clock synchronisation and is not part of the frame proper.

**Wrong traces:** B = the preamble synchronises the receiver's clock · C = identifies the upper-layer
protocol or payload length · D = identifies the sender.

---

**Q151.** In **binary exponential backoff**, after the third successive collision a station chooses its wait
from the range `[Applied]`

- **A)** 0 to 3 slot times
- **B)** 0 to 7 slot times
- **C)** 0 to 8 slot times
- **D)** 0 to 15 slot times

**Answer: B) 0 to 7 slot times**

**Trace / Why:** after the *n*-th collision the range is **0 to 2ⁿ − 1**.

```
n = 3  →  2^3 − 1 = 7   →  choose from {0, 1, 2, ... 7}
```

The range doubles with each successive collision (1st: 0–1, 2nd: 0–3, 3rd: 0–7), spreading contending
stations further apart as congestion worsens.

**📘 CONCEPT — C86 (see Concept Index)** › The **−1** is what makes these off-by-one questions: the range has
2ⁿ *values* but its upper bound is 2ⁿ − 1, so option C (0 to 8) is the standard wrong answer. Note the two
fixed limits worth recalling: the range stops doubling after the **10th** collision (capped at 0–1023) and
the station **gives up after the 16th**, reporting failure to the layer above.

**Wrong traces:** A = the range after the **second** collision · C = 2ⁿ used as the bound instead of
2ⁿ − 1 · D = the range after the **fourth** collision.

---

**Q152.** In **token passing**, a station may transmit `[Core]`

- **A)** whenever it senses the medium is idle
- **B)** only while it holds the token
- **C)** at any time, retrying if a collision occurs
- **D)** during its pre-assigned time slot in every frame

**Answer: B) only while it holds the token**

**Trace / Why:** a special control frame — the **token** — circulates. A station wishing to transmit captures
it, sends its data, and then releases the token to the next station. Since only one token exists, only one
station can transmit at a time and **collisions are impossible by construction**.

**📘 CONCEPT — C91 · Controlled access trades overhead for predictability**
> Token passing guarantees no collisions and a **bounded worst-case waiting time** — a station waits at most
> one full circulation of the token. That determinism is why it was preferred for industrial and real-time
> networks, where a guaranteed maximum delay matters more than average throughput.
>
> **Applies when** the stem mentions tokens, polling, reservation, Token Ring or FDDI.
>
> **Boundary:** the costs are **token-management complexity** (a lost token must be regenerated, a duplicate
> removed, and a monitor station elected to do so) and **overhead at low load**, since a station must wait for
> the token even when nobody else wants the medium. CSMA/CD has the opposite profile — near-zero overhead when
> idle, and unbounded worst-case delay under contention. Switched Ethernet eventually won by removing
> contention altogether (Q158) rather than by managing it better.

**Wrong traces:** A = CSMA · C = ALOHA · D = TDMA channelisation (C83).

---

**Q153.** A **cut-through** switch differs from a **store-and-forward** switch in that a cut-through switch
`[Applied]`

- **A)** buffers the entire frame and verifies the FCS before forwarding
- **B)** forwards the frame as soon as it has read the destination address, without waiting for the whole frame
- **C)** forwards frames only to ports in the same VLAN
- **D)** converts frames between different data link protocols

**Answer: B) forwards the frame as soon as it has read the destination address, without waiting for the whole frame**

**Trace / Why:** a cut-through switch begins forwarding after the first 6 bytes (the destination MAC),
cutting latency dramatically. The price is that the **FCS has not yet arrived**, so it cannot check for
errors — corrupted and runt frames are forwarded anyway.

**📘 CONCEPT — C92 · Switching methods: latency against error checking**
> | Method | Waits for | Latency | Error checking |
> |---|---|---|---|
> | **Store-and-forward** | the **whole frame** | highest | **full FCS check** |
> | **Cut-through** | 6 bytes (destination MAC) | **lowest** | **none** |
> | Fragment-free | first 64 bytes | medium | detects collision fragments |
>
> **Applies when** the stem contrasts switching methods or asks about switch latency.
>
> **Boundary:** **fragment-free** is the compromise, and its 64-byte threshold is not arbitrary — it is
> Ethernet's minimum frame size (C89), so any frame shorter than that is a collision fragment and can be
> discarded, catching the commonest error class without waiting for the full frame. Note that modern switches
> overwhelmingly use store-and-forward, because at gigabit rates the buffering delay is negligible and error
> checking is worth having.

**Wrong traces:** A = **store-and-forward** · C = VLAN behaviour, orthogonal to the switching method ·
D = a translational bridge or gateway.

---

**Q154.** The purpose of the **Spanning Tree Protocol** in a bridged network is to `[Core]`

- **A)** prevent broadcast storms and frame duplication by blocking redundant links to leave a loop-free topology
- **B)** distribute traffic evenly across all available links
- **C)** assign IP addresses to bridges automatically
- **D)** encrypt frames travelling between bridges

**Answer: A) prevent broadcast storms and frame duplication by blocking redundant links to leave a loop-free topology**

**Trace / Why:** bridges **flood** frames whose destination is unknown, and broadcasts always (C38). With a
physical loop, a flooded frame circulates endlessly and is duplicated at each pass — a **broadcast storm**
that saturates the network within seconds. Since layer-2 frames carry no TTL to stop them, the loop must be
removed logically: STP elects a root bridge and blocks selected ports so that exactly one active path remains
between any two segments.

**📘 CONCEPT — C93 · Layer 2 has no TTL, so loops must be prevented rather than survived**
> A looped frame at layer 2 lives **forever**, because nothing decrements a hop count. Three symptoms:
> broadcast storms, multiple frame copies, and MAC address table instability as the same source appears on
> different ports.
>
> STP's mechanism: elect a **root bridge** (lowest bridge ID) → each bridge finds its lowest-cost path to the
> root → all other ports are **blocked**, leaving a tree.
>
> **Applies when** the stem mentions redundant links between switches, broadcast storms, or root bridges.
>
> **Boundary:** the contrast with layer 3 is the examinable insight — an IP packet caught in a routing loop is
> killed by the **TTL** (C97), so the network layer tolerates transient loops while the data link layer cannot.
> Note the cost of STP: redundant links are **blocked, not load-shared**, so bandwidth sits idle purely as
> standby — which is what later protocols (link aggregation, TRILL, SPB) were designed to reclaim.

**Wrong traces:** B = STP **blocks** redundant links rather than balancing across them · C = that is DHCP ·
D = STP performs no encryption.

---

**Q155.** A **VLAN** allows a network administrator to `[Applied]`

- **A)** group ports into separate broadcast domains regardless of their physical location
- **B)** increase the physical bandwidth of each switch port
- **C)** route packets between different IP networks without a router
- **D)** eliminate the need for MAC addresses on the LAN

**Answer: A) group ports into separate broadcast domains regardless of their physical location**

**Trace / Why:** a switch normally puts all its ports in one broadcast domain (C37). VLANs partition it
logically: ports assigned to VLAN 10 form one broadcast domain and ports in VLAN 20 another, with no traffic
between them at layer 2 — even though they share the same switch. Membership follows configuration, not
cabling, so users on different floors can share a VLAN.

**📘 CONCEPT — C94 · VLANs give a switch the broadcast-domain separation it otherwise lacks**
> Benefits: **smaller broadcast domains** without extra hardware · security segmentation · logical grouping
> independent of physical layout · simplified moves and changes. Frames crossing between switches carry an
> **802.1Q tag** (4 bytes, including a 12-bit VLAN ID) on trunk links.
>
> **Applies when** the stem mentions broadcast-domain segmentation on a switch, 802.1Q, trunks, or logical
> grouping.
>
> **Boundary:** VLANs separate domains but **cannot move traffic between them** — that still requires a router
> or a layer-3 switch, an arrangement called **inter-VLAN routing** or router-on-a-stick. So option C inverts
> the actual limitation: VLANs create the boundary that a router is then needed to cross. In counting terms,
> **broadcast domains = number of VLANs**, which refines the rule in C37.

**Wrong traces:** B = VLANs are logical and add no bandwidth · C = inter-VLAN traffic **requires** layer-3
routing · D = MAC addresses remain essential.

---

**Q156.** An 8-port **switch** has one host connected to each port. How many **collision domains** exist?
`[Applied]`

- **A)** 1
- **B)** 2
- **C)** 4
- **D)** 8

**Answer: D) 8**

**Trace / Why:** each switch port is its own collision domain, because the switch buffers frames and no two
hosts contend for the same medium.

```
collision domains = number of switch ports in use = 8
broadcast domains = 1  (a single VLAN)
```

By contrast an 8-port **hub** would give **1** collision domain — all eight hosts sharing one medium — and
1 broadcast domain.

**📘 CONCEPT — C95 · Counting domains: switches split collisions, routers split broadcasts**
> ```
> collision domains  = number of switch/bridge ports   (a hub = 1 for the whole device)
> broadcast domains  = number of router interfaces, or number of VLANs
> ```
> | Device | Collision domains | Broadcast domains |
> |---|---|---|
> | 8-port hub | **1** | 1 |
> | 8-port switch | **8** | 1 |
> | Router, 3 interfaces | 3 | **3** |
>
> **Applies when** the stem gives a topology and asks for a count of either domain type.
>
> **Boundary:** with **full-duplex** switched links there are strictly no collisions at all, so "8 collision
> domains" is really "8 links on which collision handling is unnecessary" — the standard exam answer counts
> ports regardless. Note that adding switches never reduces the broadcast-domain count; only routers and
> VLANs do (C94).

**Wrong traces:** A = the answer for a **hub** · B and C = no derivation from the port count.

---

**Q157.** **PPP** (Point-to-Point Protocol) is used to `[Core]`

- **A)** resolve IP addresses into MAC addresses on a LAN
- **B)** carry frames over a point-to-point link, with negotiation of link and network parameters
- **C)** control which station may transmit on a shared medium
- **D)** route packets between autonomous systems

**Answer: B) carry frames over a point-to-point link, with negotiation of link and network parameters**

**Trace / Why:** PPP is the standard data link protocol for links with exactly two endpoints — dial-up, DSL,
serial WAN links. Because there is no contention it needs no MAC protocol; instead it provides **LCP** (Link
Control Protocol) to negotiate link options and authenticate, and **NCP** to configure network-layer
parameters such as assigning an IP address.

**📘 CONCEPT — C96 · Point-to-point data link protocols: no medium access, but negotiation instead**
> | | PPP | HDLC |
> |---|---|---|
> | Framing | flag-delimited, **byte stuffing** | flag-delimited, **bit stuffing** (C68) |
> | Negotiation | **LCP + NCP** | none |
> | Authentication | PAP, CHAP | none |
> | Multiprotocol | **yes** — carries IP, IPv6 and others | limited |
>
> **Applies when** the stem names PPP, HDLC, SLIP, LCP, CHAP, or a dial-up or serial WAN link.
>
> **Boundary:** PPP has **no MAC sublayer function at all** (C83), because a point-to-point link has nothing
> to arbitrate — which is the cleanest illustration that medium access control exists only for shared media
> (C21). What PPP adds instead is everything a shared LAN gets from elsewhere: authentication, address
> assignment and protocol negotiation.

**Wrong traces:** A = ARP (C98) · C = a MAC protocol, which PPP does not need · D = BGP, a routing protocol
(C101).

---

**Q158.** In a **full-duplex switched** Ethernet network, CSMA/CD is `[Applied]`

- **A)** disabled, because each link has exactly two endpoints and collisions cannot occur
- **B)** still required, to arbitrate between the switch and the host
- **C)** replaced by token passing
- **D)** required only on links faster than 1 Gbps

**Answer: A) disabled, because each link has exactly two endpoints and collisions cannot occur**

**Trace / Why:** a switch port connected to one host is a **point-to-point** link, and full duplex gives each
direction its own path. Only one transmitter exists per direction, so contention is structurally impossible
and the collision-handling machinery is switched off — which also removes the minimum-frame-size constraint
(C88) and the cable-length limits it implied.

**📘 CONCEPT — C97 · Switched full duplex removes contention rather than managing it**
> | | Shared (hub, half duplex) | Switched (full duplex) |
> |---|---|---|
> | Medium | multipoint | **point-to-point per port** |
> | Collisions | possible | **impossible** |
> | CSMA/CD | required | **disabled** |
> | Bandwidth per host | shared among all | **dedicated, both directions** |
>
> **Applies when** the stem contrasts hubs with switches, or asks whether CSMA/CD is active.
>
> **Boundary:** this is the endpoint of the whole medium-access story — decades of protocol effort (ALOHA →
> CSMA → CSMA/CD → backoff tuning) were ultimately made unnecessary by cheap switching, which eliminated the
> shared medium that created the problem. CSMA/CD remains examinable and remains in the standard, but is
> **inactive in essentially every modern wired LAN**. Wireless, being genuinely shared, still needs
> CSMA/CA (C87).

**Wrong traces:** B = two endpoints in full duplex cannot collide · C = token passing is unrelated to
Ethernet · D = the rate is irrelevant; the duplex mode and topology decide.

---
## Section 8 — Network Layer: IP, Fragmentation, ICMP & Routing

*Addressing and subnetting live in the [Subnetting chapter](../subnetting/mcq_subnetting.md); this section
covers everything else the network layer does.*

**Q159.** The IP protocol provides a service that is `[Core]`

- **A)** connection-oriented and reliable, guaranteeing delivery in order
- **B)** connection-oriented but unreliable
- **C)** connectionless and unreliable, making a best-effort attempt at delivery
- **D)** connectionless but reliable, retransmitting lost packets

**Answer: C) connectionless and unreliable, making a best-effort attempt at delivery**

**Trace / Why:** IP sets up nothing before sending, routes each datagram independently (C26), and makes no
promise about delivery, ordering or duplication. It will discard a packet whose TTL expires, whose header
checksum fails, or which arrives at a congested router with no buffer — and it does not tell the sender.

**📘 CONCEPT — C98 · IP is deliberately minimal, and reliability is built above it**
> IP guarantees **none** of: delivery, ordering, freedom from duplication, or a bounded delay. What it does
> provide is addressing, routing and fragmentation. Reliability is supplied end to end by **TCP** where it is
> wanted (C102), and deliberately omitted by **UDP** where it is not.
>
> **Applies when** the stem asks what IP guarantees, or which layer supplies reliability.
>
> **Boundary:** IP's own **header checksum** covers the header only, **not the payload** — so a corrupted
> header is caught (and the packet dropped) while corrupted data passes through for TCP or UDP to detect.
> Note that "best effort" is a design choice, not a deficiency: keeping the network simple is what let IP run
> over any link technology and scale to the Internet, with complexity pushed to the endpoints.

**Wrong traces:** A = describes TCP, not IP · B = IP is not connection-oriented in any sense · D = IP never
retransmits.

---

**Q160.** The **TTL** field in an IPv4 header exists to `[Applied]`

- **A)** indicate how long the destination should buffer the packet
- **B)** specify the maximum transmission rate for the packet
- **C)** record the time at which the packet was sent
- **D)** prevent a packet from circulating forever if a routing loop occurs

**Answer: D) prevent a packet from circulating forever if a routing loop occurs**

**Trace / Why:** every router **decrements** the TTL by one and discards the packet when it reaches zero,
sending an ICMP *Time Exceeded* message back to the source. Without it, a transient routing loop would trap
packets permanently and they would accumulate until the loop consumed all capacity.

**📘 CONCEPT — C99 · TTL is a hop counter, and it is what makes layer-3 loops survivable**
> Despite the name, TTL counts **hops**, not seconds. Two uses follow:
> - **loop protection** — a looped packet dies after at most TTL hops;
> - **traceroute** — deliberately sending packets with TTL 1, 2, 3 … elicits a *Time Exceeded* from each
>   router in turn, revealing the path (Q167).
>
> **Applies when** the stem mentions TTL, hop limits, routing loops, or traceroute.
>
> **Boundary:** contrast this with **layer 2**, which has no TTL at all — a looped Ethernet frame lives
> forever, which is precisely why the Spanning Tree Protocol must *prevent* loops rather than tolerate them
> (C93). The network layer's willingness to tolerate transient loops is what allows routing protocols to
> converge gradually instead of having to block links.

**Wrong traces:** A = no such buffering instruction exists · B = rate is not carried in the header ·
C = the header holds no timestamp in normal operation.

---

**Q161.** A datagram of **4,000 bytes** (including a 20-byte header) must cross a link with an **MTU of
1,500 bytes**. How many fragments are produced? `[Applied]` `[Asked: GATE-style]`

- **A)** 1
- **B)** 2
- **C)** 3
- **D)** 4

**Answer: C) 3**

**Trace / Why:** each fragment needs its own 20-byte header, and the data carried per fragment must be a
**multiple of 8** bytes.

```
data to carry          = 4,000 − 20 = 3,980 bytes
max data per fragment  = 1,500 − 20 = 1,480 bytes   (1,480 is a multiple of 8 ✔)

fragment 1: 1,480 bytes
fragment 2: 1,480 bytes
fragment 3: 3,980 − 2,960 = 1,020 bytes
                              → 3 fragments
```

**📘 CONCEPT — C100 · Fragmentation arithmetic: subtract the header, then divide by a multiple of 8**
> ```
> data per fragment = largest multiple of 8 ≤ (MTU − header size)
> fragments         = ceil( total data / data per fragment )
> ```
> Each fragment is an independent datagram with a full header, so the header cost is paid **per fragment**.
>
> **Applies when** the stem gives a datagram size and an MTU.
>
> **Boundary:** the **multiple-of-8** rule exists because the Fragment Offset field counts in **8-byte
> units** (Q162), so a fragment must start on an 8-byte boundary. Forgetting to subtract the header from the
> MTU is the other standard error and gives 1,500 bytes of data per fragment, hence 3 fragments by luck here
> but wrong offsets in Q162 — so the mistake shows up in the follow-up question rather than this one.

**Wrong traces:** A = assumes no fragmentation is needed, but 4,000 > 1,500 · B = uses ~2,000 bytes of data
per fragment · D = uses ~1,000 bytes per fragment.

---

**Q162.** For the fragmentation in Q161, the **fragment offset** value carried by the **second** fragment is
`[Applied]` `[Asked: GATE-style]`

- **A)** 0
- **B)** 148
- **C)** 185
- **D)** 1,480

**Answer: C) 185**

**Trace / Why:** the offset is measured in **units of 8 bytes**, counted from the start of the original
datagram's data.

```
bytes preceding fragment 2 = 1,480
offset = 1,480 / 8 = 185
```

The three fragments therefore carry offsets **0, 185 and 370**, with the More Fragments (MF) flag set to 1,
1 and 0 respectively.

**📘 CONCEPT — C100 (see Concept Index)** › The **÷ 8** is the whole question, and it is why fragment data
sizes must be multiples of 8 — an offset of 1,480 bytes is only expressible because 1,480/8 is an integer.
The 13-bit offset field can thus address 2¹³ × 8 = 65,536 bytes, exactly matching IP's maximum datagram size.
Note the flags that travel with it: **MF = 1** on every fragment except the last, and **DF = 1** forbids
fragmentation entirely (the router must then drop the packet and report ICMP *Fragmentation Needed*, which is
how path-MTU discovery works).

**Wrong traces:** A = the **first** fragment's offset · B = 1,480 ÷ 10, a wrong divisor · D = the offset in
**bytes**, with the division by 8 forgotten — the intended trap.

---

**Q163.** In IPv4, reassembly of a fragmented datagram is performed `[Trap]`

- **A)** at each router along the path, which then re-fragments as needed
- **B)** at the first router that has an MTU large enough
- **C)** by the sending host after receiving an acknowledgement
- **D)** only at the final destination host

**Answer: D) only at the final destination host**

**Trace / Why:** fragments are routed independently and may take different paths, so no intermediate router
can be sure it has seen them all. Reassembly therefore happens **once**, at the destination, which buffers
fragments until the set is complete (identified by matching the Identification field, source and destination
addresses).

**📘 CONCEPT — C101 · Fragment at any router, reassemble only at the destination**
> The asymmetry is deliberate. Reassembling en route would require a router to hold state, wait for stragglers
> and re-fragment for the next link — expensive and pointless when a later link may have a smaller MTU anyway.
>
> **Applies when** the stem asks where fragmentation or reassembly occurs.
>
> **Boundary:** the cost of destination-only reassembly is that **losing one fragment destroys the entire
> datagram** — the destination discards the incomplete set after a timer expires, and there is no way to
> retransmit a single fragment. That fragility is a large part of why **IPv6 forbids router fragmentation
> altogether** (Q174), requiring the source to discover the path MTU and size its packets accordingly.

**Wrong traces:** A = routers may fragment further but never reassemble · B = no router reassembles ·
C = the sender is not involved in reassembly.

---

**Q164.** The purpose of **ARP** is to `[Core]`

- **A)** assign IP addresses to hosts as they join the network
- **B)** report delivery errors back to the source host
- **C)** find the MAC address corresponding to a known IP address on the local link
- **D)** translate private addresses into public addresses

**Answer: C) find the MAC address corresponding to a known IP address on the local link**

**Trace / Why:** to send a packet, a host must build a frame, and a frame needs the **destination MAC
address**. The host knows only the IP address, so it broadcasts an ARP request — "who has 192.168.1.7?" — and
the owner replies with its MAC address, which is then cached in an ARP table.

**📘 CONCEPT — C102 · ARP bridges the layer-3/layer-2 address gap, on every hop**
> ```
> have: IP address        need: MAC address        → ARP  (request broadcast, reply unicast)
> ```
> ARP is needed at **every hop**, because MAC addresses change per link while IP addresses do not (C36) —
> a router must ARP for the next hop's MAC before it can build the outgoing frame.
>
> **Applies when** the stem describes resolving an address, mentions ARP caches, or asks how a frame's
> destination is determined.
>
> **Boundary:** ARP resolves only **within one link** — a host ARPs for the *default gateway's* MAC, not for
> the remote destination's, because the remote host is not reachable by broadcast. So an ARP table contains
> only local neighbours, however distant the traffic's ultimate destination. Note that **RARP** did the
> reverse (MAC → IP) and has been superseded by DHCP.

**Wrong traces:** A = DHCP · B = ICMP (Q165) · D = NAT.

---

**Q165.** **ICMP** is used to `[Core]`

- **A)** carry application data reliably between hosts
- **B)** assign logical addresses to interfaces
- **C)** report errors and provide diagnostic information about IP packet delivery
- **D)** resolve domain names into IP addresses

**Answer: C) report errors and provide diagnostic information about IP packet delivery**

**Trace / Why:** IP itself has no way to report a failure (C98). ICMP fills the gap, carrying messages such
as *Destination Unreachable*, *Time Exceeded*, *Redirect* and *Echo Request/Reply* back to the source. It is
encapsulated **inside IP** yet is considered part of the network layer.

**📘 CONCEPT — C103 · The ICMP messages that get examined**
> | Message | Meaning / use |
> |---|---|
> | **Echo Request / Reply** | **ping** — reachability and round-trip time |
> | **Time Exceeded** | TTL hit zero — used by **traceroute** (C99) |
> | Destination Unreachable | no route, or port closed |
> | Source Quench (obsolete) | primitive congestion signal |
> | Redirect | "use a better first-hop router" |
> | Fragmentation Needed | DF set but the packet is too large — path-MTU discovery |
>
> **Applies when** the stem names ping, traceroute, an unreachable message, or asks how IP reports errors.
>
> **Boundary:** ICMP **reports** problems but never **fixes** them — it does not retransmit, reroute or slow
> the sender down; it merely informs. And an ICMP error message is itself sent as an ordinary best-effort IP
> packet, so it can be lost, which is why diagnostics based on it are advisory rather than definitive.

**Wrong traces:** A = TCP or UDP carry data · B = DHCP assigns addresses · D = DNS.

---

**Q166.** The `ping` utility tests reachability by sending `[Applied]`

- **A)** TCP SYN segments and waiting for SYN-ACK replies
- **B)** ICMP Echo Request messages and waiting for Echo Reply messages
- **C)** ARP requests and waiting for ARP replies
- **D)** UDP datagrams to port 7 and waiting for a response

**Answer: B) ICMP Echo Request messages and waiting for Echo Reply messages**

**Trace / Why:** ping sends an ICMP Echo Request; a reachable host that is not filtering ICMP returns an Echo
Reply. The round-trip time is measured from the interval between them, and repeated probes reveal packet loss
and jitter.

**📘 CONCEPT — C103 (see Concept Index)** › Ping tests the **network layer** and no higher — a successful ping
proves the host is reachable and its IP stack is running, but says nothing about whether any application or
service is working. Conversely a **failed** ping does not prove unreachability, because firewalls very
commonly block ICMP while permitting TCP: the host may be perfectly reachable on port 443 and silent to ping.
Both halves of that asymmetry are examinable and both matter in real diagnosis.

**Wrong traces:** A = describes a TCP port scan, not ping · C = ARP works only on the local link (C102) and
cannot test a remote host · D = the echo *service* on port 7 is a different, obsolete mechanism.

---

**Q167.** `traceroute` discovers the routers along a path by `[Applied]`

- **A)** sending packets with progressively increasing TTL values and collecting the resulting Time Exceeded messages
- **B)** querying each router's routing table directly using SNMP
- **C)** broadcasting an ARP request to every network along the path
- **D)** reading the Record Route option from the returning packet

**Answer: A) sending packets with progressively increasing TTL values and collecting the resulting Time Exceeded messages**

**Trace / Why:** a packet sent with **TTL = 1** is discarded by the first router, which returns an ICMP *Time
Exceeded* naming itself. TTL = 2 elicits a reply from the second router, and so on — so the path is revealed
one hop per round. When the packet finally reaches the destination, a *Port Unreachable* (or Echo Reply) marks
the end.

**📘 CONCEPT — C99 (see Concept Index)** › Traceroute is a deliberate **exploitation** of the loop-protection
mechanism, which is what makes it an elegant piece of engineering: no router is asked to cooperate, and each
identifies itself simply by obeying the TTL rule. Two consequences worth knowing: routers that are configured
not to send ICMP appear as `* * *`, and because each probe is routed independently, **successive hops may
follow different paths** where load balancing is in use — so a traceroute is a sample of the path, not a
definitive map.

**Wrong traces:** B = requires SNMP access and credentials on every router · C = ARP is link-local (C102) ·
D = the IP Record Route option exists but is almost universally disabled and is not how traceroute works.

---

**Q168.** In a **distance-vector** routing protocol, each router `[Core]`

- **A)** shares its distance estimates to all destinations with its immediate neighbours only
- **B)** floods a complete map of the network to every other router
- **C)** computes the shortest path using Dijkstra's algorithm on a full topology database
- **D)** forwards packets based on a manually configured static table

**Answer: A) shares its distance estimates to all destinations with its immediate neighbours only**

**Trace / Why:** each router keeps a vector of ⟨destination, distance, next hop⟩ and periodically sends it to
its **neighbours**. On receiving a neighbour's vector it applies the Bellman–Ford update — "if going via this
neighbour is cheaper, adopt it". No router ever sees the whole topology; it knows only distances reported to
it.

**📘 CONCEPT — C104 · Distance vector vs link state: what each router knows and tells**
> | | Distance vector | Link state |
> |---|---|---|
> | Knows | **distances** to all destinations | the **full topology** map |
> | Tells | **all destinations**, to **neighbours only** | **its own links**, to **everyone** (flooding) |
> | Algorithm | **Bellman–Ford** | **Dijkstra** |
> | Convergence | **slow**; suffers count-to-infinity | **fast** |
> | Resource use | low memory, low CPU | higher memory and CPU |
> | Examples | **RIP**, IGRP | **OSPF**, IS-IS |
>
> **Applies when** the stem describes what is advertised, to whom, or names a protocol or algorithm.
>
> **Boundary:** the memorable contrast is "**tell everyone about your neighbours** (link state) versus **tell
> your neighbours about everyone** (distance vector)". Distance vector's ignorance of topology is exactly what
> causes the **count-to-infinity** problem (Q171), since a router cannot tell that a neighbour's advertised
> route actually loops back through itself.

**Wrong traces:** B and C = link-state behaviour · D = static routing, which involves no protocol.

---

**Q169.** In **RIP**, the maximum valid hop count is `[Applied]`

- **A)** 8
- **B)** 12
- **C)** 15
- **D)** 16

**Answer: C) 15**

**Trace / Why:** RIP uses hop count as its metric and treats **16 as infinity** — meaning unreachable. So the
largest usable path is **15 hops**, and any destination further away is unreachable as far as RIP is
concerned.

**📘 CONCEPT — C105 · RIP's 15-hop limit is a bound on count-to-infinity, not on network size alone**
> | Property | RIP |
> |---|---|
> | Type | distance vector |
> | Metric | **hop count** |
> | Maximum hops | **15** (16 = infinity) |
> | Update interval | every 30 s, full table |
> | Suits | small networks |
>
> **Applies when** the stem names RIP, hop counts, or asks why RIP does not scale.
>
> **Boundary:** the small ceiling is chosen deliberately: because count-to-infinity makes a bad route's metric
> climb one hop at a time (Q171), a **low** value of "infinity" bounds how long that climb can take. So the
> 15-hop limit is a convergence-time safeguard, not merely an arbitrary restriction — and it is why RIP is
> unsuitable for large networks in two ways at once, by diameter and by convergence speed.

**Wrong traces:** A and B = no derivation · D = 16 is the value meaning **infinity**, so it is not a valid
path length — the intended off-by-one.

---

**Q170.** **OSPF** determines routes by `[Core]`

- **A)** building a complete topology database by flooding link-state advertisements, then running Dijkstra's shortest-path algorithm
- **B)** exchanging hop-count vectors with neighbours every 30 seconds
- **C)** using the AS-path attribute to choose between external routes
- **D)** relying on statically configured routes entered by the administrator

**Answer: A) building a complete topology database by flooding link-state advertisements, then running Dijkstra's shortest-path algorithm**

**Trace / Why:** each OSPF router floods **LSAs** describing its own directly connected links to every other
router in the area. Every router therefore assembles an identical topology map and independently runs
**Dijkstra** to compute its shortest-path tree. Because every router has the full picture, convergence is
fast and loops are avoided by construction.

**📘 CONCEPT — C104 (see Concept Index)** › OSPF's extra machinery all follows from carrying a full topology:
**areas** partition the flooding domain so that large networks do not require every router to hold every LSA,
and the metric is **cost** (typically derived from bandwidth) rather than a raw hop count — so OSPF prefers a
fast two-hop path over a slow one-hop path, which RIP cannot. Both are **interior** gateway protocols,
operating within one autonomous system, in contrast to BGP (Q172).

**Wrong traces:** B = RIP · C = BGP · D = static routing.

---

**Q171.** The **count-to-infinity** problem in distance-vector routing occurs because `[Trap]`

- **A)** hop counts are stored in a field too small to hold large values
- **B)** routers flood their entire topology database too frequently
- **C)** a router cannot tell that a route advertised by a neighbour actually passes back through itself, so a failed route's metric rises one hop at a time
- **D)** link-state advertisements are lost during flooding

**Answer: C) a router cannot tell that a route advertised by a neighbour actually passes back through itself, so a failed route's metric rises one hop at a time**

**Trace / Why:** when a link fails, the router that lost it may learn a "route" to the same destination from a
neighbour — a route whose actual path goes back through the failed router itself. Neither can see the loop,
because distance vector carries distances and not paths (C104). Each update increments the metric by one, so
the count crawls upward towards infinity while packets loop.

**📘 CONCEPT — C106 · Count-to-infinity, and the three partial remedies**
> | Remedy | Mechanism |
> |---|---|
> | **Defining a small infinity** | RIP's 16 bounds how long the climb takes (C105) |
> | **Split horizon** | never advertise a route back out of the interface it was learned on |
> | **Poison reverse** | advertise such a route back with metric = infinity, explicitly |
> | Triggered updates | send immediately on a change rather than waiting for the timer |
>
> **Applies when** the stem describes slow convergence, a routing loop after a failure, or names split horizon.
>
> **Boundary:** these are **mitigations, not cures** — split horizon prevents the simple two-router loop but
> not loops involving three or more routers. The structural fix is to carry **path** information rather than
> just distance, which is what **link-state** protocols do implicitly and what **BGP's AS-path** does
> explicitly (Q172), letting a router discard any route it appears in.

**Wrong traces:** A = the field size is not the cause; the semantics of "infinity" are · B = flooding belongs
to link-state protocols, which do not suffer this problem · D = also a link-state concern.

---

**Q172.** **BGP** is best described as `[Core]`

- **A)** an interior gateway protocol using link-state flooding within an autonomous system
- **B)** an exterior gateway protocol that exchanges path information between autonomous systems
- **C)** a distance-vector protocol limited to 15 hops
- **D)** a protocol for resolving IP addresses to MAC addresses

**Answer: B) an exterior gateway protocol that exchanges path information between autonomous systems**

**Trace / Why:** BGP routes **between** autonomous systems — it is the protocol that holds the Internet
together. It is a **path-vector** protocol: an advertisement carries the full list of ASes the route traverses
(the **AS-path**), so a router can detect a loop immediately by finding its own AS in the path.

**📘 CONCEPT — C107 · Interior vs exterior gateway protocols**
> | | Interior (IGP) | Exterior (EGP) |
> |---|---|---|
> | Scope | **within** one AS | **between** ASes |
> | Chooses on | technical metrics (hops, cost) | **policy** — business relationships |
> | Examples | **RIP, OSPF, IS-IS, EIGRP** | **BGP** |
>
> **Applies when** the stem mentions autonomous systems, AS-paths, or inter-domain routing.
>
> **Boundary:** the deepest difference is that BGP selects routes by **policy rather than by shortest path** —
> an ISP may prefer a longer AS-path because it is cheaper or contractually required, which no IGP would ever
> do. And the AS-path solves structurally what split horizon only patches (C106): loop detection is exact,
> because the full path is visible.

**Wrong traces:** A = describes OSPF · C = describes RIP · D = ARP (C102).

---

**Q173.** The **token bucket** algorithm differs from the **leaky bucket** algorithm in that token bucket
`[Applied]`

- **A)** discards all packets that arrive when the bucket is full
- **B)** permits saved-up credit to be spent in a burst, whereas leaky bucket enforces a strictly constant output rate
- **C)** operates at the data link layer rather than the network layer
- **D)** requires each packet to carry a token in its header

**Answer: B) permits saved-up credit to be spent in a burst, whereas leaky bucket enforces a strictly constant output rate**

**Trace / Why:** the **leaky bucket** releases traffic at a fixed rate regardless of what arrives, smoothing
bursts completely — but a source that was idle gains nothing from its silence. The **token bucket**
accumulates tokens at a fixed rate up to a maximum; a source that has been quiet has tokens saved and may
transmit a burst, after which it is limited to the token arrival rate.

**📘 CONCEPT — C108 · Traffic shaping: smoothing versus regulated burstiness**
> | | Leaky bucket | Token bucket |
> |---|---|---|
> | Output rate | **strictly constant** | average limited; **bursts permitted** |
> | Idle periods | credit is **lost** | credit is **saved as tokens** |
> | Effect | complete smoothing | burst tolerance up to the bucket size |
>
> **Applies when** the stem describes rate limiting, shaping, or bursts.
>
> **Boundary:** token bucket's burst tolerance is what makes it the practical choice, because real traffic is
> bursty (C24) and a strictly constant rate would add needless delay to a short transfer. The bucket **depth**
> sets the maximum burst and the **token rate** sets the sustained average, so the two parameters control the
> two properties independently — which is exactly why token bucket is used in QoS policing.

**Wrong traces:** A = both algorithms may discard on overflow; that is not the distinction · C = both are
network-layer traffic-shaping mechanisms · D = tokens are counters at the shaper, never carried in packets.

---

**Q174.** Which is a genuine improvement of the **IPv6** header over the IPv4 header? `[Applied]`

- **A)** it adds a header checksum for stronger error detection
- **B)** it uses a variable-length header to save space on small packets
- **C)** it includes a field allowing routers to fragment packets more efficiently
- **D)** it has a fixed 40-byte length, no checksum, and no router fragmentation, so routers process it faster

**Answer: D) it has a fixed 40-byte length, no checksum, and no router fragmentation, so routers process it faster**

**Trace / Why:** three deliberate simplifications speed up forwarding. The header is a **fixed 40 bytes** with
no options field (extensions are chained separately), so parsing needs no length arithmetic. The **checksum is
removed**, since the data link layer already has a CRC and the transport layer has its own checksum — so
routers need not recompute it at every hop. And **routers never fragment**: the source must discover the path
MTU, which removes per-hop fragmentation work entirely.

**📘 CONCEPT — C109 · IPv4 vs IPv6 headers, and why each change was made**
> | | IPv4 | IPv6 |
> |---|---|---|
> | Address | 32 bits | **128 bits** |
> | Header length | **variable**, 20–60 bytes | **fixed 40 bytes** |
> | Header checksum | present — recomputed at every hop | **removed** |
> | Fragmentation | by source **or routers** | **source only** |
> | Options | in the header | **extension headers** |
> | Broadcast | yes | **none** — multicast instead |
>
> **Applies when** the stem contrasts the two headers, or asks why IPv6 forwarding is cheaper.
>
> **Boundary:** IPv6's header is **larger** (40 bytes against a typical 20) yet **faster to process**, because
> the cost that mattered was per-hop computation rather than bytes on the wire — removing the checksum and the
> variable-length parsing saved more than the extra addressing bytes cost. Note the consequence of dropping
> router fragmentation: **path-MTU discovery becomes mandatory**, so an IPv6 network that blocks the relevant
> ICMPv6 messages breaks in ways IPv4 would not.

**Wrong traces:** A = IPv6 **removes** the checksum · B = the header is **fixed**, not variable · C = routers
do **not** fragment in IPv6 at all.

---
## Section 9 — Transport Layer: TCP & UDP

*With the data link layer, the joint highest GATE yield in the chapter.*

**Q175.** The essential difference between **TCP** and **UDP** is that TCP `[Core]` `[Asked: BCS / Bank IT]`

- **A)** operates at the network layer while UDP operates at the transport layer
- **B)** uses IP addresses while UDP uses port numbers
- **C)** is faster because it has a smaller header
- **D)** is connection-oriented and reliable, whereas UDP is connectionless and unreliable

**Answer: D) is connection-oriented and reliable, whereas UDP is connectionless and unreliable**

**Trace / Why:** TCP establishes a connection, numbers every byte, acknowledges what arrives, retransmits
what does not, reorders what arrives out of sequence, and controls both flow and congestion. UDP does none of
that — it simply adds ports and a checksum to a datagram and hands it to IP.

**📘 CONCEPT — C110 · TCP vs UDP, on every property that gets examined**
> | | TCP | UDP |
> |---|---|---|
> | Connection | **connection-oriented** (handshake) | **connectionless** |
> | Reliability | **guaranteed** — ACKs and retransmission | **none** |
> | Ordering | **preserved** | not preserved |
> | Flow control | **yes** (receiver window) | no |
> | Congestion control | **yes** | **no** |
> | Header size | **20 bytes** minimum | **8 bytes** |
> | Speed / overhead | slower, heavier | **faster, lighter** |
> | Suits | file transfer, web, e-mail | streaming, DNS, VoIP, gaming |
>
> **Applies when** the stem contrasts the two, or names an application and asks which protocol suits it.
>
> **Boundary:** UDP's lack of guarantees is a **feature** for real-time traffic — a retransmitted video frame
> arrives too late to be displayed, so TCP's reliability would add delay for no benefit. So "TCP is better" is
> wrong; the application's tolerance for loss versus delay decides (Q176).

**Wrong traces:** A = both are transport-layer protocols · B = **both** use port numbers, and neither assigns
IP addresses · C = TCP's header is **larger** (20 bytes against 8), and the size is not the essential
difference.

---

**Q176.** Which application is best suited to **UDP** rather than TCP? `[Applied]`

- **A)** transferring a large file where every byte must arrive intact
- **B)** retrieving a web page over HTTP
- **C)** sending an e-mail message via SMTP
- **D)** live voice-over-IP conversation

**Answer: D) live voice-over-IP conversation**

**Trace / Why:** in a live conversation a packet that arrives late is **useless** — the moment it belonged to
has passed. TCP would retransmit it, adding delay and disrupting the playout for no benefit. UDP simply drops
it, and the codec conceals the tiny gap. Low, predictable delay matters more than completeness.

**📘 CONCEPT — C110 (see Concept Index)** › The decision rule is a single question: **is a late packet worth
more or less than no packet?** For a file transfer, a late byte is still essential — use TCP. For live audio
or video, a late packet is worthless and its retransmission actively harmful — use UDP. **DNS** uses UDP for
a different reason: a query and reply fit in one small datagram each, so a TCP handshake would triple the
exchange's cost, and losing a query is cheaply fixed by simply asking again.

**Wrong traces:** A, B and C = all require complete, ordered delivery, which is precisely what TCP provides.

---

**Q177.** TCP establishes a connection using a **three-way handshake** consisting of `[Core]`

- **A)** SYN, then ACK, then FIN
- **B)** SYN, then SYN, then SYN-ACK
- **C)** ACK, then SYN, then SYN-ACK
- **D)** SYN, then SYN-ACK, then ACK

**Answer: D) SYN, then SYN-ACK, then ACK**

**Trace / Why:**

```
client → server :  SYN      (seq = x)                        "I want to connect; my ISN is x"
server → client :  SYN+ACK  (seq = y, ack = x + 1)           "Accepted; my ISN is y; I got yours"
client → server :  ACK      (ack = y + 1)                    "I got yours too"
```

After the third message both sides know that the other is ready **and** that their own initial sequence
number has been received — which is why two messages would not suffice (Q178).

**📘 CONCEPT — C111 · The handshake synchronises sequence numbers in both directions**
> Its purpose is not merely to say hello: each side must learn and confirm the other's **initial sequence
> number**, because TCP's reliability machinery is built on byte numbering. The connection is full-duplex, so
> **two** ISNs must be exchanged — hence three messages, with the middle one doing double duty.
>
> **Applies when** the stem asks the order or purpose of the handshake, or mentions SYN, SYN-ACK or ISNs.
>
> **Boundary:** the ACK number is always the received sequence number **plus one**, meaning "this is the next
> byte I expect" — the acknowledgement is *cumulative* and forward-looking, not a receipt for a specific
> segment. Note that a flood of SYNs with no third message is the **SYN flood** denial-of-service attack,
> which exploits the server having to hold state after the second message.

**Wrong traces:** A = FIN belongs to teardown, not setup · B = three SYNs with no ordering · C = an ACK
cannot precede the SYN it acknowledges.

---

**Q178.** A **two-way** handshake (SYN, then SYN-ACK) would be insufficient because `[Applied]`

- **A)** the initiating side would not know that its own sequence number had been received
- **B)** the server could not learn the client's IP address
- **C)** port numbers could not be negotiated in two messages
- **D)** the checksum cannot be verified until three segments have been exchanged

**Answer: A) the initiating side would not know that its own sequence number had been received**

**Trace / Why:** after two messages the **client** knows the server is alive and has its ISN — but the
**server** has no confirmation that its own SYN-ACK arrived. If that message were lost, the server would
believe the connection was open while the client had never seen it. The third message closes the loop, making
both sides certain.

**📘 CONCEPT — C111 (see Concept Index)** › This is the **two-army problem** in miniature: confirming mutual
agreement over an unreliable channel needs each side's information to be acknowledged, and a full-duplex
connection has two directions to confirm. Three messages is the **minimum** that achieves it, which is why the
handshake is three-way and not two or four. The same reasoning is why a *delayed duplicate* old SYN cannot
open a phantom connection — the third message would carry a sequence number the client never chose.

**Wrong traces:** B = the IP address arrives in the first packet's header · C = ports are stated, never
negotiated · D = checksums are per-segment and independent of the handshake.

---

**Q179.** Terminating a TCP connection normally requires `[Core]`

- **A)** a single RST segment from either side
- **B)** a three-way exchange identical to setup
- **C)** a four-way exchange, because each direction is closed independently
- **D)** no explicit exchange; the connection times out

**Answer: C) a four-way exchange, because each direction is closed independently**

**Trace / Why:** a TCP connection is two independent byte streams, so each is shut down separately.

```
A → B :  FIN        "I have no more data to send"
B → A :  ACK        "acknowledged"
B → A :  FIN        "I have no more data either"
A → B :  ACK        "acknowledged"
```

Between the second and third messages the connection is **half-closed** — A cannot send but B still can,
which is legitimate and occasionally useful.

**📘 CONCEPT — C112 · Setup is symmetric, teardown is not**
> Setup needs three messages because the two ISNs can be combined in one middle segment. Teardown needs
> **four** because each side decides independently when it has finished sending, and those moments need not
> coincide. The closing side then waits in **TIME-WAIT** (typically 2 × MSL) so that a delayed duplicate
> segment cannot be mistaken for part of a new connection on the same port pair.
>
> **Applies when** the stem asks the number of segments to close, mentions FIN, half-close or TIME-WAIT.
>
> **Boundary:** an **RST** does close a connection in one segment, but it is an **abort** — it discards any
> unsent data and reports an error, so it is not the normal graceful teardown that option A implies. The
> distinction between graceful close (FIN) and abort (RST) is examinable.

**Wrong traces:** A = an abort, not a graceful close · B = three messages suffice only for setup ·
D = TCP closes explicitly; timeouts are a failure path.

---

**Q180.** A TCP **sequence number** identifies `[Core]`

- **A)** the position of the segment within the connection, counting segments
- **B)** the position of the segment's first data byte within the connection's byte stream
- **C)** the port number of the sending process
- **D)** the number of segments still unacknowledged

**Answer: B) the position of the segment's first data byte within the connection's byte stream**

**Trace / Why:** TCP numbers **bytes**, not segments. A segment's sequence number is the stream position of
its first byte, so a 500-byte segment starting at 1,000 occupies bytes 1,000–1,499 and the next segment starts
at 1,500. Byte numbering is what lets TCP retransmit a partial segment or coalesce data differently on a
retransmission.

**📘 CONCEPT — C113 · TCP numbers bytes, and acknowledgements are cumulative**
> - **Sequence number** — stream position of this segment's first data byte.
> - **Acknowledgement number** — the **next** byte expected, so it acknowledges everything before it.
> - The ACK is therefore **cumulative**: ack = 1,500 confirms every byte up to 1,499, whatever segments they
>   arrived in.
>
> **Applies when** the stem gives a sequence number and a segment length and asks for the next value.
>
> **Boundary:** because acknowledgements are cumulative, a **single lost segment** stalls confirmation of
> everything after it, even if those later bytes arrived — the receiver can only keep repeating the old ACK.
> That is exactly what generates the **duplicate ACKs** that trigger fast retransmit (Q185), and it is why
> **selective acknowledgement (SACK)** was later added as an option to report the gaps explicitly.

**Wrong traces:** A = TCP counts bytes, not segments — the commonest misconception · C = ports are separate
header fields · D = that is a sender-side window quantity, not carried as the sequence number.

---

**Q181.** TCP **flow control** prevents a fast sender from overwhelming a slow receiver by means of
`[Applied]`

- **A)** the congestion window, computed from observed packet loss
- **B)** the TTL field, limiting how long data may remain in the network
- **C)** the slow-start threshold, halved after every loss
- **D)** the receive window advertised by the receiver in every acknowledgement

**Answer: D) the receive window advertised by the receiver in every acknowledgement**

**Trace / Why:** the receiver advertises, in the 16-bit **Window** field of every segment it sends, how much
free buffer space it currently has. The sender may have at most that many unacknowledged bytes outstanding. If
the receiver's application is slow to read, the window shrinks; if it fills completely the window reaches
**zero** and the sender must stop.

**📘 CONCEPT — C114 · Flow control protects the receiver; congestion control protects the network**
> | | Flow control | Congestion control |
> |---|---|---|
> | Protects | the **receiver's buffer** | the **network's** capacity |
> | Driven by | the receiver's advertised window | the sender's inference from **loss** |
> | Mechanism | **rwnd** in every ACK | **cwnd**, slow start and AIMD (C115) |
> | Signal | explicit | **implicit** — loss is the only feedback |
>
> The sender may send at most **min(rwnd, cwnd)** unacknowledged bytes.
>
> **Applies when** the stem describes a slow receiver (flow control) or a congested network (congestion
> control).
>
> **Boundary:** the two are constantly confused, and the discriminator is **who is being protected**. Note
> that the 16-bit window field caps rwnd at 65,535 bytes, which is far too small for a modern high
> bandwidth-delay path (C7) — hence the **window-scaling** option, which multiplies it by a negotiated power
> of two.

**Wrong traces:** A and C = congestion-control mechanisms, protecting the network rather than the receiver ·
B = TTL is a network-layer loop guard (C99).

---

**Q182.** A TCP connection begins in **slow start** with a congestion window of 1 MSS. Assuming every
acknowledgement arrives and no loss occurs, the congestion window after **3** round-trip times is
`[Applied]` `[Asked: GATE-style]`

- **A)** 4 MSS
- **B)** 8 MSS
- **C)** 16 MSS
- **D)** 32 MSS

**Answer: B) 8 MSS**

**Trace / Why:** in slow start the window **doubles** every round trip, because each acknowledged segment
increases cwnd by 1 MSS.

| After | cwnd |
|---|---|
| start | 1 MSS |
| 1 RTT | 2 MSS |
| 2 RTTs | 4 MSS |
| **3 RTTs** | **8 MSS** |

So cwnd = 2ⁿ after n round trips — exponential growth, despite the name "slow start".

**📘 CONCEPT — C115 · TCP congestion control: exponential, then linear, then halve**
> | Phase | cwnd behaviour | Ends when |
> |---|---|---|
> | **Slow start** | **doubles** each RTT (×2, exponential) | cwnd reaches **ssthresh**, or loss occurs |
> | **Congestion avoidance** | **+1 MSS** per RTT (linear, additive increase) | loss occurs |
> | **On timeout** | ssthresh = cwnd/2; **cwnd = 1**; slow start again | — |
> | **On 3 duplicate ACKs** | ssthresh = cwnd/2; **cwnd = ssthresh**; fast recovery | — |
>
> **Applies when** the stem gives an initial window and a number of round trips, or a loss event.
>
> **Boundary:** "slow start" is named for its **starting point** (one segment), not its rate — the growth is
> exponential and is the fastest phase TCP has. The name misleads reliably enough that it is worth stating:
> slow start is the *aggressive* phase, congestion avoidance the cautious one.

**Wrong traces:** A = the value after **2** RTTs · C = after 4 RTTs · D = after 5 RTTs.

---

**Q183.** In the **congestion avoidance** phase, TCP increases its congestion window by `[Core]`

- **A)** doubling it every round-trip time
- **B)** one MSS for every acknowledgement received
- **C)** approximately one MSS per round-trip time
- **D)** half the current window every round-trip time

**Answer: C) approximately one MSS per round-trip time**

**Trace / Why:** once cwnd reaches **ssthresh**, TCP switches from exponential to **linear** growth: cwnd
rises by roughly one MSS per RTT, regardless of how many acknowledgements arrive in that time. The idea is to
probe cautiously for extra capacity near the point where loss last occurred.

**📘 CONCEPT — C115 (see Concept Index)** › The two phases exist for different purposes: slow start's job is
to **find the rough operating point quickly** from a standing start, and congestion avoidance's job is to
**probe gently** once close to it. Note the contrast with option B — increasing by 1 MSS per *acknowledgement*
is exactly the **slow start** rule, and since a window of N generates N acknowledgements per RTT, that rule
produces doubling. Per-ACK versus per-RTT is the entire difference between exponential and linear growth.

**Wrong traces:** A = **slow start** · B = the slow-start rule, which yields doubling · D = the reduction
applied on loss, not an increase.

---

**Q184.** TCP's congestion control is described as **AIMD** because it `[Applied]`

- **A)** increases the window additively and decreases it additively
- **B)** increases the window additively and decreases it multiplicatively on detecting loss
- **C)** increases the window multiplicatively and decreases it additively
- **D)** keeps the window constant and adjusts the transmission rate instead

**Answer: B) increases the window additively and decreases it multiplicatively on detecting loss**

**Trace / Why:** **A**dditive **I**ncrease — cwnd rises by 1 MSS per RTT while all is well (C115).
**M**ultiplicative **D**ecrease — on loss, ssthresh is **halved**. Cautious growth, decisive retreat.

**📘 CONCEPT — C116 · AIMD is asymmetric on purpose, and that asymmetry is what makes TCP stable and fair**
> Congestion is dangerous and under-utilisation merely wasteful, so the response to danger must be far faster
> than the exploration of spare capacity. Halving on loss reacts immediately; adding one MSS per RTT probes
> slowly. Two consequences:
> - **stability** — the aggregate rate cannot run away, since every flow retreats sharply on congestion;
> - **fairness** — flows sharing a bottleneck converge towards equal shares, because the multiplicative cut
>   takes proportionally more from the larger flow.
>
> **Applies when** the stem names AIMD, asks about TCP fairness, or asks why the responses are asymmetric.
>
> **Boundary:** AIMD's fairness holds only among **TCP-like** flows. **UDP has no congestion control at all**
> (C110), so a UDP flood does not retreat and can crowd out TCP traffic entirely — which is why unresponsive
> traffic is policed at the network edge rather than trusted to behave.

**Wrong traces:** A = a purely additive decrease would react far too slowly to congestion · C = exactly
inverted, and would be unstable · D = TCP controls the rate precisely *by* adjusting the window.

---

**Q185.** TCP's **fast retransmit** is triggered when the sender receives `[Applied]`

- **A)** a single duplicate acknowledgement
- **B)** an explicit negative acknowledgement from the receiver
- **C)** an ICMP Source Quench message
- **D)** three duplicate acknowledgements for the same sequence number

**Answer: D) three duplicate acknowledgements for the same sequence number**

**Trace / Why:** because acknowledgements are cumulative (C113), a receiver missing one segment keeps
re-sending the **same** ACK as later segments arrive. One or two duplicates might merely reflect reordering,
but **three** strongly indicate loss — so the sender retransmits the missing segment at once rather than
waiting for the retransmission timer, which would idle the connection for far longer.

**📘 CONCEPT — C117 · Two loss signals, with two different severities of response**
> | Signal | Interpretation | Response |
> |---|---|---|
> | **Timeout** | severe congestion; nothing is getting through | ssthresh = cwnd/2, **cwnd = 1**, slow start |
> | **3 duplicate ACKs** | one segment lost, but data is still flowing | ssthresh = cwnd/2, **cwnd = ssthresh**, fast recovery |
>
> **Applies when** the stem describes duplicate ACKs, a timeout, or asks how TCP reacts to each.
>
> **Boundary:** the asymmetry is the examinable point — duplicate ACKs are **good news alongside bad**, since
> their arrival proves later segments are still getting through, so TCP need not collapse its window to 1.
> A timeout carries no such evidence and triggers the full retreat. The threshold of **three** is a
> deliberate compromise: fewer would react to ordinary reordering, more would delay recovery.

**Wrong traces:** A = a single duplicate is usually just reordering · B = standard TCP has no NAK; SACK
reports gaps but does not trigger this · C = Source Quench is obsolete and was never TCP's mechanism.

---

**Q186.** Which port number is correctly paired with its service? `[Applied]` `[Asked: BCS / Bank IT]`

- **A)** 20 — SSH
- **B)** 25 — DNS
- **C)** 53 — SMTP
- **D)** 80 — HTTP

**Answer: D) 80 — HTTP**

**Trace / Why:** HTTP is the classic well-known port 80. The distractors each pair a real port with the wrong
service: 20 is FTP's data connection (SSH is 22), 25 is SMTP (DNS is 53), and 53 is DNS (SMTP is 25).

**📘 CONCEPT — C118 · The well-known ports that appear in exams**
> | Port | Service | | Port | Service |
> |---|---|---|---|---|
> | **20 / 21** | FTP data / control | | **80** | HTTP |
> | **22** | SSH | | **110** | POP3 |
> | **23** | TELNET | | **143** | IMAP |
> | **25** | SMTP | | **161** | SNMP |
> | **53** | DNS | | **443** | HTTPS |
> | **67 / 68** | DHCP server / client | | **3389** | RDP |
>
> **Applies when** the stem gives a port or a service and asks for the other.
>
> **Boundary:** the numbers cluster deceptively — **22, 23, 25** are SSH, TELNET and SMTP in sequence, and
> mixing them is the commonest error. Note that **FTP uses two ports** (21 for commands, 20 for data), which
> is unusual and frequently asked, and that these are all in the **0–1023 well-known** range reserved for
> servers (C41).

**Wrong traces:** A = 20 is FTP data; SSH is 22 · B = 25 is SMTP; DNS is 53 · C = 53 is DNS; SMTP is 25.

---

**Q187.** **Nagle's algorithm** addresses which problem? `[Trap]`

- **A)** packets arriving out of order at the receiver
- **B)** the receiver's buffer filling faster than the application can read
- **C)** a sender generating many tiny segments, each with 40 bytes of header for a few bytes of data
- **D)** routers dropping packets when their queues overflow

**Answer: C) a sender generating many tiny segments, each with 40 bytes of header for a few bytes of data**

**Trace / Why:** an interactive application such as TELNET may hand TCP **one byte** at a time. Sending each
immediately means a 41-byte packet per keystroke — 40 bytes of TCP and IP header for 1 byte of data. Nagle's
rule is to hold small amounts of data until either the outstanding data is acknowledged or a full-sized
segment has accumulated, so the small pieces coalesce.

**📘 CONCEPT — C119 · Two "silly window" problems, one at each end**
> | Problem | Caused by | Fixed by |
> |---|---|---|
> | Tiny segments sent | the **sender** trickling out small writes | **Nagle's algorithm** |
> | Tiny windows advertised | the **receiver** freeing buffer a byte at a time | **Clark's solution** — delay the window update until a useful amount is free |
>
> Together these are the **silly window syndrome**: an exchange technically working while wasting almost all
> its capacity on headers.
>
> **Applies when** the stem describes tiny segments, header overhead dominating, or interactive traffic.
>
> **Boundary:** Nagle **conflicts with interactivity**, which is the practical catch: coalescing adds delay,
> so latency-sensitive applications disable it (`TCP_NODELAY`). Combined with **delayed ACKs** (C80) it can
> even produce a mutual stall, each side waiting for the other — a classic real-world pathology, and the
> reason Nagle is off by default in many modern libraries.

**Wrong traces:** A = handled by sequence numbers (C113) · B = flow control's silly-window counterpart, fixed
by Clark's solution rather than Nagle's · D = congestion, handled by AIMD (C116).

---

**Q188.** A TCP connection is uniquely identified by `[Applied]`

- **A)** the destination port number alone
- **B)** the pair of IP addresses only
- **C)** the four-tuple of source IP, source port, destination IP and destination port
- **D)** the initial sequence number chosen by the client

**Answer: C) the four-tuple of source IP, source port, destination IP and destination port**

**Trace / Why:** a server listening on port 80 may serve thousands of simultaneous connections. They are
distinguished because each client contributes a **different source IP or source port**, so the four-tuple
differs even though three of its components may be shared.

**📘 CONCEPT — C41 (see Part 1 Concept Index)** › This is why **demultiplexing** works: an arriving segment is
matched against the four-tuple to find its connection, so one well-known port can serve unlimited clients. The
contrast with **UDP** is examinable — a UDP socket is identified by the **two-tuple** ⟨destination IP,
destination port⟩ only, because UDP is connectionless and keeps no per-peer state, which is precisely why a
single UDP socket receives datagrams from every sender.

**Wrong traces:** A = the destination port is shared by every client of that service · B = one client can hold
several connections to one server, differing only in source port · D = the ISN is used for byte numbering, not
for identification.

---
## Section 10 — Application Layer & Network Security

**Q189.** The purpose of **DNS** is to `[Core]`

- **A)** assign IP addresses dynamically to hosts as they join a network
- **B)** translate human-readable domain names into IP addresses
- **C)** transfer files between a client and a server
- **D)** encrypt web traffic between a browser and a server

**Answer: B) translate human-readable domain names into IP addresses**

**Trace / Why:** people use names such as `www.example.com`; IP routing needs `93.184.216.34`. DNS is the
distributed database that maps between them, queried over **UDP port 53** for ordinary lookups.

**📘 CONCEPT — C120 · DNS: a distributed hierarchy, queried iteratively or recursively**
> The name space is a tree — **root** → **top-level domains** (.com, .org, .bd) → second level → subdomains —
> and no single server holds it all. Two query styles:
> - **Recursive** — the client asks its local resolver to obtain the final answer and do all the work.
> - **Iterative** — a server answers with a **referral** to the next server down, and the asker follows it.
>
> Common record types: **A** (IPv4 address), **AAAA** (IPv6), **CNAME** (alias), **MX** (mail exchanger),
> **NS** (name server), **PTR** (reverse lookup).
>
> **Applies when** the stem mentions name resolution, record types, root servers, or port 53.
>
> **Boundary:** typically the client→resolver query is **recursive** while the resolver→server queries are
> **iterative** — so both styles occur in one lookup, and questions asking "which does DNS use" are
> under-specified unless they say which leg. **Caching** at every level is what makes the system viable, and
> the **TTL** on each record controls how long an answer may be reused.

**Wrong traces:** A = DHCP (Q194) · C = FTP or HTTP · D = TLS/HTTPS.

---

**Q190.** In the DNS hierarchy, if a local resolver holds no cached answer, it begins resolution by querying
`[Applied]`

- **A)** the destination web server directly
- **B)** every DNS server on the Internet simultaneously
- **C)** a root name server, which refers it to the appropriate top-level domain server
- **D)** the client's default gateway

**Answer: C) a root name server, which refers it to the appropriate top-level domain server**

**Trace / Why:** resolution proceeds **down** the tree. The root server does not know the answer but knows
which TLD server is authoritative for `.com`; that server refers the resolver to the authoritative server for
`example.com`, which finally supplies the A record.

**📘 CONCEPT — C120 (see Concept Index)** › The design point is that **no server needs global knowledge** —
each knows only its own zone plus pointers to its children, which is what allows the name space to scale to
billions of names and to be administered independently by millions of organisations. The **13 root server
addresses** (each in fact many machines via anycast) are the only information a resolver must be configured
with in advance, and every other server is discovered by referral.

**Wrong traces:** A = the web server does not run the DNS zone and could not be found without DNS anyway ·
B = no flooding occurs; that would not scale · D = the gateway routes packets and is not a DNS authority.

---

**Q191.** **HTTP** is described as a *stateless* protocol, which means `[Core]`

- **A)** it cannot transfer binary data such as images
- **B)** it requires a new TCP connection for every object on a page
- **C)** the server retains no memory of previous requests from the same client
- **D)** it operates without any transport-layer protocol beneath it

**Answer: C) the server retains no memory of previous requests from the same client**

**Trace / Why:** each HTTP request is self-contained and the server treats it independently. That keeps
servers simple and enormously scalable — any server in a farm can handle any request — but means the protocol
itself cannot recognise a returning visitor or track a shopping basket.

**📘 CONCEPT — C121 · HTTP is stateless, so state is added above it by cookies**
> A **cookie** is a small value the server sends in a response header; the browser stores it and returns it
> with subsequent requests, letting the server correlate them into a session. Sessions, logins and baskets are
> all built this way.
>
> Methods: **GET** (retrieve), **POST** (submit), **PUT**, **DELETE**, **HEAD** (headers only).
> Status classes: **2xx** success, **3xx** redirection, **4xx** client error (404 Not Found), **5xx** server
> error.
>
> **Applies when** the stem mentions statelessness, cookies, sessions, methods or status codes.
>
> **Boundary:** statelessness is **not** the same as connectionlessness — HTTP runs over **TCP**, which is very
> much connection-oriented, and **HTTP/1.1 persistent connections** reuse one TCP connection for many requests
> (which is what option B describes and what HTTP/1.0 did). So a persistent connection is still stateless: the
> *transport* remembers, the *application* does not.

**Wrong traces:** A = HTTP carries any content type · B = describes **non-persistent** connections, a
different property · D = HTTP depends on TCP.

---

**Q192.** **FTP** is unusual among application protocols in that it `[Applied]`

- **A)** operates directly over IP with no transport-layer protocol
- **B)** encrypts all transfers by default
- **C)** uses two separate TCP connections — one for control commands and one for data
- **D)** transfers files without requiring authentication

**Answer: C) uses two separate TCP connections — one for control commands and one for data**

**Trace / Why:** FTP keeps a **control** connection on **port 21** open for the whole session, carrying
commands and replies, and opens a separate **data** connection on **port 20** (in active mode) for each file
transfer or listing. Separating them means control commands — including *abort* — remain usable while a large
transfer is in progress.

**📘 CONCEPT — C122 · FTP's two connections, and the active/passive distinction that follows**
> | Connection | Port | Lifetime |
> |---|---|---|
> | **Control** | **21** | the whole session |
> | **Data** | **20** (active mode) | one per transfer |
>
> - **Active mode** — the **server** opens the data connection back to the client. Firewalls and NAT usually
>   block this inbound connection.
> - **Passive mode** — the **client** opens both connections, which is why passive FTP works through NAT and
>   is now the default.
>
> **Applies when** the stem mentions FTP ports, active versus passive mode, or FTP failing through a firewall.
>
> **Boundary:** FTP transmits **credentials and data in clear text**, so it is insecure — **SFTP** (over SSH)
> and **FTPS** (over TLS) replace it. Option B is therefore the exact opposite of the truth, and the security
> weakness is as examinable as the two-connection design.

**Wrong traces:** A = FTP relies on TCP · B = FTP is plaintext, which is its chief weakness · D = FTP
authenticates with a username and password (anonymous FTP is a configured special case).

---

**Q193.** In the e-mail architecture, **SMTP** is used to `[Core]`

- **A)** retrieve messages from a mailbox to a client, deleting them from the server
- **B)** synchronise a mailbox across several client devices
- **C)** push messages from the sender to the recipient's mail server
- **D)** encode binary attachments as ASCII text

**Answer: C) push messages from the sender to the recipient's mail server**

**Trace / Why:** SMTP (port 25) is a **push** protocol — it moves a message *toward* its destination, from
client to server and between servers. Getting a message *out of* the destination mailbox is a **pull**
operation and needs a different protocol: POP3 or IMAP.

**📘 CONCEPT — C123 · The three mail protocols split by direction and by where mail is stored**
> | Protocol | Port | Direction | Storage |
> |---|---|---|---|
> | **SMTP** | 25 | **push** — sending and relaying | in transit |
> | **POP3** | 110 | **pull** | downloads and typically **deletes** from the server |
> | **IMAP** | 143 | **pull** | **keeps mail on the server**, syncing folders and flags |
>
> **MIME** is not a transport protocol at all — it is the encoding that lets SMTP, a 7-bit ASCII protocol,
> carry binary attachments and non-English character sets.
>
> **Applies when** the stem names a mail protocol, a port, or describes access from several devices.
>
> **Boundary:** the practical discriminator between POP3 and IMAP is **multiple devices** — POP3's
> download-and-delete model leaves each client with a different partial view, while IMAP keeps the
> authoritative copy on the server so phone and laptop agree. That is why IMAP (and its webmail equivalents)
> displaced POP3.

**Wrong traces:** A = POP3 · B = IMAP · D = MIME, an encoding rather than a protocol.

---

**Q194.** The **DHCP** address-assignment exchange follows which sequence? `[Applied]`

- **A)** Request, Offer, Discover, Acknowledge
- **B)** Offer, Discover, Acknowledge, Request
- **C)** Discover, Offer, Request, Acknowledge
- **D)** Discover, Acknowledge, Offer, Request

**Answer: C) Discover, Offer, Request, Acknowledge**

**Trace / Why:** the mnemonic is **DORA**.

```
client → broadcast :  DHCPDISCOVER   "is there a DHCP server?"
server → client    :  DHCPOFFER      "you may have 192.168.1.50"
client → broadcast :  DHCPREQUEST    "I accept that offer"
server → client    :  DHCPACK        "confirmed, here is your lease"
```

The client broadcasts because it has **no address yet**, so it uses source 0.0.0.0 and destination
255.255.255.255 (C15, C16).

**📘 CONCEPT — C124 · DHCP: DORA, leases, and why the request is broadcast too**
> Ports **67** (server) and **68** (client). Addresses are issued as **leases** with an expiry, and the client
> renews at roughly half the lease time. If no server answers, the client self-assigns an **APIPA** address in
> 169.254.0.0/16 — the diagnostic signal covered in the Subnetting chapter.
>
> **Applies when** the stem mentions automatic address assignment, DORA, leases, or a host that failed to get
> an address.
>
> **Boundary:** the third message is **broadcast**, not unicast, which surprises students — the client must
> tell **all** offering servers which offer it accepted, so the others can release their reservations. Note
> that DHCP supplies more than an address: the **mask, default gateway and DNS servers** come with it, which is
> why a DHCP failure breaks name resolution and routing as well as addressing.

**Wrong traces:** A, B and D = each permutes DORA; the discovery must come first and the acknowledgement last.

---

**Q195.** **SSH** is preferred over **TELNET** for remote administration because SSH `[Core]`

- **A)** encrypts the entire session, including the password, whereas TELNET sends everything in clear text
- **B)** requires no authentication, simplifying access
- **C)** uses UDP, making it faster over long distances
- **D)** operates at the transport layer rather than the application layer

**Answer: A) encrypts the entire session, including the password, whereas TELNET sends everything in clear text**

**Trace / Why:** TELNET (port 23) transmits keystrokes and credentials unencrypted, so anyone able to capture
traffic reads the administrator's password directly. SSH (port 22) provides encryption, server authentication
by host key, and integrity protection — so the same session is unreadable and untamperable in transit.

**📘 CONCEPT — C125 · The plaintext protocols and their secure replacements**
> | Insecure | Port | Secure replacement | Port |
> |---|---|---|---|
> | TELNET | 23 | **SSH** | 22 |
> | FTP | 21 | **SFTP** (over SSH) / FTPS | 22 / 990 |
> | HTTP | 80 | **HTTPS** (HTTP over TLS) | **443** |
> | POP3 / IMAP | 110 / 143 | POP3S / IMAPS | 995 / 993 |
>
> **Applies when** the stem contrasts a protocol with its secure form, or asks which is safe on an untrusted
> network.
>
> **Boundary:** the pattern is that the **older protocol is not broken, merely unencrypted** — TELNET works
> perfectly and remains fine on an isolated management network. What changed is the threat model: once traffic
> may be observed, confidentiality becomes a requirement, and the replacements add cryptography without
> altering the underlying function.

**Wrong traces:** B = SSH authenticates strongly, by password or public key · C = SSH uses TCP · D = both are
application-layer protocols.

---

**Q196.** **SNMP** is used to `[Core]`

- **A)** transfer mail between servers
- **B)** resolve host names to addresses
- **C)** assign IP configuration automatically
- **D)** monitor and manage network devices by querying and setting their variables

**Answer: D) monitor and manage network devices by querying and setting their variables**

**Trace / Why:** SNMP (port 161) lets a **manager** station read and write named variables held by an
**agent** running on a router, switch or server. Those variables — interface counters, error rates, uptime —
are defined in a **MIB**, and the agent can also send an unsolicited **trap** when something notable happens.

**📘 CONCEPT — C126 · SNMP's three components and the two directions of communication**
> - **Manager** — the monitoring station that polls and configures.
> - **Agent** — software on the managed device, exposing its variables.
> - **MIB** — the hierarchical dictionary naming those variables.
>
> Operations: **GET** and **GETNEXT** (read), **SET** (write), and **TRAP** — the only message the agent sends
> unprompted.
>
> **Applies when** the stem mentions network management, MIBs, agents, traps, or port 161.
>
> **Boundary:** the **trap** is the exception to SNMP's polling model, and it exists because polling every
> device frequently enough to catch a failure quickly would not scale — so routine data is pulled while urgent
> events are pushed. Note that SNMPv1 and v2c authenticate with a plaintext **community string**, effectively a
> shared password sent in clear, which is why **SNMPv3** added real authentication and encryption.

**Wrong traces:** A = SMTP · B = DNS · C = DHCP.

---

**Q197.** A **packet-filtering** firewall differs from an **application-gateway** firewall in that the
packet filter `[Applied]`

- **A)** inspects the full content of each message and can block specific commands
- **B)** requires a separate proxy process for every application protocol
- **C)** makes decisions from header fields such as addresses, ports and protocol, without examining payload content
- **D)** operates only on encrypted traffic

**Answer: C) makes decisions from header fields such as addresses, ports and protocol, without examining payload content**

**Trace / Why:** a packet filter works at layers 3 and 4, matching source and destination addresses, ports
and protocol against a rule list. It is fast and protocol-agnostic, but it cannot tell a legitimate HTTP
request from a malicious one, because it never looks inside the packet.

**📘 CONCEPT — C127 · Firewall types, in increasing depth of inspection**
> | Type | Layer | Inspects | Cost |
> |---|---|---|---|
> | **Packet filter** | 3–4 | headers only, each packet independently | fastest |
> | **Stateful inspection** | 3–4 + state | headers **plus connection state** | moderate |
> | **Application gateway (proxy)** | **7** | full **content**, per protocol | slowest; needs a proxy per protocol |
>
> **Applies when** the stem describes what a firewall examines, or asks which type blocks a content-based
> threat.
>
> **Boundary:** the crucial advance of **stateful** inspection over plain packet filtering is that it tracks
> connections, so it can permit return traffic for an outbound connection **without** a permanent inbound rule
> — something a stateless filter cannot express safely. And no firewall of any type can inspect the contents of
> **encrypted** traffic without terminating the encryption itself, which is why option D is wrong and why TLS
> interception is a separate and contentious mechanism.

**Wrong traces:** A and B = describe the **application gateway** · D = encryption defeats content inspection
rather than enabling it.

---

**Q198.** A **VPN** provides secure communication over a public network by `[Applied]`

- **A)** leasing a dedicated physical circuit from the carrier
- **B)** assigning private IP addresses to all hosts involved
- **C)** encapsulating and encrypting packets inside other packets, creating a secure tunnel
- **D)** compressing traffic so that it cannot be interpreted by observers

**Answer: C) encapsulating and encrypting packets inside other packets, creating a secure tunnel**

**Trace / Why:** the original packet — including its private addresses — is encrypted and placed inside a new
packet addressed between the two VPN endpoints. Observers see only traffic between those endpoints and cannot
read the payload, so the public Internet is used as if it were a private link.

**📘 CONCEPT — C128 · Tunnelling: the whole packet becomes the payload of another**
> **IPSec** is the standard network-layer mechanism, offering two modes and two protocols:
> - **Transport mode** protects the payload only; **tunnel mode** protects the whole original packet and is
>   what site-to-site VPNs use.
> - **AH** provides authentication and integrity; **ESP** adds **encryption** — so ESP is the one that
>   provides confidentiality.
>
> **TLS/SSL** provides comparable protection above the transport layer, which is how HTTPS and many
> remote-access VPNs work.
>
> **Applies when** the stem mentions tunnels, IPSec, site-to-site connectivity, or secure use of a public
> network.
>
> **Boundary:** a VPN gives the **security** of a private line without its **cost**, but not its guarantees —
> it still traverses the public Internet, so delay, jitter and loss remain whatever the Internet delivers. A
> leased line (option A) buys predictable performance; a VPN buys confidentiality. Confusing the two is the
> substantive error here.

**Wrong traces:** A = a leased line, which is genuinely private but expensive and not a VPN · B = private
addressing alone provides no security · D = compression is not encryption and is trivially reversible.

---

**Q199.** In IEEE 802.11 wireless LANs, **spread-spectrum** techniques such as FHSS and DSSS are used to
`[Trap]`

- **A)** increase the raw bit rate beyond what the Shannon limit permits
- **B)** eliminate the need for a medium-access control protocol
- **C)** encrypt the transmitted data against eavesdroppers
- **D)** spread the signal over a wider band than it needs, improving resistance to narrowband interference and jamming

**Answer: D) spread the signal over a wider band than it needs, improving resistance to narrowband interference and jamming**

**Trace / Why:** spread spectrum deliberately uses **more** bandwidth than the data requires. **FHSS** hops
the carrier among many frequencies in a pattern known to both ends, so interference on any one frequency
affects only a fraction of the transmission. **DSSS** multiplies each bit by a faster chip sequence, spreading
its energy thinly across a wide band, so narrowband noise contributes little to the correlated result.

**📘 CONCEPT — C129 · Spread spectrum trades bandwidth for robustness and coexistence**
> | | FHSS | DSSS |
> |---|---|---|
> | Method | **hop** the carrier frequency | multiply by a **chip sequence** |
> | Robustness | avoids a jammed channel | dilutes narrowband interference |
> | Used by | Bluetooth, early 802.11 | 802.11b, GPS, CDMA |
>
> A further benefit: several transmitters using **different codes** can share one band simultaneously, which
> is the basis of **CDMA** (C83).
>
> **Applies when** the stem mentions FHSS, DSSS, chipping, jamming resistance, or CDMA.
>
> **Boundary:** spreading provides **interference resistance and a degree of privacy through obscurity**, but
> it is **not encryption** — the code sequence is public in 802.11, so anyone with a standard receiver reads
> the traffic. Real confidentiality needs **WPA2/WPA3** (WEP being long broken), and option C's conflation of
> spreading with encryption is the intended trap.

**Wrong traces:** A = nothing exceeds the Shannon limit (C10); spreading in fact uses bandwidth
inefficiently · B = 802.11 still requires CSMA/CA (C87) · C = spreading is not a cryptographic mechanism.

---

**Q200.** An attacker positions themselves between two communicating parties, relaying and possibly altering
messages while each believes it is talking directly to the other. This is `[Applied]`

- **A)** a man-in-the-middle attack
- **B)** a denial-of-service attack
- **C)** a phishing attack
- **D)** a brute-force attack

**Answer: A) a man-in-the-middle attack**

**Trace / Why:** the defining features are **interception plus relaying while remaining undetected**. Because
the attacker sits in the path, they can read the traffic (a confidentiality breach) and modify it (an
integrity breach) — and neither endpoint sees anything unusual.

**📘 CONCEPT — C130 · Common attacks, classified by which security property they violate**
> | Attack | Violates | Mechanism |
> |---|---|---|
> | **Man-in-the-middle** | confidentiality **and** integrity | intercept and relay in the path |
> | **Sniffing / eavesdropping** | confidentiality | passive capture |
> | **Spoofing** | authenticity | forge a source address or identity |
> | **DoS / DDoS** | **availability** | exhaust a resource with traffic or requests |
> | **Phishing** | authenticity (social) | deceive the user into surrendering credentials |
> | **Replay** | authenticity | resend a valid captured message |
>
> **Applies when** the stem describes an attack's mechanism and asks for its name.
>
> **Boundary:** MITM is defeated by **authentication**, not by encryption alone — encrypting to an attacker who
> has substituted their own key gives no protection, which is exactly why TLS relies on **certificates** to
> prove the server is who it claims. That is the practical reason certificate warnings must not be dismissed:
> the warning is the only signal a MITM is present.

**Wrong traces:** B = attacks availability rather than intercepting · C = deceives the user directly, with no
interception of a live session · D = repeatedly guesses credentials.

---

## Answer Key — All 200 Questions

**Part 1 (Q1–Q70)**

| Q | A | Q | A | Q | A | Q | A | Q | A |
|---|---|---|---|---|---|---|---|---|---|
| 1 | D | 15 | B | 29 | B | 43 | A | 57 | D |
| 2 | D | 16 | C | 30 | C | 44 | D | 58 | D |
| 3 | D | 17 | A | 31 | A | 45 | A | 59 | B |
| 4 | A | 18 | A | 32 | A | 46 | C | 60 | A |
| 5 | B | 19 | C | 33 | C | 47 | B | 61 | A |
| 6 | D | 20 | C | 34 | C | 48 | B | 62 | D |
| 7 | D | 21 | C | 35 | B | 49 | A | 63 | D |
| 8 | D | 22 | C | 36 | B | 50 | C | 64 | A |
| 9 | B | 23 | B | 37 | B | 51 | C | 65 | A |
| 10 | C | 24 | B | 38 | B | 52 | B | 66 | D |
| 11 | A | 25 | B | 39 | C | 53 | A | 67 | B |
| 12 | C | 26 | C | 40 | D | 54 | A | 68 | B |
| 13 | C | 27 | A | 41 | B | 55 | A | 69 | B |
| 14 | A | 28 | B | 42 | D | 56 | A | 70 | A |

**Part 2 (Q71–Q140)**

| Q | A | Q | A | Q | A | Q | A | Q | A |
|---|---|---|---|---|---|---|---|---|---|
| 71 | C | 85 | D | 99 | D | 113 | C | 127 | B |
| 72 | B | 86 | B | 100 | A | 114 | C | 128 | B |
| 73 | D | 87 | A | 101 | A | 115 | C | 129 | B |
| 74 | D | 88 | C | 102 | D | 116 | D | 130 | A |
| 75 | C | 89 | D | 103 | A | 117 | A | 131 | B |
| 76 | A | 90 | C | 104 | B | 118 | B | 132 | A |
| 77 | B | 91 | A | 105 | B | 119 | A | 133 | A |
| 78 | D | 92 | D | 106 | D | 120 | D | 134 | B |
| 79 | D | 93 | C | 107 | C | 121 | B | 135 | C |
| 80 | D | 94 | C | 108 | B | 122 | B | 136 | A |
| 81 | A | 95 | B | 109 | C | 123 | A | 137 | A |
| 82 | D | 96 | C | 110 | D | 124 | C | 138 | A |
| 83 | D | 97 | A | 111 | B | 125 | C | 139 | B |
| 84 | D | 98 | D | 112 | A | 126 | A | 140 | C |

**Part 3 (Q141–Q200)**

| Q | A | Q | A | Q | A | Q | A |
|---|---|---|---|---|---|---|---|
| 141 | D | 156 | D | 171 | C | 186 | D |
| 142 | C | 157 | B | 172 | B | 187 | C |
| 143 | B | 158 | A | 173 | B | 188 | C |
| 144 | A | 159 | C | 174 | D | 189 | B |
| 145 | D | 160 | D | 175 | D | 190 | C |
| 146 | D | 161 | C | 176 | D | 191 | C |
| 147 | B | 162 | C | 177 | D | 192 | C |
| 148 | D | 163 | D | 178 | A | 193 | C |
| 149 | D | 164 | C | 179 | C | 194 | C |
| 150 | A | 165 | C | 180 | B | 195 | A |
| 151 | B | 166 | B | 181 | D | 196 | D |
| 152 | B | 167 | A | 182 | B | 197 | C |
| 153 | B | 168 | A | 183 | C | 198 | C |
| 154 | A | 169 | C | 184 | B | 199 | D |
| 155 | A | 170 | A | 185 | D | 200 | A |

**Answer distribution (counted, not estimated):** A = 50 · B = 50 · C = 50 · D = 50 — exactly uniform. The
letters were generated as a shuffled balanced sequence **before** any option was written and re-verified
against it afterwards; the handful of questions that drifted during drafting were fixed by **reordering their
options**, never by changing an answer.

**Difficulty mix (counted):** Core 95 · Applied 80 · Trap 25 — close to the skill's nominal 50/35/15 target,
and the closest of the three chapters in this Bank. **Worked calculations: 42 of 200**, concentrated in
Sections 1, 5, 6 and 8; the remaining 80 `[Applied]` questions test reasoning and comparison rather than
arithmetic, which is the honest shape of this subject. Both figures are reported as counted, not as
intended.

---

## Coverage Report

Every inventory item with the questions that test it. No row is empty.

| # | Concept | Questions |
|---|---|---|
| 1 | Five components of a data communication system | Q1 |
| 2 | Simplex, half-duplex, full-duplex | Q2, Q3 |
| 3 | Bandwidth vs throughput; performance measures | Q4 |
| 4 | The four delay components | Q5 |
| 5 | Propagation delay calculation | Q6 |
| 6 | Transmission delay calculation | Q7 |
| 7 | Bandwidth-delay product | Q8 |
| 8 | Jitter | Q9 |
| 9 | Bit rate vs baud rate; bits per symbol | Q10 |
| 10 | Nyquist formula and its numerical use | Q11, Q12 |
| 11 | Shannon capacity and its numerical use | Q13, Q14 |
| 12 | Choosing between Nyquist and Shannon | Q15 |
| 13 | Analog vs digital signals; the three impairments | Q16, Q19 |
| 14 | Attributes of a periodic signal | Q17 |
| 15 | Frequency–period relationship | Q18 |
| 16 | Decibel scale | Q20 |
| 17 | Signal-to-noise ratio | Q21 |
| 18 | Serial vs parallel transmission; skew | Q22 |
| 19 | Asynchronous vs synchronous transmission | Q23 |
| 20 | Bandwidth of a composite signal | Q24 |
| 21 | LAN, MAN, WAN, PAN | Q25, Q26, Q27 |
| 22 | Mesh link and port counts | Q28, Q29 |
| 23 | Star topology and its failure mode | Q30, Q31 |
| 24 | Bus topology, terminators, backbone breaks | Q32, Q33 |
| 25 | Ring topology and token passing | Q34 |
| 26 | Mesh advantages and quadratic cost | Q35, Q36 |
| 27 | Tree / hierarchical topology | Q37 |
| 28 | Point-to-point vs multipoint | Q38 |
| 29 | Client-server vs peer-to-peer | Q39 |
| 30 | Internet, intranet, extranet | Q40 |
| 31 | Circuit switching | Q41 |
| 32 | Packet switching and statistical sharing | Q42 |
| 33 | Message switching and store-and-forward granularity | Q43 |
| 34 | Virtual circuit vs datagram | Q44, Q46 |
| 35 | Out-of-order delivery in datagram networks | Q45 |
| 36 | The seven OSI layers | Q47, Q48, Q49 |
| 37 | Network layer function: routing | Q50 |
| 38 | Transport layer: process-to-process | Q51 |
| 39 | Session layer: dialog control, synchronisation | Q52 |
| 40 | Presentation layer: translation, encryption, compression | Q53 |
| 41 | Data link layer: framing | Q54 |
| 42 | Segmentation and reassembly | Q55 |
| 43 | PDU names per layer | Q56, Q57, Q58 |
| 44 | Functions recurring at two layers | Q59 |
| 45 | Delivery scope per layer; which addresses change | Q60 |
| 46 | Devices and their layers: hub | Q61 |
| 47 | Switch: MAC learning and forwarding | Q62 |
| 48 | Router: IP forwarding | Q63 |
| 49 | Gateway: protocol translation | Q64 |
| 50 | TCP/IP model layer count and mapping | Q65 |
| 51 | OSI layers absent from TCP/IP | Q66 |
| 52 | Physical (MAC) address | Q67 |
| 53 | Logical (IP) address | Q68 |
| 54 | Port numbers and sockets | Q69, Q188 |
| 55 | Encapsulation and decapsulation | Q70 |
| 56 | Guided vs unguided media | Q71, Q81 |
| 57 | UTP vs STP; the purpose of twisting | Q72, Q73 |
| 58 | Cable categories | Q74 |
| 59 | Coaxial cable construction | Q75 |
| 60 | Optical fibre: total internal reflection | Q76 |
| 61 | Single-mode vs multimode fibre | Q77 |
| 62 | Fibre advantages and disadvantages | Q78, Q79, Q88 |
| 63 | EMI immunity ranking of media | Q80 |
| 64 | Radio waves: omnidirectional propagation | Q82 |
| 65 | Microwave: line of sight and repeater towers | Q83 |
| 66 | Infrared: wall penetration | Q84 |
| 67 | Satellite propagation delay | Q85 |
| 68 | Connectors per medium | Q86, Q87, Q92 |
| 69 | Crosstalk | Q89 |
| 70 | Ethernet media designations and distances | Q90 |
| 71 | Repeaters vs amplifiers | Q91 |
| 72 | The four data conversions | Q93 |
| 73 | Manchester encoding and self-clocking | Q94, Q96 |
| 74 | NRZ defects: synchronisation, baseline, DC | Q95 |
| 75 | AMI / bipolar encoding | Q97 |
| 76 | Block coding (4B/5B) | Q98 |
| 77 | Scrambling (B8ZS, HDB3) | Q99 |
| 78 | ASK | Q100 |
| 79 | FSK | Q101 |
| 80 | PSK and QPSK | Q102 |
| 81 | QAM and constellations | Q103, Q104 |
| 82 | PCM: sampling, quantisation, encoding | Q105 |
| 83 | The sampling theorem | Q106 |
| 84 | PCM bit-rate calculation | Q107 |
| 85 | Delta modulation | Q108 |
| 86 | FDM | Q109 |
| 87 | Synchronous TDM | Q110 |
| 88 | Statistical TDM | Q111 |
| 89 | WDM | Q112 |
| 90 | Framing purpose and transparency | Q113 |
| 91 | Byte stuffing | Q114 |
| 92 | Bit stuffing rule and count | Q115, Q116 |
| 93 | Single-bit vs burst errors | Q117 |
| 94 | Error detection vs correction | Q118 |
| 95 | Simple parity and its blindness to even errors | Q119, Q120 |
| 96 | Two-dimensional parity | Q121 |
| 97 | Internet checksum | Q122 |
| 98 | CRC principle and computation | Q123, Q124 |
| 99 | CRC detection guarantees | Q125 |
| 100 | Hamming distance | Q126 |
| 101 | Distance needed to detect / correct | Q127 |
| 102 | Hamming code redundant bits | Q128 |
| 103 | Stop-and-wait and its efficiency | Q129, Q130 |
| 104 | Sliding window rationale | Q131 |
| 105 | Go-Back-N window limit | Q132 |
| 106 | Selective Repeat window limit | Q133, Q139 |
| 107 | GBN vs SR retransmission | Q134 |
| 108 | Window efficiency calculation | Q135 |
| 109 | Piggybacking | Q136 |
| 110 | Sizing the sequence-number field | Q137 |
| 111 | The parameter a = Tp/Tt | Q138 |
| 112 | Duplicate detection after a lost ACK | Q140 |
| 113 | MAC sublayer and access-protocol families | Q141 |
| 114 | Pure and slotted ALOHA throughput | Q142, Q143 |
| 115 | CSMA and persistence strategies | Q144 |
| 116 | CSMA/CD collision handling | Q145 |
| 117 | CSMA/CA and why wireless differs | Q146 |
| 118 | Minimum frame size calculation | Q147 |
| 119 | Ethernet frame sizes and MTU | Q148, Q150 |
| 120 | MAC address structure | Q149 |
| 121 | Binary exponential backoff | Q151 |
| 122 | Token passing / controlled access | Q152 |
| 123 | Store-and-forward vs cut-through switching | Q153 |
| 124 | Spanning Tree Protocol | Q154 |
| 125 | VLANs | Q155 |
| 126 | Counting collision and broadcast domains | Q156 |
| 127 | PPP and HDLC | Q157 |
| 128 | Full-duplex switched Ethernet disables CSMA/CD | Q158 |
| 129 | IP as connectionless best-effort | Q159 |
| 130 | TTL and loop prevention | Q160 |
| 131 | Fragmentation count | Q161 |
| 132 | Fragment offset in 8-byte units | Q162 |
| 133 | Reassembly at the destination only | Q163 |
| 134 | ARP | Q164 |
| 135 | ICMP and its message types | Q165 |
| 136 | ping | Q166 |
| 137 | traceroute | Q167 |
| 138 | Distance vector vs link state | Q168 |
| 139 | RIP and its 15-hop limit | Q169 |
| 140 | OSPF and Dijkstra | Q170 |
| 141 | Count-to-infinity and split horizon | Q171 |
| 142 | BGP; interior vs exterior gateway protocols | Q172 |
| 143 | Leaky bucket vs token bucket | Q173 |
| 144 | IPv4 vs IPv6 headers | Q174 |
| 145 | TCP vs UDP | Q175 |
| 146 | Choosing UDP for real-time traffic | Q176 |
| 147 | Three-way handshake | Q177, Q178 |
| 148 | Four-way connection termination | Q179 |
| 149 | TCP sequence numbers count bytes | Q180 |
| 150 | Flow control via the receive window | Q181 |
| 151 | Slow start growth | Q182 |
| 152 | Congestion avoidance | Q183 |
| 153 | AIMD and fairness | Q184 |
| 154 | Fast retransmit and the two loss signals | Q185 |
| 155 | Well-known port numbers | Q186 |
| 156 | Nagle's algorithm and silly window syndrome | Q187 |
| 157 | DNS purpose and hierarchy | Q189, Q190 |
| 158 | HTTP statelessness and cookies | Q191 |
| 159 | FTP's two connections | Q192 |
| 160 | SMTP, POP3, IMAP and MIME | Q193 |
| 161 | DHCP and DORA | Q194 |
| 162 | SSH vs TELNET; secure replacements | Q195 |
| 163 | SNMP, agents, MIBs and traps | Q196 |
| 164 | Firewall types | Q197 |
| 165 | VPNs, tunnelling and IPSec | Q198 |
| 166 | Spread spectrum: FHSS and DSSS | Q199 |
| 167 | Attack taxonomy | Q200 |

**167 inventory items, 167 covered, 0 empty.**
*Cross-referenced, not omitted:* IP addressing, subnet masks, CIDR, VLSM and supernetting are covered by the
**[Subnetting chapter](../subnetting/mcq_subnetting.md)** (117 further inventory items).

---

## High-Yield Revision Sheet

1. **Nyquist (noiseless): C = 2B log₂L.** **Shannon (noisy): C = B log₂(1 + SNR).** Given both, take the
   **lower**. Convert SNR from dB first: ratio = 10^(dB/10).
2. **bit rate = baud rate × log₂L.** Bit rate ≥ baud rate always.
3. **Tp = distance/speed** (independent of bandwidth) · **Tt = bits/rate** · **a = Tp/Tt** ·
   **BDP = bandwidth × delay**.
4. **Delay components:** transmission, propagation, queuing, processing. Only **queuing** varies with load, so
   it alone causes jitter.
5. **Mesh: n(n−1)/2 links, n−1 ports each.** Failure modes: **star → hub**, **bus → backbone**,
   **ring → any node**, **mesh → none**.
6. **OSI, bottom up:** Physical, Data Link, Network, Transport, Session, Presentation, Application.
   **PDUs:** bit, frame, packet, segment, data.
7. **Scope:** data link = **hop-to-hop** (MAC), network = **host-to-host** (IP), transport =
   **process-to-process** (port). **MAC addresses change every hop; IP addresses do not.**
8. **Devices:** hub/repeater = L1 · switch/bridge = L2 · router = L3 · gateway = up to L7.
   **Switches split collision domains; routers (and VLANs) split broadcast domains.**
9. **TCP/IP has 4 layers** (5 if physical is split out); **session and presentation have no counterpart**.
10. **Fibre wins on everything technical** — bandwidth, attenuation, EMI immunity, security — and loses only on
    **cost and splicing**. **Single-mode = narrow core, long distance, laser.**
11. **Satellite one-way delay ≈ 240 ms** (round trip ≈ 480 ms). Microwave needs **line of sight**; infrared
    **cannot pass through walls**.
12. **Manchester is self-clocking but needs 2× the signal rate.** 4B/5B achieves the same for 25% overhead.
    **AMI removes the DC component; B8ZS/HDB3 fix its zero-run problem.**
13. **PCM = sample → quantise → encode**; only **quantisation** loses information.
    **Sampling rate ≥ 2 × f_max**; 4 kHz voice × 8 bits = **64 kbps**.
14. **Bit stuffing: insert a 0 after five 1s** (the flag is 01111110). Stuffed bits = ⌊consecutive 1s ÷ 5⌋.
15. **Parity detects only an odd number of errors.** **CRC detects all bursts ≤ r bits.**
    **d_min ≥ s + 1** to detect s; **d_min ≥ 2t + 1** to correct t. **Hamming: 2^r ≥ m + r + 1.**
16. **Stop-and-wait efficiency = 1/(1 + 2a).** **Window efficiency = min(1, N/(1 + 2a))**, so **N ≥ 1 + 2a**
    fills the link. **GBN window ≤ 2^m − 1; SR window ≤ 2^(m−1).**
17. **ALOHA: pure 18.4% (1/2e), slotted 36.8% (1/e).**
    **Minimum frame size = 2 × Tp × bandwidth.** **Ethernet: min frame 64 B, MTU 1500 B, max frame 1518 B.**
18. **Fragmentation:** data per fragment = largest multiple of **8** ≤ (MTU − 20). **Offset counts 8-byte
    units.** **Reassembly happens only at the destination.**
19. **RIP:** distance vector, hop count, **max 15**. **OSPF:** link state, Dijkstra, cost. **BGP:** exterior,
    path vector, policy-based.
20. **TCP handshake: SYN → SYN-ACK → ACK** (three); **teardown is four-way**. **Slow start doubles per RTT;
    congestion avoidance adds 1 MSS per RTT; loss halves ssthresh (AIMD).** **Fast retransmit on 3 duplicate
    ACKs.**
21. **Flow control protects the receiver (rwnd); congestion control protects the network (cwnd).**
    Sender is limited by **min(rwnd, cwnd)**.
22. **Ports:** 20/21 FTP · 22 SSH · 23 TELNET · 25 SMTP · 53 DNS · 67/68 DHCP · 80 HTTP · 110 POP3 ·
    143 IMAP · 161 SNMP · 443 HTTPS.
23. **DHCP = DORA.** **HTTP is stateless — cookies add state.** **FTP uses two connections.**
    **SMTP pushes; POP3/IMAP pull.**

---

## Concept Index

| id | Concept | Rule in one line | Drilled by |
|---|---|---|---|
| C1 | Five components | Message, sender, receiver, medium, **protocol** | Q1 |
| C2 | Data-flow modes | Simplex one way; half/full duplex differ by **simultaneity** | Q2, Q3 |
| C3 | Performance measures | Bandwidth is capacity, throughput is achieved; jitter is delay variation | Q4, Q9 |
| C4 | Delay components | Only **queuing** varies with load | Q5 |
| C5 | Propagation delay | Distance ÷ speed; independent of bandwidth | Q6 |
| C6 | Transmission delay | Bits ÷ rate; repeated per hop | Q7 |
| C7 | Bandwidth-delay product | Bits in the pipe; the window needed to fill a link | Q8 |
| C8 | Bit vs baud rate | bit rate = baud × log₂L; bandwidth limits **baud** | Q10 |
| C9 | Nyquist | C = 2B log₂L; noiseless; gives the level count | Q11, Q12, Q15 |
| C10 | Shannon | C = B log₂(1 + SNR); convert dB first; absolute ceiling | Q13, Q14, Q21 |
| C11 | Signals and impairments | Attenuation weakens, distortion reshapes, noise adds | Q16, Q19 |
| C12 | Signal attributes | Simple signals have 3; composite signals have a bandwidth | Q17, Q18, Q24 |
| C13 | Decibels | ×2 = +3 dB (power); coefficient 10 for power, 20 for voltage | Q20 |
| C14 | Serial vs parallel | Parallel loses over distance to skew and crosstalk | Q22 |
| C15 | Async vs sync | Start/stop bits versus a shared clock | Q23 |
| C16 | Network scopes | PAN < LAN < MAN < WAN; scope drives every other property | Q25–Q27 |
| C17 | Mesh sizing | n(n−1)/2 links, n−1 ports; quadratic growth | Q28, Q29, Q36 |
| C18 | Topologies | Each has one characteristic failure mode | Q30, Q31, Q35, Q37 |
| C19 | Bus topology | Shared medium ⇒ terminators and contention | Q32, Q33 |
| C20 | Ring topology | Unidirectional token passing; dual ring for resilience | Q34 |
| C21 | Point-to-point vs multipoint | MAC protocols exist **only** for shared media | Q38 |
| C22 | Client-server vs peer-to-peer | Fixed roles and central control versus interchangeable roles | Q39 |
| C23 | Internet/intranet/extranet | Distinguished by **access policy**, not technology | Q40 |
| C24 | Switching techniques | Circuit reserves; packet shares statistically | Q41, Q42, Q46 |
| C25 | Store-and-forward granularity | Packets pipeline across hops; whole messages cannot | Q43 |
| C26 | Virtual circuit vs datagram | Setup and ordering versus independent routing | Q44, Q45 |
| C27 | The seven OSI layers | Learn the table in all five columns | Q47–Q49 |
| C28 | Physical layer | Bits, no addressing, no error control | Q48 |
| C29 | Network layer | Logical addressing plus routing; host-to-host | Q50 |
| C30 | Transport layer | Ports, segmentation, end-to-end reliability | Q51, Q55 |
| C31 | Session layer | Dialog control and synchronisation checkpoints | Q52 |
| C32 | Presentation layer | Translation, encryption, compression | Q53 |
| C33 | Data link layer | Framing, MAC addressing, per-link error control | Q54 |
| C34 | PDU ladder | bit → frame → packet → segment → data | Q56–Q58 |
| C35 | Two-layer functions | Flow and error control at both L2 and L4; the end-to-end argument | Q59 |
| C36 | Widening scope | MAC changes per hop; IP does not | Q60 |
| C37 | Devices and layers | The layer decides what a device can see | Q61, Q63, Q64 |
| C38 | Transparent bridging | Learn from sources, forward by destination, flood when unknown | Q62 |
| C39 | TCP/IP mapping | OSI 5–7 collapse into one Application layer | Q65, Q66 |
| C40 | Address types | MAC flat and local; IP hierarchical and routable | Q67, Q68 |
| C41 | Ports and sockets | A TCP connection is a four-tuple | Q69, Q188 |
| C42 | Encapsulation | Each layer adds its own header; only L2 adds a trailer | Q70 |
| C43 | Guided vs unguided | Decided by whether a physical conduit exists | Q71, Q81 |
| C44 | Twisted pair | The twist cancels common-mode noise; categories rank crosstalk | Q72–Q74 |
| C45 | Media comparison | Fibre wins technically, loses on cost and handling | Q75, Q78–Q80, Q88 |
| C46 | Fibre modes | Narrow core = one path = long distance | Q76, Q77 |
| C47 | Unguided bands | Radio omnidirectional; microwave and infrared line-of-sight | Q82–Q84 |
| C48 | Satellite delay and crosstalk | ~240 ms one way; crosstalk is pair-to-pair coupling | Q85, Q89 |
| C49 | Connectors | RJ for copper, BNC for coax, SC/ST/LC for fibre | Q86, Q87, Q92 |
| C50 | Ethernet designations | Rate-Base-medium; UTP is always 100 m | Q90 |
| C51 | Repeater vs amplifier | Regeneration stops noise accumulating; amplification does not | Q91 |
| C52 | The four conversions | Identify input and output before naming the technique | Q93 |
| C53 | Self-synchronisation | Guaranteed transitions let the receiver recover the clock | Q94 |
| C54 | Line-code defects | Synchronisation, baseline wandering, DC component | Q95 |
| C55 | Signal rate per code | Manchester costs 2× the bandwidth of NRZ | Q96 |
| C56 | AMI | Alternating 1s remove DC; zero runs still break timing | Q97 |
| C57 | Block coding | Redundant bits guarantee transitions cheaply | Q98 |
| C58 | Scrambling | Substitute a recognisable illegal pattern; no overhead | Q99 |
| C59 | Digital-to-analog | ASK amplitude, FSK frequency, PSK phase, QAM both | Q100–Q102 |
| C60 | QAM | Two dimensions hold more symbols; higher orders need better SNR | Q103, Q104 |
| C61 | PCM | Sample, quantise, encode; only quantisation loses information | Q105, Q107 |
| C62 | Sampling theorem | fs ≥ 2·f_max, or aliasing is irreversible | Q106 |
| C63 | Delta modulation | One bit per sample; slope overload versus granular noise | Q108 |
| C64 | Multiplexing families | FDM divides frequency, TDM time, WDM wavelength | Q109, Q112 |
| C65 | Synchronous vs statistical TDM | Fixed slots waste; on-demand slots need addresses | Q110, Q111 |
| C66 | Framing | A delimiter creates a transparency problem | Q113 |
| C67 | Byte stuffing | Escape the delimiter, then escape the escape | Q114 |
| C68 | Bit stuffing | Insert a 0 after five 1s; count resets after each stuff | Q115, Q116 |
| C69 | Error types | Burst errors dominate in practice | Q117 |
| C70 | Detection vs correction | Correction must locate the error, so costs far more | Q118 |
| C71 | Parity | Detects odd error counts only; 2-D parity locates one error | Q119–Q121 |
| C72 | Checksum vs CRC | Cheap arithmetic versus strong polynomial detection | Q122 |
| C73 | CRC | XOR division; remainder length = generator degree | Q123–Q125 |
| C74 | Hamming distance | d_min ≥ s+1 to detect s; ≥ 2t+1 to correct t | Q126, Q127 |
| C75 | Hamming bound | 2^r ≥ m + r + 1, solved by trial | Q128 |
| C76 | Stop-and-wait | One frame outstanding; needs a 1-bit sequence number | Q129 |
| C77 | Efficiency formula | 1/(1+2a); a rises with bandwidth as well as distance | Q130, Q138 |
| C78 | Window efficiency | min(1, N/(1+2a)); N ≥ 1+2a fills the link | Q131, Q135 |
| C79 | Window limits | GBN 2^m − 1; SR 2^(m−1); driven by the receiver window | Q132–Q134, Q139 |
| C80 | Piggybacking | Combine ACK with outgoing data; needs a delay timer | Q136 |
| C81 | Sequence-number sizing | Window first, then the protocol cap, then log₂ | Q137 |
| C82 | Duplicate detection | Sequence numbers catch retransmissions after a lost ACK | Q140 |
| C83 | MAC families | Random, controlled, channelised access | Q141 |
| C84 | ALOHA | Vulnerable period sets throughput: 18.4% and 36.8% | Q142, Q143 |
| C85 | CSMA persistence | Listen before talk; 1-persistent is greediest | Q144 |
| C86 | CSMA/CD backoff | Abort, jam, then random wait from 0 … 2ⁿ − 1 | Q145, Q151 |
| C87 | CSMA/CA | Wireless cannot detect collisions, so it avoids them | Q146 |
| C88 | Minimum frame size | 2 × Tp × bandwidth; ties rate, distance and frame size | Q147 |
| C89 | Ethernet frame | Min 64 B, min payload 46 B, MTU 1500 B, max 1518 B | Q148, Q150 |
| C90 | MAC address | 48 bits, OUI + serial, flat and therefore unroutable | Q149 |
| C91 | Controlled access | Token passing gives bounded delay at the cost of overhead | Q152 |
| C92 | Switching methods | Cut-through is fastest and checks nothing | Q153 |
| C93 | Spanning tree | Layer 2 has no TTL, so loops must be prevented | Q154 |
| C94 | VLANs | Give a switch the broadcast-domain separation it lacks | Q155 |
| C95 | Counting domains | Switch ports split collisions; router interfaces split broadcasts | Q156 |
| C96 | PPP and HDLC | Point-to-point needs negotiation, not medium access | Q157 |
| C97 | Switched full duplex | Removes contention instead of managing it | Q158 |
| C98 | IP service model | Connectionless, unreliable; header checksum only | Q159 |
| C99 | TTL | A hop counter; loop guard and the basis of traceroute | Q160, Q167 |
| C100 | Fragmentation arithmetic | Multiples of 8 after subtracting the header; offset ÷ 8 | Q161, Q162 |
| C101 | Reassembly | Fragment anywhere, reassemble only at the destination | Q163 |
| C102 | ARP | Resolves IP → MAC, on every hop, within one link only | Q164 |
| C103 | ICMP | Reports errors but never repairs them | Q165, Q166 |
| C104 | Distance vector vs link state | Tell neighbours about everyone, or everyone about your links | Q168, Q170 |
| C105 | RIP | Hop count, max 15; the low infinity bounds convergence | Q169 |
| C106 | Count-to-infinity | Distance without path; split horizon only partly fixes it | Q171 |
| C107 | IGP vs EGP | BGP routes between ASes by **policy**, not shortest path | Q172 |
| C108 | Traffic shaping | Leaky bucket smooths; token bucket permits saved bursts | Q173 |
| C109 | IPv4 vs IPv6 headers | Fixed 40 B, no checksum, no router fragmentation | Q174 |
| C110 | TCP vs UDP | Reliability versus low overhead; the application decides | Q175, Q176 |
| C111 | Three-way handshake | Three messages confirm two initial sequence numbers | Q177, Q178 |
| C112 | Four-way teardown | Each direction closes independently; TIME-WAIT protects reuse | Q179 |
| C113 | Sequence numbers | TCP numbers **bytes**; ACKs are cumulative | Q180 |
| C114 | Flow vs congestion control | Receiver's buffer versus the network's capacity | Q181 |
| C115 | Congestion phases | Slow start doubles; avoidance adds one MSS per RTT | Q182, Q183 |
| C116 | AIMD | Cautious growth, decisive retreat — hence stability and fairness | Q184 |
| C117 | Loss signals | Timeout collapses cwnd to 1; three duplicate ACKs halve it | Q185 |
| C118 | Well-known ports | 0–1023; the 22/23/25 cluster is the commonest confusion | Q186 |
| C119 | Silly window syndrome | Nagle fixes the sender; Clark fixes the receiver | Q187 |
| C120 | DNS | Distributed hierarchy; recursive to the resolver, iterative beyond | Q189, Q190 |
| C121 | HTTP statelessness | Stateless ≠ connectionless; cookies add the state | Q191 |
| C122 | FTP | Two connections; passive mode works through NAT | Q192 |
| C123 | Mail protocols | SMTP pushes; POP3 downloads; IMAP syncs on the server | Q193 |
| C124 | DHCP | DORA; the request is broadcast so losing offers are released | Q194 |
| C125 | Secure replacements | TELNET→SSH, FTP→SFTP, HTTP→HTTPS | Q195 |
| C126 | SNMP | Manager, agent, MIB; traps are the only unsolicited message | Q196 |
| C127 | Firewalls | Depth of inspection rises from headers to full content | Q197 |
| C128 | Tunnelling and IPSec | The whole packet becomes a payload; ESP encrypts | Q198 |
| C129 | Spread spectrum | Trades bandwidth for robustness; **not** encryption | Q199 |
| C130 | Attack taxonomy | Classify by the security property violated | Q200 |

---

## Status

**The Data Communication and Networking chapter is covered in full: 167 inventory items, 167 covered, across
200 questions written in this single run.** The three files are a file-size split, not a deferral.

Together with the **[Subnetting chapter](../subnetting/mcq_subnetting.md)** (130 questions, 117 items), the
Bank now covers the Computer Networks syllabus end to end: this chapter supplies fundamentals, models, media,
signalling, the data link layer, MAC and Ethernet, the network layer's mechanisms, the transport layer, the
application layer and security; the Subnetting chapter supplies all IP addressing arithmetic.

Available as reformatting of the same material:

- `/mcq Data Communication and Networking --hard` → trap-tier and harder applied questions only.
- `/mcq Data Communication and Networking --practice` → all 200 questions first, explanations at the end.

*Research note: the section ordering follows the frequency signal from GATE PYQ collections (GeeksforGeeks,
ExamSIDE, GATE Overflow), IBPS SO IT / bank IT professional-knowledge sets, and standard MCQ banks. Those
sources agree that for GATE the **data link and transport layers** carry the most marks, while for BCS, NTRCA
and bank IT the yield concentrates in **OSI layer functions, topologies, media and devices** — hence Sections
1–3 first and the calculation-heavy sections after. One widely-repeated error was found and deliberately made
into a trap question: several sources state that "the minimum frame size in CSMA/CD is 1500 bytes", when 1,500
is the Ethernet **MTU** and the minimum frame is **64 bytes** (Q148). Every question was written fresh and
every numerical figure independently computed; nothing is reproduced from any source.*
