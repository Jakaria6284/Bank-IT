# Data Communication and Networking — MCQ Bank
**Subject:** Computer Networks · **Files:** 3 · **Total questions:** 200
**Target:** BCS Preliminary / Bank IT Officer / NTRCA / GATE / IBPS SO IT

| File | Type | Questions | Added |
|---|---|---|---|
| [mcq_data-communication-and-networking.md](mcq_data-communication-and-networking.md) | Complete bank, part 1 (Q1–Q70) | 70 | 2026-08-27 |
| [mcq_data-communication-and-networking_part2.md](mcq_data-communication-and-networking_part2.md) | Complete bank, part 2 (Q71–Q140) | 70 | 2026-08-27 |
| [mcq_data-communication-and-networking_part3.md](mcq_data-communication-and-networking_part3.md) | Complete bank, part 3 (Q141–Q200) | 60 | 2026-08-27 |

All three files were written in a **single run**. The split is a file-size decision, not a deferral — the
chapter is covered end to end.

**Verified on delivery.** Answer letters A/B/C/D = **50/50/50/50** (exactly uniform); all 200 match the
shuffled balanced sequence generated before any option was written, with the few that drifted while drafting
fixed by reordering options rather than changing answers. Difficulty: **Core 95 · Applied 80 · Trap 25**.
Worked calculations: **42 of 200**. Inventory coverage **167/167, 0 empty**. Concept ids C1–C130.
Structure complete on all 200; zero "All of the above" options.

**Cross-reference, not a gap.** IP addressing, subnet masks, CIDR, VLSM and supernetting are covered by the
sibling **[Subnetting chapter](../subnetting/)** (130 questions, 117 inventory items). Section 8 here covers
the network layer's *other* material — IP header, TTL, fragmentation, ICMP, ARP, routing algorithms,
congestion control, IPv6 — without duplicating addressing arithmetic. Together the two chapters cover the
Computer Networks syllabus completely.

## Concepts covered

**Section 1 — Fundamentals & performance (Q1–24):** five components · simplex/half/full duplex · bandwidth vs
throughput · the four delay components · propagation, transmission and bandwidth-delay calculations · jitter ·
bit rate vs baud rate · Nyquist and Shannon with numericals and the choice between them · analog vs digital ·
signal attributes · decibels · SNR · serial vs parallel · async vs sync · composite-signal bandwidth.

**Section 2 — Types, topologies & switching (Q25–46):** LAN/MAN/WAN/PAN · mesh link and port counts · star,
bus, ring, mesh, tree with their failure modes · terminators · point-to-point vs multipoint ·
client-server vs peer-to-peer · internet/intranet/extranet · circuit, packet and message switching · virtual
circuit vs datagram · out-of-order delivery.

**Section 3 — OSI & TCP/IP models (Q47–70):** the seven layers and their functions · PDU per layer · delivery
scope and which addresses change per hop · flow and error control at two layers · devices and their layers
(hub, switch, router, gateway) · TCP/IP mapping and the missing session and presentation layers · MAC, IP and
port addressing · encapsulation.

**Section 4 — Transmission media (Q71–92):** guided vs unguided · UTP/STP and why wires are twisted · cable
categories · coaxial · fibre, total internal reflection, single vs multimode, advantages and disadvantages ·
EMI immunity ranking · radio, microwave, infrared · satellite delay · connectors · crosstalk · Ethernet media
designations · repeaters.

**Section 5 — Line coding, modulation & multiplexing (Q93–112):** the four conversions · NRZ defects ·
Manchester and self-clocking · AMI · 4B/5B block coding · B8ZS/HDB3 scrambling · ASK, FSK, PSK, QAM and
constellation sizing · PCM steps, sampling theorem and bit-rate calculation · delta modulation · FDM,
synchronous and statistical TDM, WDM.

**Section 6 — Data link layer (Q113–140):** framing and transparency · byte and bit stuffing with counts ·
error types · detection vs correction · parity and 2-D parity · Internet checksum vs CRC · CRC computation and
guarantees · Hamming distance and the Hamming bound · stop-and-wait and its efficiency · sliding windows ·
Go-Back-N and Selective Repeat window limits · efficiency numericals · piggybacking · sequence-number sizing ·
duplicate detection.

**Section 7 — MAC, Ethernet & LAN devices (Q141–158):** access-protocol families · pure and slotted ALOHA
throughput · CSMA persistence · CSMA/CD and backoff · CSMA/CA and why wireless differs · minimum frame size ·
Ethernet frame sizes and MTU · MAC address structure · token passing · store-and-forward vs cut-through ·
spanning tree · VLANs · counting collision and broadcast domains · PPP · full-duplex switching.

**Section 8 — Network layer (Q159–174):** IP as best-effort · TTL · fragmentation count and offsets ·
destination-only reassembly · ARP · ICMP, ping and traceroute · distance vector vs link state · RIP, OSPF,
BGP · count-to-infinity and split horizon · leaky vs token bucket · IPv4 vs IPv6 headers.

**Section 9 — Transport layer (Q175–188):** TCP vs UDP and when to choose each · three-way handshake and why
two messages fail · four-way teardown · byte-based sequence numbers · flow control · slow start, congestion
avoidance, AIMD · fast retransmit and the two loss signals · well-known ports · Nagle and silly window
syndrome · the connection four-tuple.

**Section 10 — Application layer & security (Q189–200):** DNS purpose and hierarchy · HTTP statelessness and
cookies · FTP's two connections · SMTP/POP3/IMAP/MIME · DHCP DORA · SSH vs TELNET · SNMP · firewall types ·
VPNs and IPSec · spread spectrum · attack taxonomy.

## Available as reformatting

- `/mcq Data Communication and Networking --hard` → trap-tier and harder applied questions only.
- `/mcq Data Communication and Networking --practice` → all 200 questions first, explanations at the end.
