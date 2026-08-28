# Subnetting — MCQ Question Bank
**Subject:** Computer Networks · **Target:** BCS Preliminary / Bank IT Officer / NTRCA / GATE / CCNA-style
**Questions:** 130 (Q1–Q130, across two files) · **Concepts covered:** 117/117 · **Complete in one run**

> How to use: attempt the question first, then read the explanation. The wrong
> options matter more than the right one — that's what the examiner is testing.

**This is the whole chapter.** IPv4 addressing and classes · special and private addresses · subnet
masks and CIDR prefixes · the three core formulas · network, broadcast and valid host ranges · subnet
design · VLSM · supernetting, route summarization and longest prefix match · the `/31`, `/32` and
subnet-zero exceptions · IPv6 subnetting · and the practical diagnostics examiners build questions from.
Nothing is held back for a later batch.

**Where the marks are.** Research across IndiaBIX/ExamVeda subnetting sets, GATE network-layer PYQs,
CCNA practice banks and BCS/bank ICT compilations gives a clear ranking. **Calculation questions
dominate** — block size, valid host range, "which subnet does this IP belong to", hosts-per-subnet — and
the single most frequently made error, stated explicitly across the sources, is **forgetting to subtract
2** for the network and broadcast addresses. For BCS Preliminary and NTRCA the yield concentrates in
Section 1 (class ranges, default masks, private/loopback/APIPA addresses). GATE adds longest prefix
match and summarization. Sections are ordered accordingly: the tools first, then the arithmetic that
carries most of the marks.

**79 of the 130 questions carry a worked calculation** in the explanation — appropriate for a chapter
that is almost entirely arithmetic. Every figure in this bank was computed and cross-checked with
Python's `ipaddress` module before it was written down, including the two summarization cases that
turn out to be impossible (Q114, Q117).

---

## Section 1 — IPv4 Addressing, Classes & Special Addresses

**Q1.** An IPv4 address is `[Core]`

- **A)** 16 bits long
- **B)** 24 bits long
- **C)** 48 bits long
- **D)** 32 bits long

**Answer: D) 32 bits long**

**Trace / Why:** four octets of 8 bits each = 32 bits, written in dotted decimal as `a.b.c.d`. IPv6 by
contrast is 128 bits, and a 48-bit address is a MAC (hardware) address.

**📘 CONCEPT — C1 · The address sizes you must not mix up**
> | Address | Bits | Written as |
> |---|---|---|
> | IPv4 | **32** | dotted decimal, `192.168.1.1` |
> | IPv6 | **128** | hex groups, `2001:db8::1` |
> | MAC | **48** | hex pairs, `00:1A:2B:3C:4D:5E` |
>
> **Applies when** a question asks for a length in bits, or for the total size of an address space.
>
> **Boundary:** 32 bits gives 2³² ≈ 4.29 billion addresses, and that finite total is the reason
> subnetting, CIDR, VLSM and NAT all exist. Every technique in this chapter is a response to those 32
> bits being too few.

**Wrong traces:** A = the size of a port number field, borrowed · B = the OUI portion of a MAC address ·
C = the MAC address length, the commonest swap.

---

**Q2.** In dotted-decimal notation, each octet of an IPv4 address can hold a value in the range `[Core]`

- **A)** 0 to 255
- **B)** 1 to 254
- **C)** 0 to 256
- **D)** 1 to 255

**Answer: A) 0 to 255**

**Trace / Why:** 8 bits represent 2⁸ = 256 distinct values, and counting from zero gives 0–255. So
`255.255.255.255` is the largest possible IPv4 address and `0.0.0.0` the smallest.

**📘 CONCEPT — C2 · 8 bits means 256 values, numbered 0 to 255**
> An n-bit field holds 2ⁿ values, numbered **0 to 2ⁿ − 1**. For an octet: 256 values, 0–255. The eight
> bit positions carry the place values **128 64 32 16 8 4 2 1**, and memorising that row makes every
> mask conversion in this chapter mechanical.
>
> **Applies when** converting between binary and decimal, or checking whether an address is valid.
>
> **Boundary:** 0 and 255 are **legal octet values** — the restriction that a *host* cannot use all-zero
> or all-one host bits is a separate rule about the host portion (C24), not about octets. So `10.0.0.5`
> and `10.255.0.5` are perfectly ordinary addresses.

**Wrong traces:** B = the usable-host restriction misapplied to every octet · C = 256 values counted as
the top value, the classic off-by-one · D = excludes 0, which is valid.

---

**Q3.** The total number of addresses in the IPv4 address space is `[Core]`

- **A)** 2¹⁶
- **B)** 2²⁴
- **C)** 2³²
- **D)** 2⁶⁴

**Answer: C) 2³²**

**Trace / Why:** 32 address bits, each independently 0 or 1, so 2³² = 4,294,967,296 addresses — about
4.29 billion. Fewer than the number of people alive, which is why IPv4 exhaustion arrived.

**📘 CONCEPT — C3 · Address-space arithmetic is always 2 to the power of the free bits**
> Addresses in any block = **2^(number of host bits)**. Whole space = 2³². A /24 = 2⁸ = 256 addresses.
> A /16 = 2¹⁶ = 65,536. This one rule generates every count in this chapter.
>
> **Applies when** a question asks "how many addresses / hosts / subnets".
>
> **Boundary:** "addresses" and "usable hosts" are different numbers. A /24 has **256 addresses** but
> **254 usable hosts**, because the first and last are reserved (C24). Read which one the stem wants —
> examiners deliberately offer both.

**Wrong traces:** A = a 16-bit space, the size of a /16 block · B = a 24-bit space, the host space of a
Class A network · D = a 64-bit space, which no IP version uses as its full address size.

---

**Q4.** In classful addressing, the first octet of a **Class A** address lies in the range `[Core]` `[Asked: BCS / Bank IT]`

- **A)** 0 to 127
- **B)** 1 to 127
- **C)** 1 to 126
- **D)** 1 to 128

**Answer: C) 1 to 126**

**Trace / Why:** Class A is identified by a leading bit of 0, giving first octets 0–127. But **0** is
reserved (it means "this network") and **127** is reserved for loopback, so the usable Class A network
range is **1–126**.

**📘 CONCEPT — C4 · The classful first-octet ranges, and the two holes in Class A**
> | Class | Leading bits | First octet | Default mask | Purpose |
> |---|---|---|---|---|
> | **A** | 0 | **1–126** | 255.0.0.0 (/8) | very large networks |
> | **B** | 10 | 128–191 | 255.255.0.0 (/16) | medium networks |
> | **C** | 110 | 192–223 | 255.255.255.0 (/24) | small networks |
> | **D** | 1110 | 224–239 | — | multicast |
> | **E** | 1111 | 240–255 | — | experimental / reserved |
>
> **Applies when** the stem gives an address and asks its class, or asks for a class's range or default
> mask.
>
> **Boundary:** **127 is the hole** — arithmetically it belongs to Class A but is reserved for loopback,
> so the answer is 126 and never 127. Note also that 0 is excluded at the bottom. Identifying the class
> needs only the **first octet**; the remaining three are irrelevant to it.

**Wrong traces:** A = the raw arithmetic range with both reserved values included · B = excludes 0 but
keeps 127, the single most common wrong answer · D = 128 is the start of Class B.

---

**Q5.** The address `172.16.50.1` belongs to which class? `[Core]`

- **A)** Class A
- **B)** Class B
- **C)** Class C
- **D)** Class D

**Answer: B) Class B**

**Trace / Why:** only the first octet matters. 172 lies in 128–191, so this is **Class B**, whose
default mask is 255.255.0.0. (It happens also to be a private address — a separate property, C7.)

**📘 CONCEPT — C4 (see Concept Index)** › Classification reads the **first octet alone**. Learn the four
boundaries — **128, 192, 224, 240** — and every classification question becomes a single comparison.
A useful sanity check: 172 > 128 so not A; 172 < 192 so not C; therefore B.

**Wrong traces:** A = requires a first octet ≤ 126 · C = requires 192–223 · D = requires 224–239, the
multicast range.

---

**Q6.** The default subnet mask of a **Class C** network is `[Core]` `[Asked: BCS / Bank IT — repeatedly]`

- **A)** 255.0.0.0
- **B)** 255.255.0.0
- **C)** 255.255.255.0
- **D)** 255.255.255.255

**Answer: C) 255.255.255.0**

**Trace / Why:** Class C devotes the first three octets to the network and the last to hosts, so the
mask has 24 one-bits: 255.255.255.0, written /24. That leaves 8 host bits and therefore 254 usable
hosts.

**📘 CONCEPT — C5 · Default masks follow the class boundary, one octet at a time**
> Class A → **/8** → 255.0.0.0 · Class B → **/16** → 255.255.0.0 · Class C → **/24** → 255.255.255.0.
> The mask simply marks where the class puts the network/host boundary.
>
> **Applies when** the stem says "default mask", or gives an address with no prefix and expects you to
> assume the classful default.
>
> **Boundary:** 255.255.255.255 is **not a subnet mask** in any normal sense — it is the limited
> broadcast address (Q20), and as a mask (/32) it describes a single host route. It appears as a
> distractor in almost every default-mask question precisely because it looks like the "most masked"
> option.

**Wrong traces:** A = the Class A default · B = the Class B default · D = the limited broadcast address,
not a class default.

---

**Q7.** Addresses in the range **224.0.0.0 to 239.255.255.255** are reserved for `[Core]`

- **A)** private internal networks
- **B)** loopback testing
- **C)** multicast delivery to a group of hosts
- **D)** experimental and future use

**Answer: C) multicast delivery to a group of hosts**

**Trace / Why:** Class D carries multicast group addresses — a single packet delivered to every host
that has joined the group. Routing protocols use them: OSPF at 224.0.0.5, RIPv2 at 224.0.0.9, and
all-hosts at 224.0.0.1.

**📘 CONCEPT — C6 · The four delivery models, and which address ranges implement them**
> | Model | Delivers to | IPv4 mechanism |
> |---|---|---|
> | **Unicast** | one specific host | Class A/B/C addresses |
> | **Broadcast** | every host on the local link | 255.255.255.255, or the subnet broadcast |
> | **Multicast** | every host that joined a group | **Class D, 224–239** |
> | **Anycast** | the nearest of several hosts sharing an address | routing-level, no dedicated class |
>
> **Applies when** the stem describes who receives a packet, or gives an address in 224–239.
>
> **Boundary:** a Class D address has **no network/host split**, so it cannot be subnetted and has no
> subnet mask — the whole 32 bits identify the group. Any question asking for the "default mask of a
> Class D network" is built on a false premise. Note also that **IPv6 has no broadcast at all** and uses
> multicast instead (Q127).

**Wrong traces:** A = the RFC 1918 private ranges (Q16) · B = 127.0.0.0/8 · D = Class E, 240–255 — the
adjacent range and the most tempting confusion.

---

**Q8.** The address block **240.0.0.0 to 255.255.255.255** is designated as `[Core]`

- **A)** Class E, reserved for experimental use
- **B)** Class D, used for multicast
- **C)** the private address space
- **D)** the link-local address space

**Answer: A) Class E, reserved for experimental use**

**Trace / Why:** Class E is identified by the leading bits 1111 and has never been allocated for
ordinary use. No host is assigned a Class E address on the public Internet.

**📘 CONCEPT — C4 (see Concept Index)** › Classes D and E are the two that carry **no host addresses**
for ordinary traffic, and they sit next to each other — 224–239 multicast, 240–255 reserved. Learn them
as a pair, because a question naming one always offers the other as a distractor. Remember the top of
Class E includes 255.255.255.255, the limited broadcast address.

**Wrong traces:** B = 224–239, the range immediately below · C = the private ranges, which sit inside
Classes A, B and C (Q16) · D = 169.254.0.0/16, inside Class B.

---

**Q9.** Why is the first octet value **127** excluded from the usable Class A range? `[Trap]`

- **A)** because it is part of the Class B range
- **B)** because 127.0.0.0/8 is reserved for loopback addressing
- **C)** because its default mask cannot be determined
- **D)** because it is reserved for multicast groups

**Answer: B) because 127.0.0.0/8 is reserved for loopback addressing**

**Trace / Why:** the whole 127.0.0.0/8 block — over 16 million addresses — is reserved so that a host
can address itself. A packet sent to any 127.x.x.x address never leaves the machine; it is looped back
by the TCP/IP stack. That is why Class A stops at 126.

**📘 CONCEPT — C7 · Loopback: 127.0.0.0/8, and what it actually tests**
> `127.0.0.1` (conventionally named `localhost`) tests that the **local TCP/IP stack** is installed and
> working. A successful ping to it proves the software stack functions; it proves **nothing** about the
> network cable, the NIC hardware, the switch, or connectivity to anywhere else.
>
> **Applies when** the stem mentions loopback, localhost, 127.x.x.x, or "testing the local stack".
>
> **Boundary:** the reservation is the **entire /8**, not just 127.0.0.1 — so 127.5.5.5 is also a
> loopback address. And a successful loopback ping combined with a failed gateway ping points at the
> NIC, driver or cabling, never at the stack. That diagnostic reasoning is what exams actually test.

**Wrong traces:** A = Class B begins at 128, so 127 is not in it · C = its mask is perfectly
determinable; the reason is reservation · D = multicast is 224–239.

---

**Q10.** The default subnet mask **255.255.0.0** corresponds to which prefix length? `[Core]`

- **A)** /8
- **B)** /16
- **C)** /24
- **D)** /32

**Answer: B) /16**

**Trace / Why:** count the one-bits. `255.255.0.0` is `11111111.11111111.00000000.00000000` — sixteen
ones — so the prefix is **/16**, the Class B default.

**📘 CONCEPT — C8 · Prefix length is simply the count of one-bits in the mask**
> Each `255` octet contributes 8 ones. So:
> 255.0.0.0 = /8 · 255.255.0.0 = /16 · 255.255.255.0 = /24 · 255.255.255.255 = /32.
> A partial octet contributes the rest — see C13 for the eight partial values.
>
> **Applies when** converting between dotted-decimal masks and slash notation in either direction.
>
> **Boundary:** the one-bits must be **contiguous and left-aligned** (C12). A value like 255.0.255.0 has
> sixteen one-bits but is not a legal mask, so "count the ones" only yields a valid prefix for a legal
> mask. Always check contiguity before counting.

**Wrong traces:** A = one 255 octet, the Class A default · C = three 255 octets, the Class C default ·
D = all four octets 255, a single-host prefix.

---

**Q11.** How many usable host addresses does a single **Class C** network provide? `[Applied]` `[Asked: BCS / Bank IT]`

- **A)** 128
- **B)** 254
- **C)** 255
- **D)** 256

**Answer: B) 254**

**Trace / Why:** Class C has 8 host bits.

```
addresses    = 2^8      = 256
usable hosts = 2^8 − 2  = 254
```

The two subtracted are the **network address** (all host bits 0, `x.y.z.0`) and the **broadcast
address** (all host bits 1, `x.y.z.255`). Neither can be assigned to a host.

**📘 CONCEPT — C9 · The −2 rule: the most examined fact in the whole chapter**
> **Usable hosts = 2^(host bits) − 2.** The two unusable addresses are:
> - **first address** — all host bits 0 — the **network (subnet) address**, which names the subnet;
> - **last address** — all host bits 1 — the **broadcast address** for that subnet.
>
> **Applies when** any question asks for hosts, usable addresses, or assignable addresses.
>
> **Boundary:** forgetting the −2 is, by every source consulted, the **single most common subnetting
> error** — which is exactly why 256 sits in the options. Two genuine exceptions exist: a **/31** gives
> 2 usable addresses on a point-to-point link (RFC 3021, Q121) and a **/32** is a single host route with
> no network or broadcast address at all (Q122). Outside those, the −2 always applies.

**Wrong traces:** A = 2⁷, from using 7 host bits · C = 255, subtracting only one · D = **256**, the total
address count with the −2 forgotten — the intended trap.

---

**Q12.** A Class B network provides how many usable host addresses? `[Applied]`

- **A)** 1,022
- **B)** 4,094
- **C)** 16,382
- **D)** 65,534

**Answer: D) 65,534**

**Trace / Why:** Class B uses two octets for the network, leaving **16 host bits**.

```
2^16 − 2 = 65,536 − 2 = 65,534
```

**📘 CONCEPT — C9 (see Concept Index)** › The three classful host counts are worth knowing cold, because
they anchor every estimate:
**Class A → 2²⁴ − 2 = 16,777,214** · **Class B → 2¹⁶ − 2 = 65,534** · **Class C → 2⁸ − 2 = 254**.
Note the pattern in the distractors here — 1,022 (/22), 4,094 (/20) and 16,382 (/18) are all genuine
subnet sizes, so an examiner can build a whole option set from neighbouring prefixes.

**Wrong traces:** A = a /22 subnet (10 host bits) · B = a /20 subnet (12 host bits) · C = a /18 subnet
(14 host bits).

---

**Q13.** How many Class A networks are available? `[Applied]`

- **A)** 27
- **B)** 64
- **C)** 128
- **D)** 126

**Answer: D) 126**

**Trace / Why:** the leading bit is fixed at 0, leaving **7 bits** of network id.

```
2^7     = 128 possible values (0–127)
2^7 − 2 = 126 usable, since 0 and 127 are reserved
```

**📘 CONCEPT — C10 · Counting networks per class: fix the leading bits, count what is left**
> | Class | Leading bits | Network bits | Networks |
> |---|---|---|---|
> | A | 1 bit fixed | 7 | **2⁷ − 2 = 126** |
> | B | 2 bits fixed | 14 | **2¹⁴ = 16,384** |
> | C | 3 bits fixed | 21 | **2²¹ = 2,097,152** |
>
> **Applies when** the stem asks how many networks a class contains.
>
> **Boundary:** **only Class A subtracts 2**, for the 0 and 127 reservations (C4, C7). Classes B and C
> have no comparable holes, so applying "−2" to them is an over-generalisation — and applying nothing to
> Class A is the opposite error. This asymmetry is the whole point of the question.

**Wrong traces:** A = 27 has no derivation; a surface-plausible small number · B = 2⁶, from using 6
network bits · C = 2⁷ with the two reserved values not removed — the intended trap.

---

**Q14.** In the address `10.45.200.7` with its **default** mask, the network portion and host portion are
`[Core]`

- **A)** network 10.45, host 200.7
- **B)** network 10.45.200, host 7
- **C)** network 10, host 45.200.7
- **D)** network 10.45.200.7, host none

**Answer: C) network 10, host 45.200.7**

**Trace / Why:** first octet 10 lies in 1–126, so this is **Class A** with default mask 255.0.0.0 (/8).
The mask's one-bits cover exactly the first octet, so `10` is the network and the remaining three octets
`45.200.7` are the host portion.

**📘 CONCEPT — C11 · The mask, not the address, decides where the network ends**
> The address alone carries no boundary. **Wherever the mask has 1-bits is network; wherever it has
> 0-bits is host.** With the classful default this coincides with an octet boundary, but with any other
> prefix it need not — `10.45.200.7/12` would put the boundary inside the second octet.
>
> **Applies when** the stem gives an address and a mask (or expects the classful default) and asks for
> the split.
>
> **Boundary:** the same address has a **different** network portion under a different mask, which is
> the entire basis of subnetting: borrowing host bits for the network moves the boundary rightward
> (C15). So "what is the network part of 10.45.200.7" has no answer until a mask is supplied.

**Wrong traces:** A = a /16 split, the Class B boundary applied to a Class A address · B = a /24 split,
the Class C boundary · D = a /32 split, which leaves no host bits at all.

---

**Q15.** Which of the following is a **private** IPv4 address as defined by RFC 1918? `[Core]` `[Asked: BCS / Bank IT]`

- **A)** 169.254.10.5
- **B)** 172.35.4.9
- **C)** 191.168.1.1
- **D)** 10.200.15.30

**Answer: D) 10.200.15.30**

**Trace / Why:** RFC 1918 reserves exactly three blocks, and `10.0.0.0/8` is the Class A one — so any
address starting with 10 is private. The distractors are each one character away from a private block,
which is how this question is always built.

**📘 CONCEPT — C12 · The three RFC 1918 private blocks, exactly**
> | Block | Range | Class |
> |---|---|---|
> | **10.0.0.0/8** | 10.0.0.0 – 10.255.255.255 | one Class A |
> | **172.16.0.0/12** | 172.16.0.0 – **172.31**.255.255 | 16 Class B networks |
> | **192.168.0.0/16** | 192.168.0.0 – 192.168.255.255 | 256 Class C networks |
>
> These are never routed on the public Internet, which is why hosts using them need **NAT** (Q130) to
> reach outside.
>
> **Applies when** the stem asks which address is private, or whether a host can be reached from the
> Internet directly.
>
> **Boundary:** the boundaries are exact and are where marks are lost — **172.16 to 172.31** (not
> 172.32, Q17), and **192.168** (not 191.168 or 192.169). Note that `169.254.x.x` is **not** RFC 1918;
> it is APIPA link-local (Q19), a different reservation with a different meaning.

**Wrong traces:** A = APIPA link-local, reserved but not RFC 1918 private · B = 172.35 is outside the
172.16–172.31 window · C = 191.168 is one digit off 192.168, and is in fact public Class B space.

---

**Q16.** The private block **172.16.0.0/12** ends at which address? `[Trap]` `[Asked: GATE-style]`

- **A)** 172.16.255.255
- **B)** 172.23.255.255
- **C)** 172.31.255.255
- **D)** 172.32.255.255

**Answer: C) 172.31.255.255**

**Trace / Why:** a /12 leaves 20 host bits, and the mask 255.240.0.0 fixes only the top 4 bits of the
second octet.

```
/12  → block size in the 2nd octet = 256 − 240 = 16
start 172.16.0.0  →  covers second octets 16 through 16 + 16 − 1 = 31
end   172.31.255.255
```

So the block spans **16 Class B networks**, 172.16 through 172.31 inclusive.

**📘 CONCEPT — C13 · Block size = 256 − (the interesting mask octet)**
> The "interesting octet" is the one where the mask is neither 255 nor 0. Subtract its value from 256
> and you have the **block size** — the increment between consecutive networks in that octet. Subnets
> then begin at multiples of the block size, and each ends one before the next begins.
>
> **Applies when** you need any network boundary, broadcast address or range. This is the single most
> useful shortcut in the chapter.
>
> **Boundary:** the last address of a block is **start + block size − 1**, not start + block size — the
> off-by-one that produces distractor C here. Verify by checking that the *next* block starts exactly
> one higher: 172.32.0.0 is indeed the next /12.

**Wrong traces:** A = treats the block as a single /16 · B = the end of 172.16.0.0/**13** (block size 8),
from misreading the prefix · D = start + block size, one whole network too far — the classic off-by-one.

---

**Q17.** A Windows host reports its IP address as **169.254.87.12**. This most likely means that `[Applied]`

- **A)** the host failed to obtain an address from a DHCP server and self-assigned an APIPA address
- **B)** the host is using a loopback address for local testing
- **C)** the host has been manually assigned a public address
- **D)** the host is configured as a multicast receiver

**Answer: A) the host failed to obtain an address from a DHCP server and self-assigned an APIPA address**

**Trace / Why:** `169.254.0.0/16` is the APIPA (link-local) block. A DHCP client that broadcasts a
request and gets no reply picks an address from this range at random and verifies it is unused. It can
then talk to other hosts on the same link, but has **no default gateway**, so it cannot reach anything
beyond the local segment.

**📘 CONCEPT — C14 · APIPA is a diagnostic signal, not a working configuration**
> Seeing `169.254.x.x` with mask 255.255.0.0 means exactly one thing: **DHCP failed**. Likely causes —
> the DHCP server is down, its scope is exhausted, or the link to it is broken (unplugged cable, dead
> switch port, failed NIC).
>
> **Applies when** the stem describes a host that cannot reach the Internet and shows its address, or
> mentions link-local addressing.
>
> **Boundary:** distinguish the three reserved blocks that all look "special": **127.x.x.x** = loopback,
> traffic never leaves the host · **169.254.x.x** = APIPA, local link only, DHCP failed ·
> **10/172.16/192.168** = RFC 1918 private, routable internally and NATed outward. Only the last is a
> normal working configuration.

**Wrong traces:** B = loopback is 127.0.0.0/8 · C = 169.254 is reserved and never publicly assigned ·
D = multicast is 224–239.

---

**Q18.** The address **255.255.255.255** is used as `[Core]`

- **A)** the default subnet mask for Class E
- **B)** the address of the default gateway
- **C)** a reserved multicast group address
- **D)** the limited broadcast address, delivered to all hosts on the local network only

**Answer: D) the limited broadcast address, delivered to all hosts on the local network only**

**Trace / Why:** a packet to 255.255.255.255 reaches every host on the sender's own link. Routers **do
not forward** it, which is what "limited" means — the broadcast cannot escape the local segment and
flood the Internet. DHCP discovery uses it, because a client with no address yet has no other way to
reach a server.

**📘 CONCEPT — C15 · Two kinds of broadcast, distinguished by how far they travel**
> - **Limited broadcast — 255.255.255.255.** All hosts on the local link. Never forwarded by routers.
>   Used when the sender does not yet know its own subnet (DHCP, BOOTP).
> - **Directed (subnet) broadcast — the last address of a specific subnet**, e.g. 192.168.1.255 for
>   192.168.1.0/24. Identifies a *remote* subnet's hosts, and *may* be forwarded, though routers
>   normally block it (it enables amplification attacks).
>
> **Applies when** the stem asks who receives a packet, or which address a DHCP client uses.
>
> **Boundary:** a **router is the boundary of a broadcast domain** (Q129) — which is precisely why
> subnetting reduces broadcast traffic, the main engineering reason for doing it at all (C16).

**Wrong traces:** A = Class D and E have no default masks (C6) · B = the gateway is an ordinary host
address on the subnet · C = multicast is 224.0.0.0–239.255.255.255.

---

**Q19.** In a routing table, the entry **0.0.0.0/0** represents `[Trap]`

- **A)** an unreachable network
- **B)** the loopback network
- **C)** the default route, matching any destination not matched by a more specific entry
- **D)** the local broadcast address

**Answer: C) the default route, matching any destination not matched by a more specific entry**

**Trace / Why:** a /0 prefix has **zero** significant bits, so every destination address matches it.
Because routers select the **longest** matching prefix (C40), any more specific entry wins, and
0.0.0.0/0 is used only when nothing else matches — which is exactly the behaviour wanted from a default
route.

**📘 CONCEPT — C16 · What 0.0.0.0 means depends entirely on where it appears**
> | Context | Meaning |
> |---|---|
> | Routing table as **0.0.0.0/0** | **default route** — matches everything, lowest priority |
> | Source address of a DHCP request | "I do not have an address yet" |
> | A server bind address | "listen on all local interfaces" |
> | Host portion all zeros, e.g. 192.168.1.0 | the **network address** of that subnet |
>
> **Applies when** the stem shows 0.0.0.0 and asks what it denotes.
>
> **Boundary:** 0.0.0.0/0 is the **least** specific route, yet it is the one that makes the Internet
> work for ordinary hosts — a home PC typically has just its local subnet plus a default route. The
> apparent paradox resolves through longest-prefix match: matching everything is only useful *because*
> it loses every tie.

**Wrong traces:** A = an unreachable network is not represented by a route at all · B = loopback is
127.0.0.0/8 · D = the local broadcast is 255.255.255.255.

---

**Q20.** The primary engineering reason for dividing a large network into subnets is to `[Core]`

- **A)** increase the total number of usable host addresses available
- **B)** reduce broadcast traffic by limiting the size of each broadcast domain
- **C)** allow a single network to use two different classes simultaneously
- **D)** eliminate the need for routers between hosts

**Answer: B) reduce broadcast traffic by limiting the size of each broadcast domain**

**Trace / Why:** every host on a subnet receives every broadcast sent on it. One flat network with 4,000
hosts means every ARP request interrupts 4,000 machines. Split it into sixteen subnets of 250 and a
broadcast disturbs 250. The gains in security segmentation and administrative delegation follow from the
same split.

**📘 CONCEPT — C17 · Subnetting buys containment, and *costs* addresses**
> Benefits: smaller broadcast domains · security and policy boundaries between segments ·
> administrative delegation · smaller routing tables through aggregation (C41) · matching the physical
> topology.
>
> The cost: **each subnet loses 2 addresses** to its own network and broadcast address (C9), so more
> subnets means fewer total usable hosts.
>
> **Applies when** the stem asks why subnetting is done, or what it costs.
>
> **Boundary:** subnetting **reduces** the usable host count rather than increasing it — splitting a
> /24 (254 hosts) into four /26 subnets yields 4 × 62 = **248** usable hosts, 6 fewer. Option A inverts
> the actual arithmetic, and that inversion is the trap. Subnetting also *requires* a router to move
> traffic between subnets, so it cannot remove the need for one.

**Wrong traces:** A = the opposite of the arithmetic; subnetting costs addresses · C = classes are not
mixed within one network · D = inter-subnet traffic **requires** a router, so this is backwards.

---

**Q21.** Two hosts are on the **same** subnet if `[Core]`

- **A)** they share the same first octet
- **B)** applying the subnet mask to both addresses yields the same network address
- **C)** they are physically connected to the same switch
- **D)** they have the same default gateway configured

**Answer: B) applying the subnet mask to both addresses yields the same network address**

**Trace / Why:** the definitive test is arithmetic: bitwise-AND each address with the mask and compare.
Equal results mean the same subnet, so the hosts can communicate directly without a router; different
results mean traffic must go through the gateway.

**📘 CONCEPT — C18 · The AND test is the definition of "same subnet"**
> ```
> network address = IP AND mask
> ```
> Same result ⇒ same subnet ⇒ direct delivery via ARP.
> Different result ⇒ different subnets ⇒ send to the default gateway.
>
> **Applies when** the stem gives two addresses and a mask and asks whether they can talk directly, or
> why two hosts on one switch cannot reach each other.
>
> **Boundary:** the test uses the mask, so **the same pair of addresses can be same-subnet under one
> mask and different-subnet under another** (Q80). Note also that physical connectivity is neither
> necessary nor sufficient — two hosts on one switch with mismatched subnets will not communicate, which
> is a classic troubleshooting scenario and exactly why option C fails.

**Wrong traces:** A = the first octet only decides the class, not the subnet · C = physical adjacency
does not imply logical adjacency · D = a shared gateway is usually a *consequence* of being on one
subnet, not the test for it — and can be misconfigured.

---

**Q22.** Which statement about a **Class D** address is correct? `[Trap]`

- **A)** its default subnet mask is 255.255.255.0
- **B)** it can be subnetted like a Class C address
- **C)** it has no network/host division and therefore cannot be subnetted
- **D)** it identifies a single host uniquely, like a Class A address

**Answer: C) it has no network/host division and therefore cannot be subnetted**

**Trace / Why:** all 32 bits of a Class D address identify a **multicast group**, not a network plus a
host. There is no host field to borrow bits from, so subnetting is meaningless — and no default mask
exists.

**📘 CONCEPT — C6 (see Concept Index)** › Subnetting requires a **host field to borrow from**. Classes A,
B and C have one; Classes D and E do not. So any question offering a mask, a subnet count or a host
count for a Class D address is built on a false premise, and recognising the false premise *is* the
answer. The same reasoning applies to Class E.

**Wrong traces:** A = Class D has no default mask at all · B = there are no host bits to borrow ·
D = a multicast address identifies a **group**, which is the opposite of a unique host.

---
## Section 2 — Subnet Masks, CIDR Prefixes & Conversions

*The tool section. Every calculation later depends on converting masks fluently.*

**Q23.** The function of a subnet mask is to `[Core]`

- **A)** encrypt the network portion of the address before transmission
- **B)** tell a host which bits of an IP address identify the network and which identify the host
- **C)** convert a private address into a public address
- **D)** assign IP addresses automatically to hosts on a segment

**Answer: B) tell a host which bits of an IP address identify the network and which identify the host**

**Trace / Why:** the mask is a 32-bit pattern of contiguous 1s followed by 0s. Wherever it holds a 1,
the corresponding address bit is network; wherever 0, host. A host ANDs its own address with the mask to
learn its own subnet, and does the same with a destination to decide local delivery or gateway (C18).

**📘 CONCEPT — C19 · The mask carries no address information — only a boundary**
> An IP address without a mask is ambiguous: `192.168.1.130` could be one of 254 hosts on a /24 or one
> of 126 on a /25, and the two answers differ. The mask supplies the missing boundary, which is why
> every real configuration is a **pair**: address + mask.
>
> **Applies when** the stem asks what the mask does, or presents an address with no mask.
>
> **Boundary:** the mask is a **local** decision, used by the host and its router. It is **not
> transmitted in the IP header** — a packet carries source and destination addresses only. That is
> precisely why a mask *mismatch* between two hosts on one wire causes one-way traffic failures: each
> computes a different subnet and neither can be corrected by the other.

**Wrong traces:** A = masks perform no encryption · C = that is NAT (Q130) · D = that is DHCP.

---

**Q24.** The prefix **/26** written as a dotted-decimal subnet mask is `[Applied]`

- **A)** 255.255.255.128
- **B)** 255.255.255.192
- **C)** 255.255.255.224
- **D)** 255.255.255.240

**Answer: B) 255.255.255.192**

**Trace / Why:** /26 means 26 one-bits. Three full octets use 24, leaving **2 one-bits** at the top of
the fourth octet.

```
11111111.11111111.11111111.11000000
                            ^^
    fourth octet = 128 + 64 = 192
```

**📘 CONCEPT — C20 · The eight legal partial-octet values, memorised once**
> | One-bits in the octet | Value | Prefix ends at |
> |---|---|---|
> | 1 | **128** | /9, /17, /25 |
> | 2 | **192** | /10, /18, /26 |
> | 3 | **224** | /11, /19, /27 |
> | 4 | **240** | /12, /20, /28 |
> | 5 | **248** | /13, /21, /29 |
> | 6 | **252** | /14, /22, /30 |
> | 7 | **254** | /15, /23, /31 |
> | 8 | **255** | /16, /24, /32 |
>
> Running totals of 128, 64, 32, 16, 8, 4, 2, 1 — each row adds the next place value.
>
> **Applies when** converting a prefix to a mask or a mask to a prefix, in either direction.
>
> **Boundary:** which **octet** the partial value lands in comes from the prefix: /9–/16 → second octet,
> /17–/24 → third, /25–/32 → fourth. Getting the value right but the octet wrong is a common half-error
> — 255.192.0.0 is /10, not /26.

**Wrong traces:** A = one one-bit, /25 · C = three one-bits, /27 · D = four one-bits, /28.

---

**Q25.** The subnet mask **255.255.255.248** corresponds to `[Applied]`

- **A)** /26
- **B)** /27
- **C)** /28
- **D)** /29

**Answer: D) /29**

**Trace / Why:** work backwards through the table. 248 = 11111000, which is **5 one-bits**. Three full
octets give 24, so 24 + 5 = **/29**. That leaves 3 host bits, giving 2³ − 2 = 6 usable hosts.

**📘 CONCEPT — C20 (see Concept Index)** › Two reliable ways to convert an octet value to a bit count:
subtract from 256 and take log₂ of the result (256 − 248 = 8 = 2³, so 3 zero-bits, hence 5 one-bits), or
read the memorised row directly. The first method also hands you the **block size** for free (C13) —
here 8 — which is why it is worth preferring.

**Wrong traces:** A = 192 · B = 224 · C = 240 — each the adjacent row in the table, which is exactly how
this option set is built.

---

**Q26.** The prefix **/19** as a dotted-decimal mask is `[Applied]`

- **A)** 255.255.192.0
- **B)** 255.255.240.0
- **C)** 255.255.224.0
- **D)** 255.224.0.0

**Answer: C) 255.255.224.0**

**Trace / Why:** /19 = 16 bits (two full octets) + **3 more** in the third octet.

```
11111111.11111111.11100000.00000000
                   ^^^
    third octet = 128 + 64 + 32 = 224
```

**📘 CONCEPT — C20 (see Concept Index)** › Note that options A, B and C here all have the partial value
in the **correct octet** and differ only in bit count, while D has the right value in the **wrong
octet** — 255.224.0.0 is /11. A complete option set for these questions probes both errors at once, so
check the octet position *and* the value before answering.

**Wrong traces:** A = 2 bits, /18 · B = 4 bits, /20 · D = correct value 224 placed in the second octet,
giving /11.

---

**Q27.** The subnet mask **255.255.240.0** provides how many host bits? `[Applied]`

- **A)** 8
- **B)** 10
- **C)** 12
- **D)** 16

**Answer: C) 12**

**Trace / Why:** 240 = 11110000 = 4 one-bits, so the prefix is 16 + 4 = **/20**.

```
host bits = 32 − prefix = 32 − 20 = 12
usable hosts = 2^12 − 2 = 4,094
```

**📘 CONCEPT — C21 · Host bits = 32 − prefix, and everything else follows from it**
> One subtraction unlocks the whole question set:
> - **host bits** h = 32 − prefix
> - **addresses per subnet** = 2^h
> - **usable hosts** = 2^h − 2 (C9)
> - **block size** = 2^h, expressed in the interesting octet as 256 − mask value (C13)
>
> **Applies when** the stem gives a mask or prefix and asks for any count.
>
> **Boundary:** host bits are counted from the **right**, network bits from the **left**, and they always
> sum to 32. A question that gives you one is giving you both — so "how many subnet bits does /20 have
> in a Class B network?" is answered from the same subtraction: 20 − 16 = 4 borrowed bits (C22).

**Wrong traces:** A = /24 · B = /22 · D = /16, the Class B default with nothing borrowed.

---

**Q28.** Which of the following is **NOT** a valid IPv4 subnet mask? `[Trap]`

- **A)** 255.255.0.255
- **B)** 255.255.255.192
- **C)** 255.128.0.0
- **D)** 255.255.252.0

**Answer: A) 255.255.0.255**

**Trace / Why:** write it in binary:

```
11111111.11111111.00000000.11111111
                            ^^^^^^^^ one-bits AFTER zero-bits — illegal
```

A subnet mask must be a **single unbroken run of 1s followed by 0s**. This value has 1s reappearing
after 0s, so it does not define a contiguous network/host boundary and no prefix length describes it.

**📘 CONCEPT — C22 · The contiguity rule, and the quick test for it**
> A legal mask is **contiguous 1s, left-aligned, then 0s** — so it is always expressible as a single
> `/n`. Quick test: every octet must be **255**, or **0**, or one of the eight legal partial values
> (128, 192, 224, 240, 248, 252, 254), and once a non-255 octet appears, **every octet after it must be
> 0**.
>
> **Applies when** the stem asks which mask is invalid, or offers an unusual-looking value.
>
> **Boundary:** the rule catches two distinct violations — a **non-legal octet value** (255.255.100.0:
> 100 = 01100100, not left-aligned) and a **legal value followed by a non-zero octet** (255.255.0.255).
> Historically, non-contiguous masks were permitted by some early implementations, which is why they
> still appear as distractors; CIDR made contiguity mandatory.

**Wrong traces:** B = /26, valid · C = /9, valid — unusual-looking but a legal single one-bit in the
second octet · D = /22, valid.

---

**Q29.** Which set contains **only** valid values for a partial octet in a subnet mask? `[Core]`

- **A)** 128, 192, 224, 240
- **B)** 100, 150, 200, 250
- **C)** 127, 191, 223, 239
- **D)** 129, 193, 225, 241

**Answer: A) 128, 192, 224, 240**

**Trace / Why:** each of these is a left-aligned run of 1s: 128 = 10000000, 192 = 11000000,
224 = 11100000, 240 = 11110000. The full legal set is **128, 192, 224, 240, 248, 252, 254, 255** — and no
other value can appear in a mask.

**📘 CONCEPT — C22 (see Concept Index)** › The eight values are exactly the running sums of the place
values from the left: 128, +64, +32, +16, +8, +4, +2, +1. Any other number has a 0 somewhere before a 1
and so breaks contiguity. Note how the distractors are constructed: **C is each legal value minus 1**
(127 = 01111111, right-aligned — the exact inverse of legal) and **D is each legal value plus 1**
(129 = 10000001, with a gap). Recognising ±1 tampering answers these instantly.

**Wrong traces:** B = arbitrary round decimals, none left-aligned · C = each value one **less** than
legal, producing right-aligned runs · D = each value one **more** than legal, producing a gap.

---

**Q30.** The **wildcard mask** corresponding to the subnet mask 255.255.255.192 is `[Applied]`

- **A)** 0.0.0.63
- **B)** 0.0.0.64
- **C)** 0.0.0.192
- **D)** 255.255.255.63

**Answer: A) 0.0.0.63**

**Trace / Why:** a wildcard mask is the **bitwise inverse** of the subnet mask, obtained by subtracting
each octet from 255.

```
255 − 255 = 0
255 − 255 = 0
255 − 255 = 0
255 − 192 = 63
        →  0.0.0.63
```

**📘 CONCEPT — C23 · Wildcard masks invert the meaning of every bit**
> | | Subnet mask | Wildcard mask |
> |---|---|---|
> | A **0** bit means | host bit | **must match** |
> | A **1** bit means | network bit | don't care |
> | Used by | hosts and routing | **ACLs**, OSPF network statements |
>
> Conversion in either direction: **255 − each octet**.
>
> **Applies when** the stem mentions an access control list, an OSPF `network` statement, or asks for the
> inverse mask.
>
> **Boundary:** the semantics are **exactly reversed**, which is the whole trap — in a wildcard mask 0
> means "must match" while in a subnet mask 0 means "host bit, free to vary". Note the useful identity:
> wildcard value + 1 = **block size** (63 + 1 = 64), so a wildcard mask hands you the block size
> directly.

**Wrong traces:** B = the block size (64) rather than the wildcard (63) — off by one · C = the original
octet uninverted · D = only the last octet inverted, leaving the first three at 255.

---

**Q31.** In subnetting, "borrowed bits" refers to bits taken from `[Core]`

- **A)** the host portion and added to the network portion
- **B)** the network portion and added to the host portion
- **C)** the broadcast address and reassigned to hosts
- **D)** a neighbouring subnet's address space

**Answer: A) the host portion and added to the network portion**

**Trace / Why:** subnetting lengthens the prefix. Taking a Class C /24 to /27 borrows **3 bits** from the
8 host bits, so the mask grows and the network field grows with it. Result: 2³ = 8 subnets, each with
2⁵ − 2 = 30 hosts instead of one network of 254.

**📘 CONCEPT — C24 · Borrowing bits: the trade is always subnets against hosts**
> Borrow **s** bits from the host field:
> - **subnets created** = 2^s
> - **hosts per subnet** = 2^(h − s) − 2, where h was the original host-bit count
>
> The product of the two shrinks as s grows, because each new subnet loses 2 addresses to its own
> network and broadcast (C9).
>
> **Applies when** the stem gives an original class or prefix and a new, longer prefix.
>
> **Boundary:** the borrowing is **one-directional** — you can only lengthen the prefix when subnetting.
> Shortening it merges networks, which is **supernetting** (C41), a different operation with different
> rules. And the total is conserved unfavourably: more subnets always means fewer usable hosts overall
> (C17).

**Wrong traces:** B = the reverse direction, which is supernetting · C = the broadcast address is a
consequence of the split, not a source of bits · D = borrowing happens within one address block, never
from a neighbour.

---

**Q32.** For the prefix **/27**, the binary pattern of the fourth octet of the mask is `[Applied]`

- **A)** 10000000
- **B)** 11000000
- **C)** 11100000
- **D)** 11110000

**Answer: C) 11100000**

**Trace / Why:** /27 = 24 (three full octets) + 3, so the fourth octet holds **3 one-bits** followed by
5 zeros: `11100000` = 128 + 64 + 32 = **224**. Mask = 255.255.255.224, block size 32, hosts 30.

**📘 CONCEPT — C20 (see Concept Index)** › The three representations of one prefix are interchangeable
and you should be able to move between them in either direction without hesitation:
**/27 ↔ 255.255.255.224 ↔ 11100000 (fourth octet)**. The five zero-bits are the host bits, hence
2⁵ = 32 addresses per block and 30 usable. Every question in Sections 3 and 4 is a use of this triangle.

**Wrong traces:** A = /25 · B = /26 · D = /28 — the adjacent prefixes, one bit either side.

---

**Q33.** As the CIDR prefix length increases, the number of usable hosts per subnet `[Trap]`

- **A)** increases, because more network bits are available
- **B)** stays the same, since the total address space is fixed
- **C)** increases up to /24 and then decreases
- **D)** decreases, because fewer host bits remain

**Answer: D) decreases, because fewer host bits remain**

**Trace / Why:** host bits = 32 − prefix, so a longer prefix leaves fewer of them and 2^h − 2 shrinks
sharply.

| Prefix | Host bits | Usable hosts |
|---|---|---|
| /24 | 8 | 254 |
| /26 | 6 | 62 |
| /28 | 4 | 14 |
| /30 | 2 | 2 |

Each extra prefix bit **halves** the hosts (a little worse than halving, because the −2 is a fixed cost).

**📘 CONCEPT — C25 · The direction rule: longer prefix ⇒ smaller subnet ⇒ more subnets**
> | As the prefix gets longer | |
> |---|---|
> | Network bits | more |
> | Host bits | fewer |
> | Hosts per subnet | **fewer** |
> | Number of subnets | **more** |
> | Block size | smaller |
>
> **Applies when** the stem varies a prefix and asks what happens, or asks which of two prefixes is
> "bigger".
>
> **Boundary:** the word **"bigger"** is genuinely ambiguous and examiners exploit it — a /30 is a
> *bigger number* but a *smaller network* than a /24. Read whether the stem means the prefix value or
> the address block. Note also that /31 breaks the halving pattern (2 usable, not 0) and /32 breaks it
> again (1 address, no hosts) — the two exceptions in Q121–Q122.

**Wrong traces:** A = confuses more network bits with more hosts — the direct inversion · B = the total
space is fixed, but its division into subnets is not · C = invents a turning point at /24 that does not
exist.

---

**Q34.** How many host bits does the mask **255.255.248.0** leave? `[Applied]`

- **A)** 11
- **B)** 12
- **C)** 13
- **D)** 21

**Answer: A) 11**

**Trace / Why:** 248 = 11111000 = 5 one-bits, so the prefix is 16 + 5 = **/21**.

```
host bits = 32 − 21 = 11
usable hosts = 2^11 − 2 = 2,046
```

**📘 CONCEPT — C21 (see Concept Index)** › Notice the trap in option D: **21 is the prefix, not the host
count**, and the two are complements. Whenever a prefix-derived number appears among the options
alongside its complement, check which side of the 32 the question wants. The reliable habit is to write
down both numbers — "/21, so 11 host bits" — before looking at the options at all.

**Wrong traces:** B = /20 · C = /19 · D = the **prefix length** itself, mistaken for the host-bit count.

---

**Q35.** The mask **255.255.255.128** is equivalent to `[Applied]`

- **A)** /22
- **B)** /23
- **C)** /24
- **D)** /25

**Answer: D) /25**

**Trace / Why:** 128 = 10000000 = **1 one-bit**, so the prefix is 24 + 1 = **/25**. That splits a /24
into exactly two halves of 128 addresses (126 usable each): `x.y.z.0/25` and `x.y.z.128/25`.

**📘 CONCEPT — C26 · /25 is the smallest split, and the boundary between "third octet" and "fourth octet" work**
> Prefixes /25 through /30 put the boundary in the **fourth** octet, so all the arithmetic stays inside
> the last number and block sizes are 128, 64, 32, 16, 8, 4. Prefixes /17 through /24 put it in the
> **third** octet, so subnets step through the third number and each carries the whole fourth octet.
>
> **Applies when** you must decide which octet to do the arithmetic in — the first decision in every
> calculation question.
>
> **Boundary:** the shift of octet is where slips happen. /25 has block size **128 in the fourth octet**;
> /17 has block size **128 in the third octet** — same value, different column, wildly different subnet
> size (126 usable hosts against 32,766). Identify the interesting octet before anything else.

**Wrong traces:** A = 255.255.252.0 · B = 255.255.254.0 · C = 255.255.255.0, the unsubnetted Class C
default.

---

**Q36.** Which statement comparing classful addressing with CIDR is correct? `[Trap]`

- **A)** CIDR requires every network to use its class's default mask
- **B)** CIDR supports only prefixes that fall on octet boundaries
- **C)** classful addressing allowed variable prefix lengths, whereas CIDR fixed them
- **D)** CIDR allows any prefix length, so the network boundary is no longer tied to the address class

**Answer: D) CIDR allows any prefix length, so the network boundary is no longer tied to the address class**

**Trace / Why:** under classful rules the first octet dictated the mask — 200.x.x.x *had* to be /24. CIDR
decouples them: the prefix is carried explicitly, so 200.10.5.0/28 or 10.0.0.0/12 are both ordinary. This
allowed allocations sized to actual need instead of to class granularity, which was the whole point.

**📘 CONCEPT — C27 · CIDR: Classless Inter-Domain Routing, and the two problems it solved**
> Classful addressing wasted addresses catastrophically — an organisation needing 300 hosts had to take
> a whole Class B (65,534 addresses) because a Class C (254) was just too small. CIDR fixed two things
> at once:
> - **address exhaustion** — allocate a /23 for 300 hosts instead of a /16;
> - **routing-table growth** — contiguous blocks can be **aggregated** into one advertised route (C41).
>
> **Applies when** the stem mentions CIDR, classless addressing, slash notation, or why classful
> addressing was abandoned.
>
> **Boundary:** CIDR did not abolish the class *ranges* — 192.168.1.1 is still described as "Class C
> space" in exam language, and BCS-style questions still ask for class and default mask (Q4–Q6). What
> CIDR abolished is the *obligation* to use the default mask. Both facts are examinable, and they
> coexist.

**Wrong traces:** A = the classful rule that CIDR removed · B = CIDR's central feature is
**non**-octet-aligned prefixes such as /26 · C = the two systems exactly swapped.

---

**Q37.** CIDR stands for `[Core]`

- **A)** Classless Inter-Domain Routing
- **B)** Class Identification and Domain Resolution
- **C)** Classful Internet Domain Registry
- **D)** Common Internet Data Routing

**Answer: A) Classless Inter-Domain Routing**

**Trace / Why:** each word carries meaning. **Classless** — the prefix is explicit, not implied by the
class. **Inter-Domain** — its purpose is routing *between* autonomous systems, where aggregation matters
most. **Routing** — it is a routing scheme, not merely a notation.

**📘 CONCEPT — C27 (see Concept Index)** › Distractors for full-form questions are built by swapping one
word, and here the decisive swap is **Classless** against **Classful** (option C) — the exact opposite of
what CIDR means. Read the first word of every option before anything else.

**Wrong traces:** B = invented expansion using "Class" and "Domain" as keywords · C = **Classful**, the
precise inversion of the answer · D = plausible-sounding but unrelated to the standard.

---

**Q38.** The prefix **/13** written as a dotted-decimal mask is `[Applied]`

- **A)** 255.248.0.0
- **B)** 255.252.0.0
- **C)** 255.255.248.0
- **D)** 255.240.0.0

**Answer: A) 255.248.0.0**

**Trace / Why:** /13 = 8 (first full octet) + **5 more** in the second octet.

```
11111111.11111000.00000000.00000000
          ^^^^^
     second octet = 128+64+32+16+8 = 248
```

Verify: /13 leaves 19 host bits, so 2¹⁹ − 2 = 524,286 usable hosts — a very large block, consistent with
a short prefix.

**📘 CONCEPT — C20 (see Concept Index)** › The octet position is decided by the prefix range: **/1–/8**
→ first octet · **/9–/16** → second · **/17–/24** → third · **/25–/32** → fourth. Here /13 sits in the
second range, so 248 belongs in the second octet. Option C has the right value in the wrong octet (/21),
which is the standard way to punish a half-remembered conversion.

**Wrong traces:** B = 6 bits, /14 · C = correct value 248 placed in the third octet, giving /21 ·
D = 4 bits, /12.

---

**Q39.** Is **255.255.255.254** a valid subnet mask? `[Trap]`

- **A)** No, because a mask cannot end in an even number
- **B)** No, because it leaves no usable host addresses
- **C)** No, because 254 is not a left-aligned bit pattern
- **D)** Yes — it is /31, valid and used for point-to-point links under RFC 3021

**Answer: D) Yes — it is /31, valid and used for point-to-point links under RFC 3021**

**Trace / Why:** 254 = 11111110, which is a legal left-aligned run of 7 one-bits, so the mask is /31 and
contiguity holds (C22). It leaves 1 host bit — 2 addresses. Under the ordinary −2 rule that would give
**zero** usable hosts, which is why /31 was long considered useless. RFC 3021 changed that: on a
**point-to-point link** there is no need for a broadcast address, so **both** addresses are assignable.

**📘 CONCEPT — C28 · /31 is the documented exception to the −2 rule**
> On a point-to-point link (a serial or routed link between exactly two interfaces), a broadcast is
> meaningless — anything either end sends reaches only the other end. RFC 3021 therefore permits /31,
> with **2 usable addresses**, halving the waste of the traditional /30.
>
> | Prefix | Addresses | Usable | Used for |
> |---|---|---|---|
> | /30 | 4 | **2** | point-to-point, the classic choice |
> | /31 | 2 | **2** | point-to-point, RFC 3021 — no waste |
> | /32 | 1 | **1** | a single host route or loopback interface |
>
> **Applies when** the stem mentions point-to-point links, WAN links between routers, or asks the most
> efficient mask for two hosts.
>
> **Boundary:** /31 works **only** on point-to-point links — on a shared LAN segment, where broadcast is
> required for ARP, it is unusable. So "what is the smallest subnet for two hosts on a LAN?" is /30,
> while "on a point-to-point link?" is /31. Note that /30 and /31 both give 2 usable addresses, for
> entirely different reasons: /30 by subtracting 2 from 4, /31 by subtracting nothing from 2.

**Wrong traces:** A = invents a parity rule; 254, 252, 248 are all legal · B = true under the general
rule, but RFC 3021 is exactly the exception — the most tempting wrong answer · C = 254 **is**
left-aligned; 127 would not be.

---

**Q40.** How many **one-bits** does the mask 255.255.192.0 contain? `[Applied]`

- **A)** 14
- **B)** 16
- **C)** 18
- **D)** 20

**Answer: C) 18**

**Trace / Why:** count octet by octet.

```
255 → 11111111 → 8
255 → 11111111 → 8
192 → 11000000 → 2
  0 → 00000000 → 0
                 ──
                 18   ⇒  /18
```

So /18, leaving 14 host bits and 2¹⁴ − 2 = 16,382 usable hosts.

**📘 CONCEPT — C8 (see Concept Index)** › Counting one-bits *is* finding the prefix, and the reliable
method is octet-by-octet addition rather than pattern recognition. Note how this question's options
pair up with the complement trap of Q34: **14 is the host-bit count** for this mask and **18 is the
prefix** — both are real properties of 255.255.192.0, and only one answers the question asked. Always
name what you computed before selecting.

**Wrong traces:** A = the **host**-bit count, the complement of the answer · B = /16, from ignoring the
192 octet entirely · D = /20, from reading 192 as four one-bits (that would be 240).

---
## Section 3 — The Three Core Formulas: Subnets, Hosts, Block Size

*Highest-yield section for calculation marks. Every question here is one of three formulas.*

**Q41.** How many usable host addresses does a **/27** subnet provide? `[Applied]` `[Asked: CCNA / bank IT — repeatedly]`

- **A)** 28
- **B)** 30
- **C)** 32
- **D)** 62

**Answer: B) 30**

**Trace / Why:**

```
host bits = 32 − 27 = 5
addresses  = 2^5     = 32
usable     = 2^5 − 2 = 30
```

**📘 CONCEPT — C29 · The host table worth memorising outright**
> | Prefix | Host bits | Addresses | **Usable hosts** | Block size |
> |---|---|---|---|---|
> | /24 | 8 | 256 | **254** | 256 |
> | /25 | 7 | 128 | **126** | 128 |
> | /26 | 6 | 64 | **62** | 64 |
> | /27 | 5 | 32 | **30** | 32 |
> | /28 | 4 | 16 | **14** | 16 |
> | /29 | 3 | 8 | **6** | 8 |
> | /30 | 2 | 4 | **2** | 4 |
>
> Reading this table from memory turns most of Sections 3 and 4 into recall rather than arithmetic.
>
> **Applies when** any question names a prefix in the /24–/30 range.
>
> **Boundary:** notice the two columns that get confused — **addresses** and **usable hosts** differ by
> exactly 2, and the block size equals the address count, never the usable count. So the increment
> between subnets is 32 for a /27 while the hosts number 30; using 30 as the increment is a guaranteed
> wrong answer in every range question (Section 4).

**Wrong traces:** A = 32 − 4, subtracting two too many · C = the **address count** with the −2 forgotten
· D = the /26 value, one prefix bit away.

---

**Q42.** How many usable host addresses does a **/22** subnet provide? `[Applied]`

- **A)** 510
- **B)** 512
- **C)** 1,024
- **D)** 1,022

**Answer: D) 1,022**

**Trace / Why:**

```
host bits = 32 − 22 = 10
addresses  = 2^10     = 1,024
usable     = 2^10 − 2 = 1,022
```

**📘 CONCEPT — C29 (see Concept Index)** › Above /24 the same table continues, and these are the values
that carry Class B subnetting questions:
**/23 → 510** · **/22 → 1,022** · **/21 → 2,046** · **/20 → 4,094** · **/19 → 8,190** · **/18 → 16,382**.
Each step down the prefix roughly doubles the previous value. Note that the pattern "one less than a
power of two, minus one more" makes these numbers instantly recognisable — anything ending in a round
power of two (512, 1024) is an *address* count, not a *usable host* count.

**Wrong traces:** A = the /23 value · B = 2⁹, a raw power of two · C = the **address count**, −2
forgotten.

---

**Q43.** A Class C network is subnetted with the mask **255.255.255.240**. How many subnets are created?
`[Applied]`

- **A)** 4
- **B)** 8
- **C)** 16
- **D)** 32

**Answer: C) 16**

**Trace / Why:** 240 = /28, and the Class C default is /24.

```
borrowed bits s = 28 − 24 = 4
subnets = 2^4 = 16
hosts per subnet = 2^(32−28) − 2 = 14
```

Check the total: 16 × 16 addresses = 256 ✓ — the four bits partition the single octet exactly.

**📘 CONCEPT — C30 · Subnets = 2^(borrowed bits), and borrowed bits come from the default prefix**
> ```
> borrowed s = new prefix − default prefix of the class
> subnets    = 2^s
> ```
> Default prefixes: Class A **/8**, Class B **/16**, Class C **/24** (C5).
>
> **Applies when** the stem names a class (or a classful base address) and a new mask.
>
> **Boundary:** the count depends on the **starting** prefix, so the same mask yields wildly different
> subnet counts from different classes — /26 gives **4** subnets from a Class C but **1,024** from a
> Class B (Q54). A question that supplies a mask but no class is under-specified for a subnet count,
> though it is fully specified for a *host* count, which needs only the new prefix. That asymmetry is
> worth internalising.

**Wrong traces:** A = 2², from /26 · B = 2³, from /27 · D = 2⁵, from /29.

---

**Q44.** Applying the mask **255.255.255.224** to the network 192.168.10.0 produces how many subnets?
`[Applied]`

- **A)** 4
- **B)** 8
- **C)** 30
- **D)** 32

**Answer: B) 8**

**Trace / Why:** 224 = /27; 192.168.10.0 is Class C, so the default is /24.

```
borrowed s = 27 − 24 = 3
subnets = 2^3 = 8
```

The eight subnets begin at .0, .32, .64, .96, .128, .160, .192, .224 — increments of the block size 32.

**📘 CONCEPT — C30 (see Concept Index)** › Two independent numbers come out of one mask and they must not
be swapped: **2^(borrowed bits) = how many subnets**, **2^(host bits) − 2 = how many hosts in each**. For
/27 on a Class C that is **8 subnets of 30 hosts**. Options C and D here are the host count and the block
size respectively — both real properties of /27, neither the answer to "how many subnets".

**Wrong traces:** A = 2², from /26 · C = the **hosts per subnet**, not the subnet count · D = the block
size, 32.

---

**Q45.** The network **172.16.0.0** is subnetted using **255.255.252.0**. How many subnets result?
`[Applied]` `[Asked: GATE-style]`

- **A)** 16
- **B)** 32
- **C)** 62
- **D)** 64

**Answer: D) 64**

**Trace / Why:** 252 = /22; 172.16.0.0 is Class B, default /16.

```
borrowed s = 22 − 16 = 6
subnets = 2^6 = 64
hosts per subnet = 2^(32−22) − 2 = 1,022
```

Sanity check: 64 subnets × 1,024 addresses = 65,536 = the whole Class B space ✓.

**📘 CONCEPT — C31 · Always verify with the conservation check**
> **subnets × addresses per subnet = addresses in the original block.**
> Here 64 × 1,024 = 65,536 = 2¹⁶ ✓. For a Class C split into /27: 8 × 32 = 256 = 2⁸ ✓.
>
> This one multiplication catches almost every arithmetic slip, because an error in either factor breaks
> the identity.
>
> **Applies when** you have computed both a subnet count and a per-subnet size — always.
>
> **Boundary:** the check uses **addresses**, not usable hosts — 64 × 1,022 = 65,408, which is *not* 2¹⁶,
> and that gap of 128 is exactly the 64 × 2 addresses consumed by the per-subnet network and broadcast
> addresses (C17). Using the usable count in the check will make a correct answer look wrong.

**Wrong traces:** A = 2⁴, from /20 · B = 2⁵, from /21 · C = 2⁶ − 2, the obsolete "subtract two subnets"
rule (Q50).

---

**Q46.** For a **/26** mask, the block size in the fourth octet is `[Applied]`

- **A)** 16
- **B)** 32
- **C)** 64
- **D)** 128

**Answer: C) 64**

**Trace / Why:** /26 gives the mask 255.255.255.**192**, and the block size is the interesting octet
subtracted from 256.

```
block size = 256 − 192 = 64
```

So subnets begin at .0, .64, .128, .192 — four blocks of 64 addresses each, 62 usable.

**📘 CONCEPT — C13 (see Concept Index)** › Block size can be found two equivalent ways, and knowing both
lets you cross-check: **256 − interesting mask octet**, or **2^(host bits within that octet)**. For /26:
256 − 192 = 64, and 2⁶ = 64 ✓. The block size is simultaneously the **increment between subnets** and the
**number of addresses in each**, which is why it is the single most useful number to compute first.

**Wrong traces:** A = the /28 block size · B = the /27 block size · D = the /25 block size — the
neighbouring prefixes on either side.

---

**Q47.** For the mask **255.255.240.0**, the block size in the **third** octet is `[Applied]`

- **A)** 4
- **B)** 8
- **C)** 12
- **D)** 16

**Answer: D) 16**

**Trace / Why:** the interesting octet is the third, holding 240.

```
block size = 256 − 240 = 16
```

So /20 subnets step through the third octet as x.y.**0**.0, x.y.**16**.0, x.y.**32**.0, x.y.**48**.0 …
each covering 16 whole third-octet values and therefore 16 × 256 = 4,096 addresses (4,094 usable).

**📘 CONCEPT — C32 · When the boundary is in the third octet, each block spans whole fourth octets**
> The interesting octet tells you where to count, and everything to its **right** belongs entirely to the
> host portion. For /20:
> - increments of **16** in the third octet;
> - the fourth octet runs its full 0–255 inside every block;
> - so one subnet is 172.16.**32**.0 through 172.16.**47**.255.
>
> **Applies when** the prefix is between /17 and /24, so the arithmetic sits in the third octet.
>
> **Boundary:** the broadcast address of such a subnet ends in **.255**, and its last usable host in
> **.254** — the fourth octet is maxed out, not the third. Reading the boundary as 172.16.47.0 (third
> octet correct, fourth octet forgotten) is the standard error here (Q64).

**Wrong traces:** A = the /22 block size · B = the /21 block size · C = 12 is not a power of two, so it
can never be a block size.

---

**Q48.** A **/30** subnet provides how many usable host addresses? `[Applied]`

- **A)** 0
- **B)** 1
- **C)** 4
- **D)** 2

**Answer: D) 2**

**Trace / Why:**

```
host bits = 32 − 30 = 2
addresses = 2^2 = 4     (network, host, host, broadcast)
usable    = 4 − 2 = 2
```

Two usable addresses is exactly what a router-to-router link needs, which is why /30 is the traditional
point-to-point mask.

**📘 CONCEPT — C33 · /30 is the smallest conventional subnet, and it exists for point-to-point links**
> Four addresses: one network, two hosts, one broadcast. 50% overhead, but it is the smallest block that
> satisfies the ordinary −2 rule and works on any medium.
>
> **Applies when** the stem mentions a WAN link, a serial link, or a connection between exactly two
> router interfaces.
>
> **Boundary:** /30 is the smallest **conventional** subnet, not the smallest possible — **/31** (2
> addresses, both usable) is more efficient on point-to-point links under RFC 3021 (Q39, Q121), and
> **/32** describes a single host. So "smallest subnet that supports two hosts" is /31 on a
> point-to-point link and /30 on anything shared.

**Wrong traces:** A = the naive result of applying −2 to a /31, not a /30 · B = a /32 host route ·
C = the **address** count, with the −2 forgotten.

---

**Q49.** How many usable hosts are available in a **/29** subnet? `[Applied]`

- **A)** 6
- **B)** 8
- **C)** 14
- **D)** 30

**Answer: A) 6**

**Trace / Why:**

```
host bits = 32 − 29 = 3
addresses = 2^3 = 8
usable    = 8 − 2 = 6
```

/29 is the standard allocation for a small block of servers or a handful of static devices.

**📘 CONCEPT — C29 (see Concept Index)** › Learn the small prefixes as a descending ladder, because VLSM
design questions (Section 6) require choosing among them under pressure:
**/26 → 62** · **/27 → 30** · **/28 → 14** · **/29 → 6** · **/30 → 2**.
Each step down halves the usable count and then loses a little more to the fixed −2, which is why the
overhead percentage climbs steeply: 3% at /26 but 50% at /30.

**Wrong traces:** B = the address count, −2 forgotten · C = the /28 value · D = the /27 value.

---

**Q50.** With **s** bits borrowed for subnetting, the number of usable subnets in **modern** practice is
`[Trap]`

- **A)** 2^s
- **B)** 2^s − 1
- **C)** 2^s − 2
- **D)** 2^(s−1)

**Answer: A) 2^s**

**Trace / Why:** older texts taught 2^s − 2, excluding the **subnet-zero** (all subnet bits 0) and the
**all-ones subnet** (all subnet bits 1). RFC 1878 and modern equipment permit both, and Cisco has
defaulted to `ip subnet-zero` for decades. So all 2^s subnets are usable.

**📘 CONCEPT — C34 · Subnet zero: the formula that changed, and why both versions still appear**
> - **Modern (correct today): subnets = 2^s.** Both the zero subnet and the all-ones subnet are usable.
> - **Historical: subnets = 2^s − 2.** Subnet zero was excluded because its subnet address was
>   indistinguishable from the parent network's address under classful routing, and the all-ones subnet
>   because its broadcast collided with the parent's broadcast.
>
> **Applies when** the stem asks for a subnet count, and especially if it says "usable subnets".
>
> **Boundary:** the **hosts** formula never changed — it is 2^h − 2 in every era, because the network and
> broadcast addresses within a subnet are genuinely unusable (C9). So one −2 survives and the other does
> not, and mixing them up is the trap. If a question's options only fit the old rule, it is testing the
> historical formula; say 2^s unless the stem signals otherwise.

**Wrong traces:** B = excludes one subnet, a rule that never existed · C = the **obsolete** formula ·
D = halves the count, which corresponds to borrowing one bit fewer.

---

**Q51.** A /24 network is divided into four **/26** subnets. The **total** number of usable host addresses
across all four subnets is `[Applied]`

- **A)** 246
- **B)** 248
- **C)** 252
- **D)** 254

**Answer: B) 248**

**Trace / Why:** each /26 has 62 usable hosts (C29).

```
4 subnets × 62 hosts = 248
```

The original unsubnetted /24 offered 254. Subnetting therefore **cost 6 usable addresses** — the four
subnets consume 8 addresses in network/broadcast overhead where the single /24 consumed only 2.

**📘 CONCEPT — C35 · Subnetting overhead: 2 addresses per subnet, always**
> ```
> addresses lost = 2 × (number of subnets)
> total usable   = (number of subnets) × (2^h − 2)
> ```
> Splitting a /24: into 2 × /25 → 252 usable · 4 × /26 → 248 · 8 × /27 → 240 · 16 × /28 → 224 ·
> 64 × /30 → 128. The finer the split, the heavier the loss.
>
> **Applies when** the stem asks for a total across subnets, or why subnetting "wastes" addresses.
>
> **Boundary:** this is the quantified form of C17 — subnetting **reduces** total usable hosts, and the
> reduction is severe at small subnet sizes (a /24 split into /30s loses half its capacity). That cost is
> exactly what **VLSM** exists to minimise: size each subnet to its actual need rather than splitting
> uniformly (Section 6).

**Wrong traces:** A = 4 × 62 miscomputed, or 254 − 8 · C = the 2 × /25 total · D = the unsubnetted /24
count, ignoring the split entirely.

---

**Q52.** Converting a Class C network from its default mask to **/29** borrows how many bits? `[Applied]`

- **A)** 3
- **B)** 4
- **C)** 6
- **D)** 5

**Answer: D) 5**

**Trace / Why:** the Class C default is /24.

```
borrowed = 29 − 24 = 5
subnets  = 2^5 = 32
hosts    = 2^3 − 2 = 6
```

Conservation check: 32 × 8 = 256 ✓ (C31).

**📘 CONCEPT — C30 (see Concept Index)** › The subtraction must use the **class default**, not 32 and not
zero. A frequent error is computing 32 − 29 = 3 and reporting 3 borrowed bits — but 3 is the **host**-bit
count, not the borrowed count. For any subnetted network there are three numbers, and all three should be
written down: **network bits = new prefix**, **borrowed bits = new prefix − default**, **host bits = 32 −
new prefix**. Here: 29, 5 and 3.

**Wrong traces:** A = the **host**-bit count (32 − 29) · B = from a /28 · C = from a /30.

---

**Q53.** The network **10.0.0.0** is subnetted with the mask **255.255.0.0**. How many subnets are created?
`[Applied]`

- **A)** 128
- **B)** 256
- **C)** 65,536
- **D)** 16,777,214

**Answer: B) 256**

**Trace / Why:** 10.0.0.0 is Class A, default /8. The mask 255.255.0.0 is /16.

```
borrowed = 16 − 8 = 8
subnets  = 2^8 = 256
hosts per subnet = 2^16 − 2 = 65,534
```

Check: 256 × 65,536 = 16,777,216 = 2²⁴ ✓, the whole Class A host space.

**📘 CONCEPT — C30 (see Concept Index)** › This is the mask that turns one Class A into 256 "Class-B-sized"
subnets — 10.0.0.0/16, 10.1.0.0/16, … 10.255.0.0/16 — which is why large private deployments use
`10.x.0.0/16` per site. The pattern is worth recognising because the second octet becomes a site number,
making the addressing plan human-readable.

**Wrong traces:** A = 2⁷ · C = 2¹⁶, the *hosts* per subnet's address count, mistaken for the subnet count ·
D = the total usable hosts in an unsubnetted Class A.

---

**Q54.** How many subnets does the mask **255.255.255.192** create when applied to **172.16.0.0**?
`[Applied]` `[Asked: GATE-style]`

- **A)** 4
- **B)** 1,024
- **C)** 62
- **D)** 4,096

**Answer: B) 1,024**

**Trace / Why:** 172.16.0.0 is **Class B**, default /16. The mask 255.255.255.192 is /26.

```
borrowed = 26 − 16 = 10
subnets  = 2^10 = 1,024
hosts per subnet = 2^6 − 2 = 62
```

Check: 1,024 × 64 = 65,536 ✓.

**📘 CONCEPT — C30 (see Concept Index)** › Compare with Q46: the **same /26 mask** gives **4** subnets from
a Class C and **1,024** from a Class B, because the borrowed-bit count is measured from a different
default. Option A here is precisely the Class C answer, planted for anyone who read "/26" and reached for
the familiar number without checking the first octet. Always identify the class before dividing.

**Wrong traces:** A = the answer for a **Class C** base — the intended trap · C = the hosts per subnet ·
D = 2¹², from mistakenly borrowing 12 bits.

---

**Q55.** The number of usable host addresses in an unsubnetted **Class A** network is `[Applied]`

- **A)** 65,534
- **B)** 16,777,214
- **C)** 16,777,216
- **D)** 2,097,152

**Answer: B) 16,777,214**

**Trace / Why:** Class A has 24 host bits.

```
2^24 − 2 = 16,777,216 − 2 = 16,777,214
```

**📘 CONCEPT — C9 (see Concept Index)** › The three classful host counts once more, because they anchor
sanity checks throughout the chapter: **A → 16,777,214**, **B → 65,534**, **C → 254**. Note the
distractor pattern here — 16,777,216 is the address count (−2 forgotten) and 2,097,152 is the number of
**Class C networks** (C10), a genuine chapter figure serving as a plausible large number. Recognising
which quantity a familiar-looking number belongs to is half the defence.

**Wrong traces:** A = the Class B host count · C = the address count, −2 forgotten · D = 2²¹, the count of
Class C **networks**, not hosts.

---

**Q56.** Using a **/26** mask, the fourth-octet subnet addresses are `[Applied]`

- **A)** 0, 32, 64, 96
- **B)** 0, 16, 32, 48
- **C)** 0, 64, 128, 192
- **D)** 0, 128

**Answer: C) 0, 64, 128, 192**

**Trace / Why:** /26 has block size 64 (C46), so subnets start at multiples of 64: **0, 64, 128, 192** —
four subnets filling the 256-address octet exactly.

| Subnet | Range | Broadcast | Usable hosts |
|---|---|---|---|
| .0/26 | .0 – .63 | .63 | .1 – .62 |
| .64/26 | .64 – .127 | .127 | .65 – .126 |
| .128/26 | .128 – .191 | .191 | .129 – .190 |
| .192/26 | .192 – .255 | .255 | .193 – .254 |

**📘 CONCEPT — C36 · Subnets always begin at multiples of the block size, starting from zero**
> The subnet addresses are **0, b, 2b, 3b, …** where b is the block size. Consequences used constantly in
> Section 4: a subnet's **broadcast** is the next subnet's address minus 1, and its **last usable host**
> is the broadcast minus 1.
>
> **Applies when** you must list subnets, or find which subnet contains a given address.
>
> **Boundary:** the multiples always start at **0**, never at 1 or at the block size — the first subnet of
> a /26 is `.0/26`, which is the *subnet zero* that old texts excluded (C34). Modern practice includes it,
> so a list of /26 subnets has four entries beginning with .0, not three beginning with .64.

**Wrong traces:** A = the /27 increments (block 32) · B = the /28 increments (block 16) · D = the /25
increments (block 128).

---

**Q57.** A **/25** subnet supports how many usable hosts? `[Applied]`

- **A)** 62
- **B)** 126
- **C)** 128
- **D)** 254

**Answer: B) 126**

**Trace / Why:**

```
host bits = 32 − 25 = 7
addresses = 2^7 = 128
usable    = 128 − 2 = 126
```

/25 splits a /24 into exactly two halves, each with 126 usable hosts — a common choice when one segment
must be divided in two.

**📘 CONCEPT — C29 (see Concept Index)** › /25 is worth singling out because it is the **only** prefix that
divides a Class C into two, and because its numbers are easily confused with the parent /24: 126 usable
against 254, block size 128 against 256. When a question involves "splitting a network in half", /25 is
the mask and 126 is the host count.

**Wrong traces:** A = the /26 value · C = the address count, −2 forgotten · D = the parent /24 value.

---

**Q58.** How many usable hosts does a **/28** subnet provide? `[Applied]`

- **A)** 14
- **B)** 16
- **C)** 30
- **D)** 32

**Answer: A) 14**

**Trace / Why:**

```
host bits = 32 − 28 = 4
addresses = 2^4 = 16
usable    = 16 − 2 = 14
```

**📘 CONCEPT — C29 (see Concept Index)** › /28 is the most commonly assigned small block — sixteen
addresses is the right size for a rack of servers or a small office segment, and a Class C splits into 16
of them exactly. Note the option pattern once more: **16 is the block size**, **30 is the /27 host count**,
**32 is the /27 block size** — three real figures from adjacent rows of the table, only one of which
answers the question.

**Wrong traces:** B = the address count and block size · C = the /27 host count · D = the /27 block size.

---

**Q59.** For a **/29** mask, the increment between consecutive subnet addresses is `[Applied]`

- **A)** 4
- **B)** 8
- **C)** 16
- **D)** 32

**Answer: B) 8**

**Trace / Why:** /29 gives the mask 255.255.255.**248**.

```
block size = 256 − 248 = 8
```

So subnets begin at .0, .8, .16, .24, .32 … thirty-two subnets of 8 addresses each within one /24
(32 × 8 = 256 ✓).

**📘 CONCEPT — C13 (see Concept Index)** › "Increment", "block size", "step" and "addresses per subnet" are
four names for **the same number**. Examiners rotate the wording, so treat them as synonyms and compute
256 − interesting octet once. The value is also the wildcard's last octet plus one (C23): wildcard
0.0.0.7, so block size 8.

**Wrong traces:** A = the /30 block size · C = the /28 block size · D = the /27 block size.

---

**Q60.** A /24 network is split into four **/26** subnets. Across all four subnets, how many addresses
**cannot** be assigned to hosts? `[Trap]`

- **A)** 2
- **B)** 8
- **C)** 16
- **D)** 32

**Answer: B) 8**

**Trace / Why:** every subnet loses its own network and broadcast address (C9).

```
4 subnets × 2 unusable each = 8 addresses
```

Cross-check against Q51: 256 total addresses − 8 unusable = **248 usable** ✓ — the two questions must
agree, and they do.

**📘 CONCEPT — C35 (see Concept Index)** › Unusable addresses scale with the **number of subnets**, not
with the size of the parent block: 2 per subnet, always. So the overhead as a fraction climbs steeply as
subnets get smaller — 0.8% for a single /24, 3.1% when split into /26s, 50% when split into /30s. This is
the quantitative argument for VLSM: allocate each segment only as much address space as it needs, so the
number of subnets stays as small as the topology permits.

**Wrong traces:** A = the loss for a **single** unsubnetted network, ignoring the split · C = 4 × 4,
subtracting four per subnet · D = the /27 split's figure miscomputed (8 × 2 = 16, not 32).

---
## Section 4 — Network Address, Broadcast Address & Valid Host Range

*The most heavily examined section in the chapter. One method answers every question here.*

**The method, once, for all 24 questions:**

```
1. Convert the prefix to a mask and find the INTERESTING octet (the one that is neither 255 nor 0).
2. block size = 256 − interesting octet value.
3. Count multiples of the block size until you pass the address's value in that octet;
   step back one — that is the NETWORK address.
4. broadcast = next network address − 1.
5. first usable = network + 1 · last usable = broadcast − 1.
```

---

**Q61.** For the host **192.168.10.100/26**, the network (subnet) address is `[Applied]` `[Asked: CCNA / bank IT]`

- **A)** 192.168.10.0
- **B)** 192.168.10.32
- **C)** 192.168.10.63
- **D)** 192.168.10.64

**Answer: D) 192.168.10.64**

**Trace / Why:**

```
/26 → mask 255.255.255.192 → interesting octet = 4th = 192
block size = 256 − 192 = 64
multiples: 0, 64, 128, 192
100 lies between 64 and 128  →  network = 192.168.10.64
```

**📘 CONCEPT — C37 · Finding the network address: step down to the nearest multiple of the block size**
> The network address is the **largest multiple of the block size that does not exceed** the host's value
> in the interesting octet. Equivalently, it is IP AND mask (C18) — but counting multiples is faster by
> hand and less error-prone under exam pressure.
>
> **Applies when** the stem gives an address with a prefix or mask and asks for the network, subnet, or
> "which subnet does this belong to".
>
> **Boundary:** "step **down**, never up" is the rule that decides it — 100 belongs to the block starting
> at 64, not the one starting at 128. Stepping up gives the *next* subnet's address, which is option A's
> sibling error and the single commonest slip in this whole section.

**Wrong traces:** A = the first subnet, from ignoring the block arithmetic · B = the /27 block boundary
(block 32) · C = the **broadcast** of the previous subnet, one below the answer.

---

**Q62.** For the host **192.168.10.100/26**, the broadcast address is `[Applied]`

- **A)** 192.168.10.127
- **B)** 192.168.10.128
- **C)** 192.168.10.191
- **D)** 192.168.10.255

**Answer: A) 192.168.10.127**

**Trace / Why:** from Q61 the network is 192.168.10.64 and the block size is 64.

```
next network = 64 + 64 = 128
broadcast    = 128 − 1 = 127
             → 192.168.10.127
```

**📘 CONCEPT — C38 · Broadcast = next network address − 1**
> Equivalently: **network + block size − 1**, or "set all host bits to 1". The three forms agree always,
> and checking one against another catches errors:
> 64 + 64 − 1 = 127 ✓.
>
> **Applies when** the stem asks for the broadcast, the last address, or the upper bound of a subnet.
>
> **Boundary:** the **−1** is what separates the broadcast from the *next subnet's* network address —
> 127 versus 128. Forgetting it produces option B, which is not merely wrong but belongs to a different
> subnet entirely. Note that the broadcast is a **real, reachable** address (packets to it reach every
> host on the subnet); it simply cannot be *assigned* to an interface.

**Wrong traces:** B = the next subnet's network address, the −1 forgotten · C = the broadcast of the
.128/26 subnet · D = the broadcast of the whole /24, from using the wrong mask.

---

**Q63.** The valid host range of the subnet containing **200.10.5.68/28** is `[Applied]`

- **A)** 200.10.5.64 to 200.10.5.79
- **B)** 200.10.5.65 to 200.10.5.78
- **C)** 200.10.5.65 to 200.10.5.79
- **D)** 200.10.5.64 to 200.10.5.78

**Answer: B) 200.10.5.65 to 200.10.5.78**

**Trace / Why:**

```
/28 → mask 255.255.255.240 → block size = 256 − 240 = 16
multiples: 0, 16, 32, 48, 64, 80 …
68 lies between 64 and 80

network      = 200.10.5.64
broadcast    = 80 − 1 = 200.10.5.79
first usable = 64 + 1 = 200.10.5.65
last usable  = 79 − 1 = 200.10.5.78      (14 hosts ✓ = 2^4 − 2)
```

**📘 CONCEPT — C39 · The four boundary addresses of every subnet, in order**
> ```
> network  |  first usable  ...  last usable  |  broadcast
>    n           n+1                 b−1            b
> ```
> Both **ends are excluded** from the usable range, which is exactly the −2 of C9 expressed as addresses
> rather than as a count. Verify by counting: 78 − 65 + 1 = 14 ✓.
>
> **Applies when** the stem asks for the valid, usable, or assignable host range.
>
> **Boundary:** the two ends fail independently, so an option set probes each separately — A includes
> **both** ends, C includes only the top end, D includes only the bottom. Check both boundaries before
> selecting, and confirm the count matches 2^h − 2.

**Wrong traces:** A = both network and broadcast included · C = broadcast included at the top ·
D = network included at the bottom.

---

**Q64.** For the host **172.16.45.200/20**, the broadcast address is `[Applied]` `[Asked: GATE-style]`

- **A)** 172.16.47.255
- **B)** 172.16.48.0
- **C)** 172.16.63.255
- **D)** 172.16.255.255

**Answer: A) 172.16.47.255**

**Trace / Why:** /20 puts the boundary in the **third** octet.

```
mask = 255.255.240.0 → interesting octet = 3rd = 240
block size = 256 − 240 = 16
multiples in 3rd octet: 0, 16, 32, 48 …
45 lies between 32 and 48  →  network = 172.16.32.0

next network = 172.16.48.0
broadcast    = one less   = 172.16.47.255
```

**📘 CONCEPT — C32 (see Concept Index)** › When the boundary is in the third octet, the broadcast has the
third octet at **one below the next multiple** and the fourth octet at its **maximum, 255**. So the answer
ends in `.47.255`, never `.47.0` and never `.48.0`. Subtracting 1 from `172.16.48.0` borrows across the
octet boundary — it becomes 47.255, exactly as 100 − 1 = 99 borrows in decimal.

**Wrong traces:** B = the next subnet's network address, the −1 forgotten · C = the /18 broadcast (block
64) · D = the broadcast of the whole Class B, from using the default mask.

---

**Q65.** For the host **172.16.45.200/20**, the network address is `[Applied]`

- **A)** 172.16.0.0
- **B)** 172.16.16.0
- **C)** 172.16.32.0
- **D)** 172.16.40.0

**Answer: C) 172.16.32.0**

**Trace / Why:** block size 16 in the third octet, multiples 0, 16, 32, 48. The value 45 sits in the block
that starts at **32**, so the network is **172.16.32.0** and the subnet spans 172.16.32.0 – 172.16.47.255.

**📘 CONCEPT — C37 (see Concept Index)** › Option D is the instructive wrong answer: **40 is not a multiple
of 16**, so it can never be a /20 network address. A fast sanity check for any network address is that its
interesting-octet value must be **divisible by the block size** — 32 ÷ 16 = 2 ✓, whereas 40 ÷ 16 = 2.5 ✗.
That single divisibility test eliminates plausible-looking options without any further work.

**Wrong traces:** A = the first /20 block, from ignoring 45 · B = the block below the correct one ·
D = 40 is not a multiple of 16, so not a valid /20 boundary.

---

**Q66.** For the host **10.5.120.77/13**, the broadcast address is `[Applied]`

- **A)** 10.5.255.255
- **B)** 10.6.255.255
- **C)** 10.7.255.254
- **D)** 10.7.255.255

**Answer: D) 10.7.255.255**

**Trace / Why:** /13 puts the boundary in the **second** octet.

```
mask = 255.248.0.0 → interesting octet = 2nd = 248
block size = 256 − 248 = 8
multiples in 2nd octet: 0, 8, 16, 24 …
5 lies between 0 and 8  →  network = 10.0.0.0

next network = 10.8.0.0
broadcast    = one less = 10.7.255.255
```

Every octet to the **right** of the interesting one is maxed out in the broadcast.

**📘 CONCEPT — C40 · With a short prefix, all octets right of the boundary fill up**
> For a boundary in the second octet, the broadcast is `a.(next−1).255.255`. For the third octet,
> `a.b.(next−1).255`. For the fourth, `a.b.c.(next−1)`. The pattern is always: **one below the next
> multiple in the interesting octet, then 255 in every octet after it.**
>
> **Applies when** the prefix is shorter than /24, so the arithmetic is not confined to the last octet.
>
> **Boundary:** the network address is the mirror image — **the multiple, then 0 in every octet after it**
> (10.0.0.0 here). Getting the interesting octet right but leaving the trailing octets at 0 in the
> broadcast produces `10.7.0.0`, a common half-answer that is neither a network nor a broadcast address.

**Wrong traces:** A = one octet short in the second position · B = two short · C = the **last usable host**,
one below the broadcast.

---

**Q67.** For the host **192.168.1.130/25**, the network address is `[Applied]`

- **A)** 192.168.1.0
- **B)** 192.168.1.128
- **C)** 192.168.1.129
- **D)** 192.168.1.255

**Answer: B) 192.168.1.128**

**Trace / Why:**

```
/25 → mask 255.255.255.128 → block size = 256 − 128 = 128
multiples: 0, 128
130 lies between 128 and 256  →  network = 192.168.1.128
broadcast = 192.168.1.255 · usable = .129 to .254 (126 hosts ✓)
```

**📘 CONCEPT — C37 (see Concept Index)** › A /25 has only **two** subnets, so the decision is a single
comparison against 128: below it the network is `.0`, at or above it the network is `.128`. Note the
distractor structure — A is the *other* subnet, C is the **first usable host** of the correct subnet, and
D is its broadcast. All four options are real addresses within the /24; only one is the network address of
the subnet containing .130.

**Wrong traces:** A = the lower /25 subnet, which .130 is not in · C = the first usable host, one above the
network · D = the broadcast of this subnet.

---

**Q68.** For the host **172.16.2.1/23**, the network address is `[Applied]` `[Asked: ExamVeda-style]`

- **A)** 172.16.2.0
- **B)** 172.16.2.1
- **C)** 172.16.3.0
- **D)** 172.16.3.255

**Answer: A) 172.16.2.0**

**Trace / Why:** /23 puts the boundary in the third octet.

```
mask = 255.255.254.0 → block size = 256 − 254 = 2
multiples in 3rd octet: 0, 2, 4, 6 …
2 is itself a multiple  →  network = 172.16.2.0
subnet spans 172.16.2.0 through 172.16.3.255
```

Note that a /23 joins **two** third-octet values (2 and 3) into one subnet of 512 addresses.

**📘 CONCEPT — C41 · A prefix shorter than /24 merges several third-octet values into one subnet**
> /23 → 2 consecutive third-octet values · /22 → 4 · /21 → 8 · /20 → 16. So `172.16.2.0/23` contains
> every address from 172.16.2.0 to 172.16.3.255, and a host at 172.16.3.100 is on the **same subnet** as
> one at 172.16.2.50.
>
> **Applies when** the prefix is between /17 and /23 and the stem asks whether two addresses with
> *different third octets* are on one subnet.
>
> **Boundary:** when the address's interesting-octet value **is already a multiple** of the block size, the
> network address equals the address itself in that octet — here 2 is a multiple of 2, so the third octet
> stays 2 and only the fourth is zeroed. Stepping down unnecessarily to 172.16.0.0 is the error this case
> invites.

**Wrong traces:** B = the address itself, with the host bits not zeroed · C = the second half of the same
subnet, not its start · D = the broadcast of the subnet.

---

**Q69.** The valid host range of **172.16.2.1/23** is `[Applied]`

- **A)** 172.16.2.0 to 172.16.3.255
- **B)** 172.16.2.1 to 172.16.2.254
- **C)** 172.16.2.0 to 172.16.3.254
- **D)** 172.16.2.1 to 172.16.3.254

**Answer: D) 172.16.2.1 to 172.16.3.254**

**Trace / Why:** network 172.16.2.0, broadcast 172.16.3.255 (from Q68).

```
first usable = network + 1   = 172.16.2.1
last usable  = broadcast − 1 = 172.16.3.254
count = 2^9 − 2 = 510 hosts ✓
```

**📘 CONCEPT — C39 (see Concept Index)** › The usable range **crosses the third-octet boundary** here,
running .2.1 → .2.255 → .3.0 → .3.254. So `172.16.2.255` and `172.16.3.0` are ordinary assignable host
addresses inside this subnet — they only *look* like a broadcast and a network address. That illusion is
the whole point of the question: an address ending in .255 or .0 is only special **relative to its own
subnet's mask**, never in the abstract.

**Wrong traces:** A = both boundaries included · B = stops at the end of the third octet's first value,
treating the subnet as a /24 · C = network address included at the bottom.

---

**Q70.** For the host **192.168.100.37/27**, the network address is `[Applied]`

- **A)** 192.168.100.32
- **B)** 192.168.100.33
- **C)** 192.168.100.63
- **D)** 192.168.100.64

**Answer: A) 192.168.100.32**

**Trace / Why:**

```
/27 → mask 255.255.255.224 → block size = 256 − 224 = 32
multiples: 0, 32, 64, 96, 128, 160, 192, 224
37 lies between 32 and 64  →  network = 192.168.100.32
broadcast = 63 · usable = .33 to .62 (30 hosts ✓)
```

**📘 CONCEPT — C37 (see Concept Index)** › /27 with block size 32 is the most frequently set calculation in
practice banks, so the eight boundaries — **0, 32, 64, 96, 128, 160, 192, 224** — are worth knowing by
heart. Reciting them turns any /27 question into a lookup. The four options here are, in order, the
network, first host, broadcast and next network: the complete boundary set of C39, which is how a
well-built option set for this section always looks.

**Wrong traces:** B = the first usable host · C = the broadcast · D = the next subnet's network address.

---

**Q71.** For the host **10.10.10.10/30**, the network address is `[Applied]`

- **A)** 10.10.10.8
- **B)** 10.10.10.9
- **C)** 10.10.10.11
- **D)** 10.10.10.12

**Answer: A) 10.10.10.8**

**Trace / Why:**

```
/30 → mask 255.255.255.252 → block size = 256 − 252 = 4
multiples: 0, 4, 8, 12, 16 …
10 lies between 8 and 12  →  network = 10.10.10.8
broadcast = 12 − 1 = 10.10.10.11
usable    = .9 and .10   (2 hosts ✓)
```

**📘 CONCEPT — C42 · /30 blocks step in fours, and the pattern repeats every four addresses**
> Within any /30 block starting at n: **n = network**, **n+1 and n+2 = the two usable hosts**,
> **n+3 = broadcast**. So the usable addresses are always the *middle two*.
>
> **Applies when** the stem gives a /30 or 255.255.255.252, which almost always signals a router-to-router
> link.
>
> **Boundary:** because /30 boundaries are multiples of 4, a valid /30 network address always ends in
> **0, 4, 8, 12, …** — so 10.10.10.9 or 10.10.10.10 can never be one. The divisibility check of C65 applies
> here too and settles the question instantly.

**Wrong traces:** B = the first usable host · C = the broadcast · D = the next /30 block's network address.

---

**Q72.** The two usable host addresses in the subnet containing **10.10.10.10/30** are `[Applied]`

- **A)** 10.10.10.9 and 10.10.10.10
- **B)** 10.10.10.8 and 10.10.10.11
- **C)** 10.10.10.10 and 10.10.10.11
- **D)** 10.10.10.8 and 10.10.10.9

**Answer: A) 10.10.10.9 and 10.10.10.10**

**Trace / Why:** the block is .8 (network), .9, .10, .11 (broadcast). Excluding both ends leaves **.9 and
.10** — and the host in the stem, .10, is indeed one of them, which is a useful consistency check.

**📘 CONCEPT — C42 (see Concept Index)** › The **middle two** rule makes /30 questions instant. It also
gives a free validation: the address given in the stem must itself be one of the two usable addresses,
otherwise the configuration would be invalid. If your computed pair does not contain the stem's address,
you have the wrong block.

**Wrong traces:** B = both **unusable** addresses, exactly inverted · C = includes the broadcast ·
D = includes the network address.

---

**Q73.** For the host **172.20.14.99/22**, the network address is `[Applied]`

- **A)** 172.20.0.0
- **B)** 172.20.8.0
- **C)** 172.20.12.0
- **D)** 172.20.14.0

**Answer: C) 172.20.12.0**

**Trace / Why:**

```
/22 → mask 255.255.252.0 → interesting octet = 3rd = 252
block size = 256 − 252 = 4
multiples in 3rd octet: 0, 4, 8, 12, 16 …
14 lies between 12 and 16  →  network = 172.20.12.0
subnet spans 172.20.12.0 – 172.20.15.255  (1,022 usable ✓)
```

**📘 CONCEPT — C41 (see Concept Index)** › A /22 groups **four** consecutive third-octet values — here 12,
13, 14 and 15. So hosts at 172.20.12.5 and 172.20.15.200 are on the same subnet despite differing in the
third octet by 3. Option D is the trap for anyone who leaves the third octet at the address's own value:
**14 is not a multiple of 4**, so it cannot begin a /22.

**Wrong traces:** A = the first /22 block · B = the block below the correct one · D = the address's own
third octet, which is not a /22 boundary.

---

**Q74.** For the host **192.168.5.201/29**, the network address is `[Applied]`

- **A)** 192.168.5.200
- **B)** 192.168.5.201
- **C)** 192.168.5.207
- **D)** 192.168.5.208

**Answer: A) 192.168.5.200**

**Trace / Why:**

```
/29 → mask 255.255.255.248 → block size = 256 − 248 = 8
multiples: … 192, 200, 208 …
201 lies between 200 and 208  →  network = 192.168.5.200
broadcast = 207 · usable = .201 to .206  (6 hosts ✓)
```

The host .201 is the **first usable** address of its subnet, which is a common configuration for a gateway.

**📘 CONCEPT — C37 (see Concept Index)** › A quick way to find the multiple without listing: **divide and
floor**. 201 ÷ 8 = 25.125, floor 25, and 25 × 8 = **200**. This scales to any block size and avoids
enumerating boundaries for large octet values — 250 ÷ 8 = 31.25 → 31 × 8 = 248, so 192.168.5.250 would be
on the .248/29 subnet.

**Wrong traces:** B = the address itself, which happens to be the first usable host · C = the broadcast ·
D = the next /29 block.

---

**Q75.** For the host **150.75.60.44/18**, the broadcast address is `[Applied]`

- **A)** 150.75.60.255
- **B)** 150.75.63.255
- **C)** 150.75.64.0
- **D)** 150.75.255.255

**Answer: B) 150.75.63.255**

**Trace / Why:**

```
/18 → mask 255.255.192.0 → interesting octet = 3rd = 192
block size = 256 − 192 = 64
multiples in 3rd octet: 0, 64, 128, 192
60 lies between 0 and 64  →  network = 150.75.0.0

next network = 150.75.64.0
broadcast    = one less = 150.75.63.255
```

**📘 CONCEPT — C40 (see Concept Index)** › Note the shape of the answer: third octet **63** (one below the
next multiple 64), fourth octet **255** (filled). Option A is the classic half-answer — it keeps the
address's own third octet (60) and fills only the fourth, giving a value that is inside the subnet but is
just an ordinary host address, not the broadcast. Always take the interesting octet to *one below the next
boundary*, not to the address's own value.

**Wrong traces:** A = the address's own third octet with only the fourth filled — an ordinary host address ·
C = the next subnet's network address · D = the broadcast of the whole Class B network.

---

**Q76.** For the host **172.16.99.250/21**, the network address is `[Applied]`

- **A)** 172.16.88.0
- **B)** 172.16.92.0
- **C)** 172.16.96.0
- **D)** 172.16.99.0

**Answer: C) 172.16.96.0**

**Trace / Why:**

```
/21 → mask 255.255.248.0 → block size = 256 − 248 = 8
multiples in 3rd octet: 0, 8, 16 … 88, 96, 104 …
99 lies between 96 and 104  →  network = 172.16.96.0
subnet spans 172.16.96.0 – 172.16.103.255  (2,046 usable ✓)
```

Divide-and-floor check: 99 ÷ 8 = 12.375 → 12 × 8 = 96 ✓.

**📘 CONCEPT — C37 (see Concept Index)** › With larger octet values, listing multiples becomes slow and
error-prone, so divide-and-floor is the reliable method. Option A (88) is the *previous* block and option B
(92) is not a multiple of 8 at all — the two failure modes of hand-counting. The divisibility test settles
B without any division: 92 ÷ 8 = 11.5, so 92 cannot begin a /21.

**Wrong traces:** A = the previous /21 block · B = 92 is not a multiple of 8 · D = the address's own third
octet, which is not a boundary.

---

**Q77.** For the host **172.16.99.250/21**, the broadcast address is `[Applied]`

- **A)** 172.16.99.255
- **B)** 172.16.103.254
- **C)** 172.16.103.255
- **D)** 172.16.104.0

**Answer: C) 172.16.103.255**

**Trace / Why:** network 172.16.96.0 (Q76), block size 8 in the third octet.

```
next network = 172.16.104.0
broadcast    = one less = 172.16.103.255
last usable  = 172.16.103.254
```

**📘 CONCEPT — C38 (see Concept Index)** › The three addresses at the top of a subnet must be kept
distinct, and this option set contains all three: **.103.254 = last usable host**, **.103.255 =
broadcast**, **.104.0 = next subnet's network address**. They are consecutive, so an off-by-one in either
direction lands on a real address that answers a *different* question. Name which of the three you want
before you compute.

**Wrong traces:** A = the address's own third octet with the fourth filled, an ordinary host address ·
B = the **last usable host**, one below the broadcast · D = the next subnet's network address, one above.

---

**Q78.** The host **192.168.16.83/26** belongs to which subnet? `[Applied]`

- **A)** 192.168.16.0/26
- **B)** 192.168.16.64/26
- **C)** 192.168.16.128/26
- **D)** 192.168.16.192/26

**Answer: B) 192.168.16.64/26**

**Trace / Why:** block size 64, boundaries 0, 64, 128, 192. The value 83 lies between 64 and 128, so the
subnet is **192.168.16.64/26**, spanning .64 – .127 with usable hosts .65 – .126.

**📘 CONCEPT — C43 · "Which subnet does this host belong to" is the same question as "what is the network address"**
> Examiners phrase this four ways — "which subnet", "what is the network address", "what is the subnet id",
> "which range contains this host" — and all four are answered by the same step-down to the nearest
> multiple of the block size (C37).
>
> **Applies when** the stem gives one host address and a set of candidate subnets.
>
> **Boundary:** when the options are presented as **subnets** rather than bare addresses, you can also work
> in reverse: check which range contains 83. That is often faster with four given options, and it
> double-checks the forward calculation. Both routes must agree.

**Wrong traces:** A = the block below · C = the block above · D = the last block — the three other /26
subnets in the same /24.

---

**Q79.** Can the address **192.168.1.63** be assigned to a host on the subnet 192.168.1.0/26? `[Trap]`

- **A)** Yes, because 63 is within the range 0–63
- **B)** Yes, because only addresses ending in 0 and 255 are reserved
- **C)** No, because it is the broadcast address of the 192.168.1.0/26 subnet
- **D)** No, because addresses ending in 63 are always reserved

**Answer: C) No, because it is the broadcast address of the 192.168.1.0/26 subnet**

**Trace / Why:** the subnet 192.168.1.0/26 covers .0 – .63. Its **last** address, .63, has all six host
bits set to 1, which makes it that subnet's broadcast address. Usable hosts are .1 – .62.

**📘 CONCEPT — C44 · Whether an address is reserved depends on the mask, not on how it looks**
> "Ends in 0 or 255" is a **/24 habit**, not a rule. Under other masks:
> - `192.168.1.63/26` → **broadcast** (reserved)
> - `192.168.1.64/26` → **network** (reserved)
> - `172.16.2.255/23` → an **ordinary usable host** (Q69)
> - `172.16.3.0/23` → an **ordinary usable host**
>
> **Applies when** the stem asks whether a specific address is assignable, or offers an address that looks
> special.
>
> **Boundary:** the test is always **all host bits 0 → network** or **all host bits 1 → broadcast**,
> evaluated against *this* subnet's mask. Nothing about the decimal appearance of the last octet decides
> it, which is why option D — a rule about the number 63 — is wrong even though it reaches the right
> verdict here. Getting the right answer for the wrong reason fails the next question.

**Wrong traces:** A = the range includes .63, but the range is not the *usable* range · B = the "0 and 255"
rule is a /24-only habit · D = right verdict, invented reason; 63 is an ordinary host address under /25.

---

**Q80.** Hosts **192.168.1.100** and **192.168.1.200**, both using mask **255.255.255.128**, are `[Trap]`

- **A)** on the same subnet, since the first three octets match
- **B)** on the same subnet, since both are below 255
- **C)** on the same subnet, but require a router to communicate
- **D)** on different subnets, so traffic between them must pass through a router

**Answer: D) on different subnets, so traffic between them must pass through a router**

**Trace / Why:** the mask 255.255.255.128 is /25, block size 128, boundaries 0 and 128.

```
192.168.1.100 → 100 < 128 → network 192.168.1.0
192.168.1.200 → 200 ≥ 128 → network 192.168.1.128
                             different  ⇒  routing required
```

**📘 CONCEPT — C18 (see Concept Index)** › Matching leading octets prove nothing. Two addresses differing
only in the last octet can sit in different subnets whenever the mask is longer than /24, and this is the
most common real-world misconfiguration: an administrator assumes `192.168.1.x` is "one network" and
assigns .100 and .200 to machines on one switch, which then cannot see each other. The AND test is the
only authority.

**Wrong traces:** A = matching octets are irrelevant once the mask splits the last octet · B = "below 255"
is not a subnet test · C = self-contradictory — hosts on the *same* subnet never need a router.

---

**Q81.** Hosts **172.16.17.30** and **172.16.28.15**, both using mask **255.255.240.0**, are `[Applied]`

- **A)** on the same subnet
- **B)** on different subnets, 16 blocks apart
- **C)** on different subnets, since their third octets differ
- **D)** unable to communicate at all, as the addresses overlap

**Answer: A) on the same subnet**

**Trace / Why:** the mask is /20, block size 16 in the third octet, boundaries 0, 16, 32, 48 …

```
172.16.17.30 → 17 is between 16 and 32 → network 172.16.16.0
172.16.28.15 → 28 is between 16 and 32 → network 172.16.16.0
                                          same  ⇒  direct delivery
```

The subnet 172.16.16.0/20 spans 172.16.16.0 – 172.16.31.255, so both addresses fall comfortably inside it.

**📘 CONCEPT — C41 (see Concept Index)** › A differing third octet does **not** imply different subnets when
the prefix is shorter than /24 — a /20 absorbs sixteen third-octet values into one subnet. This is the
mirror image of Q80: there, matching octets hid a split; here, differing octets hide a shared subnet. Only
the AND test decides either case, and it is why option C's reasoning is wrong even where it might
accidentally be right.

**Wrong traces:** B = both are in the *same* block, not 16 apart · C = a differing third octet is not
decisive under a /20 · D = addresses do not overlap; both are valid and distinct.

---

**Q82.** Hosts **10.4.5.6** and **10.4.9.6**, both using **/21**, are `[Applied]`

- **A)** on the same subnet, 10.4.0.0/21
- **B)** on the same subnet, 10.4.8.0/21
- **C)** on different subnets, 10.4.0.0/21 and 10.4.8.0/21
- **D)** on different subnets, 10.4.5.0/21 and 10.4.9.0/21

**Answer: C) on different subnets, 10.4.0.0/21 and 10.4.8.0/21**

**Trace / Why:** /21 gives block size 8 in the third octet; boundaries 0, 8, 16, 24 …

```
10.4.5.6 → 5 is between 0 and 8  → network 10.4.0.0/21   (spans 10.4.0.0 – 10.4.7.255)
10.4.9.6 → 9 is between 8 and 16 → network 10.4.8.0/21   (spans 10.4.8.0 – 10.4.15.255)
                                    different subnets
```

The two addresses differ by only 4 in the third octet, yet the boundary at 8 falls between them.

**📘 CONCEPT — C18 (see Concept Index)** › Proximity is not membership. Addresses can be numerically close
and still straddle a block boundary — 10.4.7.255 and 10.4.8.0 are consecutive addresses in *different*
/21 subnets. Always locate the **boundary** rather than measuring the gap between the two addresses.
Option D shows the other standard error: quoting each address's own third octet as its network, when 5 and
9 are not multiples of 8.

**Wrong traces:** A and B = both claim one shared subnet, ignoring the boundary at 8 · D = correct verdict
with wrong network addresses; neither 5 nor 9 is a /21 boundary.

---

**Q83.** In any conventional IPv4 subnet, the **last usable host** address is `[Trap]`

- **A)** one less than the broadcast address
- **B)** equal to the broadcast address
- **C)** one less than the next subnet's network address
- **D)** the last address of the block

**Answer: A) one less than the broadcast address**

**Trace / Why:** the block ends with the broadcast, so the last address a host may occupy is the one before
it.

```
… last usable | broadcast | next network …
     b − 1          b          b + 1
```

Option C describes the **broadcast** itself (next network − 1 = broadcast), and option D describes it too —
both are one address too high.

**📘 CONCEPT — C39 (see Concept Index)** › Three descriptions of the top of a block sound alike and are
**not** interchangeable: **"last address of the block" = broadcast**, **"last usable host" = broadcast −
1**, **"next network address" = broadcast + 1**. Options B, C and D here are all the same address described
three ways, which is what makes this a clean test of the distinction rather than of arithmetic.

**Wrong traces:** B = the broadcast cannot be assigned to a host · C = next network − 1 **is** the
broadcast · D = the last address of the block **is** the broadcast.

---

**Q84.** A host is configured as **192.168.16.83/26**. Which address may be assigned to another host on the
**same** subnet? `[Applied]`

- **A)** 192.168.16.60
- **B)** 192.168.16.100
- **C)** 192.168.16.128
- **D)** 192.168.16.63

**Answer: B) 192.168.16.100**

**Trace / Why:** from Q78 the subnet is 192.168.16.64/26, spanning .64 – .127 with usable hosts **.65 –
.126**.

| Candidate | Verdict |
|---|---|
| .60 | in the **.0/26** subnet — a different subnet |
| **.100** | inside .65 – .126 ✔ **valid** |
| .128 | the **.128/26** subnet's network address — different subnet, and reserved |
| .63 | the broadcast of the **.0/26** subnet — different subnet, and reserved |

**📘 CONCEPT — C45 · Validating a candidate address needs two checks, not one**
> 1. **Is it in the right subnet?** Compare its network address with the reference host's.
> 2. **Is it usable?** It must be neither the network nor the broadcast address of that subnet.
>
> An address can fail either test independently, and a full option set probes both.
>
> **Applies when** the stem asks which address is valid for a host, or which address to assign to a new
> device.
>
> **Boundary:** the two failures are different faults with different symptoms — a wrong-subnet address
> makes the host unreachable *through the gateway*, while a reserved address may appear to configure and
> then break ARP or broadcast behaviour. Note that .63 and .128 here fail **both** tests at once, which is
> why they are the strongest distractors.

**Wrong traces:** A = in the .0/26 subnet · C = the network address of the next subnet · D = the broadcast
address of the previous subnet.

---
