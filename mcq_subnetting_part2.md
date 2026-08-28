# Subnetting — MCQ Question Bank · Part 2
**Subject:** Computer Networks · **Target:** BCS Preliminary / Bank IT Officer / NTRCA / GATE / CCNA-style
**Questions:** 46 (Q85–Q130) · **Batch:** Part 2 of 2, written in the same run as Part 1

> How to use: attempt the question first, then read the explanation. The wrong
> options matter more than the right one — that's what the examiner is testing.

**Continues Part 1 directly.** Part 1 (Q1–Q84) covered IPv4 addressing and classes, special and private
addresses, masks and CIDR prefixes, the three core formulas, and network/broadcast/host-range
calculations. This file completes the chapter: **subnet design, VLSM, supernetting and route
summarization, longest prefix match, the /31 and /32 exceptions, subnet zero, IPv6 subnetting, and
practical diagnostics.** Concept ids continue from Part 1, starting at **C46**.

---

## Section 5 — Subnet Design: Choosing the Mask

*Part 1 asked "what does this mask give?" This section asks the reverse — the harder direction.*

**Q85.** A subnet must accommodate **50 hosts**. What is the longest prefix (most efficient mask) that
satisfies this? `[Applied]`

- **A)** /23
- **B)** /24
- **C)** /25
- **D)** /26

**Answer: D) /26**

**Trace / Why:** work down the prefixes until the host count falls below the requirement.

| Prefix | Usable hosts | Enough for 50? |
|---|---|---|
| /24 | 254 | yes, but wasteful |
| /25 | 126 | yes, but wasteful |
| **/26** | **62** | **yes — smallest that fits** |
| /27 | 30 | **no** |

So /26 is the longest prefix (smallest block) that still holds 50 hosts. It wastes 12 addresses; /27
would fall 20 short.

**📘 CONCEPT — C46 · Sizing a subnet from a host requirement: round the host count UP to a power of two**
> ```
> find the smallest h with 2^h − 2 ≥ required hosts
> prefix = 32 − h
> ```
> The practical shortcut is a table read backwards: 2 → /30 · 6 → /29 · 14 → /28 · 30 → /27 · 62 → /26 ·
> 126 → /25 · 254 → /24 · 510 → /23 · 1,022 → /22 · 2,046 → /21 · 4,094 → /20.
>
> **Applies when** the stem gives a host requirement and asks for a mask or prefix.
>
> **Boundary:** you must round **up**, never to the nearest — a requirement of 50 needs /26 (62 hosts),
> and choosing /27 (30 hosts) because 50 is "closer" to 30 than to 62 fails outright. Also remember the
> **+2**: a requirement of exactly 62 hosts needs /26, but a requirement of exactly 64 hosts needs /25,
> because 2⁶ − 2 = 62 < 64. Requirements sitting exactly on a power of two are the trap.

**Wrong traces:** A = /23 gives 510 hosts, correct but grossly wasteful — not the *longest* prefix ·
B = 254 hosts, still wasteful · C = 126 hosts, wasteful by a factor of two.

---

**Q86.** A segment must support **1,000 hosts**. The most efficient prefix is `[Applied]` `[Asked: GATE-style]`

- **A)** /19
- **B)** /20
- **C)** /21
- **D)** /22

**Answer: D) /22**

**Trace / Why:**

```
/22 → 2^10 − 2 = 1,022 hosts  ≥ 1,000 ✔
/23 → 2^9  − 2 =   510 hosts  < 1,000 ✘
```

So /22 is the longest prefix that fits, wasting only 22 addresses. Note that classful addressing would
have forced a whole Class B (65,534 addresses) for this requirement — the waste that CIDR was created to
eliminate (C27).

**📘 CONCEPT — C46 (see Concept Index)** › For large requirements, find the power of two just above the
requirement and read off the prefix: 1,000 → 1,024 = 2¹⁰ → /22. The habit worth building is to check the
**next tighter** prefix as well, because that comparison is what proves the answer is the *most
efficient* one rather than merely a working one — 510 < 1,000 confirms /22 is the boundary.

**Wrong traces:** A = 8,190 hosts, eight times more than needed · B = 4,094 hosts · C = 2,046 hosts —
each a working but progressively less efficient choice.

---

**Q87.** A Class C network must be divided into at least **14 subnets**. The minimum number of host bits
that must be borrowed is `[Applied]`

- **A)** 4
- **B)** 5
- **C)** 6
- **D)** 8

**Answer: A) 4**

**Trace / Why:**

```
3 bits → 2^3 =  8 subnets  < 14 ✘
4 bits → 2^4 = 16 subnets  ≥ 14 ✔
```

Borrowing 4 bits from the Class C default /24 gives /28: **16 subnets of 14 usable hosts** each.

**📘 CONCEPT — C47 · Sizing from a subnet requirement: round the subnet count UP to a power of two**
> ```
> find the smallest s with 2^s ≥ required subnets
> new prefix = default prefix + s
> ```
> 2 → 1 bit · 3–4 → 2 bits · 5–8 → 3 bits · 9–16 → 4 bits · 17–32 → 5 bits · 33–64 → 6 bits.
>
> **Applies when** the stem gives a number of subnets required.
>
> **Boundary:** this is the mirror of C46 and it also rounds **up**, but note the asymmetry: the subnet
> formula is **2^s** with no subtraction (C34), whereas the host formula is **2^h − 2**. Applying the −2
> to subnets is the classic cross-contamination — 14 subnets would then appear to need 5 bits
> (2⁵ − 2 = 30) instead of 4.

**Wrong traces:** B = from applying the obsolete 2^s − 2 rule · C = from 2⁶, far more than needed ·
D = borrows the entire host field, leaving no host bits at all.

---

**Q88.** A Class C network must provide **6 subnets** with at least **25 hosts** each. Which prefix
satisfies both requirements? `[Applied]`

- **A)** /27
- **B)** /28
- **C)** /29
- **D)** /30

**Answer: A) /27**

**Trace / Why:** test both constraints against each candidate.

| Prefix | Subnets (from /24) | Hosts each | 6 subnets? | 25 hosts? |
|---|---|---|---|---|
| **/27** | **8** | **30** | ✔ | ✔ |
| /28 | 16 | 14 | ✔ | ✘ |
| /29 | 32 | 6 | ✔ | ✘ |
| /30 | 64 | 2 | ✔ | ✘ |

Only /27 satisfies both. The host requirement is the binding constraint here, and it sets the **upper**
limit on prefix length while the subnet requirement sets the **lower** limit.

**📘 CONCEPT — C48 · Two requirements bracket the prefix from opposite sides**
> - The **subnet** requirement sets a **minimum** prefix length (you need at least s borrowed bits).
> - The **host** requirement sets a **maximum** prefix length (you cannot leave fewer than h host bits).
>
> A valid answer lies in the window between them. Here: subnets need ≥ /27 (3 bits for 6 subnets is
> /27), hosts need ≤ /27 (25 hosts needs 5 host bits) — the window is exactly one prefix wide.
>
> **Applies when** the stem states both a subnet count and a host count.
>
> **Boundary:** the window can be **empty**, and then the requirement is impossible in the given address
> space (Q89). Check both bounds before answering; satisfying only the more obvious one is how these
> questions are failed.

**Wrong traces:** B = 14 hosts, short of 25 · C = 6 hosts · D = 2 hosts — all give enough subnets but
too few hosts, which is why the host constraint decides.

---

**Q89.** A single Class C network must provide **10 subnets** with at least **30 hosts** each. This
requirement is `[Trap]`

- **A)** satisfied by /27
- **B)** satisfied by /28
- **C)** satisfied by /26
- **D)** impossible within one Class C network

**Answer: D) impossible within one Class C network**

**Trace / Why:** apply the bracket of C48.

```
10 subnets  → need 4 borrowed bits (2^4 = 16) → prefix must be at least /28
30 hosts    → need 5 host bits (2^5 − 2 = 30) → prefix must be at most  /27
```

The window is **empty**: /28 or longer gives at most 14 hosts, and /27 or shorter gives at most 8
subnets. A simple capacity check confirms it — 10 subnets × 32 addresses = 320 > 256 available.

**📘 CONCEPT — C49 · The capacity check: subnets × block size must not exceed the parent block**
> ```
> required subnets × (rounded-up addresses per subnet) ≤ addresses in the parent block
> ```
> Here 10 × 32 = 320 > 256 ⇒ impossible. This single multiplication decides feasibility before any mask
> is chosen, and it is faster than testing prefixes one by one.
>
> **Applies when** the stem gives both counts and the options include an "impossible" choice, or when a
> design seems not to fit.
>
> **Boundary:** "impossible" is a **legitimate answer** and examiners do use it — but only when the
> arithmetic genuinely closes the window. Note that the requirement *is* satisfiable with a larger parent
> block (a /23 gives 512 addresses, enough for 16 × /27), so the impossibility is relative to the Class C,
> not absolute. Read what address space the stem allows.

**Wrong traces:** A = /27 gives only 8 subnets, short of 10 · B = /28 gives only 14 hosts, short of 30 ·
C = /26 gives only 4 subnets.

---

**Q90.** How many **/26** subnets fit inside a single **/24** network? `[Applied]`

- **A)** 2
- **B)** 4
- **C)** 8
- **D)** 16

**Answer: B) 4**

**Trace / Why:** two equivalent routes to the same answer.

```
by prefix difference:  2^(26 − 24) = 2^2 = 4
by address count:      256 ÷ 64      = 4
```

The four subnets are .0/26, .64/26, .128/26 and .192/26.

**📘 CONCEPT — C50 · Nesting count = 2^(longer prefix − shorter prefix)**
> ```
> number of smaller blocks that fit = 2^(difference in prefix lengths)
> ```
> /24 into /26 → 2² = 4 · /24 into /28 → 2⁴ = 16 · /22 into /24 → 2² = 4 · /16 into /24 → 2⁸ = 256 ·
> /48 into /64 in IPv6 → 2¹⁶ = 65,536 (Q125).
>
> **Applies when** the stem asks how many subnets of one size fit in a block of another.
>
> **Boundary:** the difference must be taken **longer minus shorter**, and the result counts *blocks*, not
> hosts. Reversing the subtraction gives a negative exponent and a fractional answer, which is the signal
> that the question was read backwards (Q93). Note this formula needs no class information at all — it is
> purely the gap between two prefixes.

**Wrong traces:** A = 2¹, from a /25 · C = 2³, from a /27 · D = 2⁴, from a /28.

---

**Q91.** How many **/30** subnets can be created from one **/24** network? `[Applied]`

- **A)** 8
- **B)** 16
- **C)** 32
- **D)** 64

**Answer: D) 64**

**Trace / Why:**

```
2^(30 − 24) = 2^6 = 64
check: 256 addresses ÷ 4 per subnet = 64 ✔
```

64 point-to-point links from one Class C — but only 64 × 2 = **128 usable host addresses** out of 256, so
half the block is consumed by network and broadcast overhead (C35).

**📘 CONCEPT — C50 (see Concept Index)** › This is the extreme case of the overhead argument: /30 gives
the maximum number of subnets from a block and simultaneously the worst utilisation (50%). It is
nevertheless the right choice for router-to-router links, because those genuinely need only two
addresses — and /31 improves it further to 100% utilisation on point-to-point links (Q122). Efficiency is
judged against **need**, not in the abstract.

**Wrong traces:** A = 2³, from a /27 · B = 2⁴, from a /28 · C = 2⁵, from a /29.

---

**Q92.** How many **/27** subnets fit inside a **/22** block? `[Applied]`

- **A)** 16
- **B)** 32
- **C)** 64
- **D)** 128

**Answer: B) 32**

**Trace / Why:**

```
2^(27 − 22) = 2^5 = 32
check: /22 has 1,024 addresses ÷ 32 per /27 = 32 ✔
```

**📘 CONCEPT — C50 (see Concept Index)** › The formula is indifferent to where the prefixes sit — the gap
between /22 and /27 is 5, exactly as the gap between /19 and /24 would be, so both nest 32 blocks. That
independence is why the rule is worth trusting over intuition about octets: a /22-into-/27 split crosses
the third-octet boundary, which makes it look harder than a /24-into-/29 split, yet the count is
identical.

**Wrong traces:** A = 2⁴, from a gap of 4 · C = 2⁶, from a gap of 6 · D = 2⁷, from a gap of 7.

---

**Q93.** How many **/24** networks fit inside a **/26** block? `[Trap]`

- **A)** 4
- **B)** None — a /24 is larger than a /26, so it cannot fit inside one
- **C)** 1
- **D)** 64

**Answer: B) None — a /24 is larger than a /26, so it cannot fit inside one**

**Trace / Why:** a longer prefix means a **smaller** block (C25). A /26 holds 64 addresses; a /24 needs
256. The nesting formula returns 2^(24 − 26) = 2⁻² = ¼, and a fractional answer is the arithmetic telling
you the question is inverted.

**📘 CONCEPT — C25 (see Concept Index)** › Nesting only works **longer inside shorter**. Before applying
C50, check the direction: the block being fitted must have the **longer** prefix. Option A is the answer
to the reversed question ("how many /26 fit in a /24") and is planted for exactly that misreading — which
is why re-reading which prefix is the container is worth the two seconds it costs.

**Wrong traces:** A = the answer to the **reversed** question · C = a /26 is a *quarter* of a /24, so not
even one fits · D = 2⁶, unrelated to this prefix pair.

---

**Q94.** In designing a VLSM addressing plan, the correct **first** step is to `[Applied]`

- **A)** identify the segment with the largest host requirement and allocate for it first
- **B)** allocate the point-to-point links first, since they are the most numerous
- **C)** divide the address space into equal-sized subnets and assign them arbitrarily
- **D)** choose a single mask that satisfies the average host requirement

**Answer: A) identify the segment with the largest host requirement and allocate for it first**

**Trace / Why:** VLSM allocates from a shared pool, so a large block must start on its own natural
boundary. Allocating small blocks first scatters them and leaves no correctly-aligned space for a large
one. Working **largest first** guarantees every subsequent block finds an aligned gap.

**📘 CONCEPT — C51 · VLSM procedure: sort descending by host requirement, then allocate contiguously**
> 1. List every segment with its host requirement.
> 2. **Sort descending** by requirement.
> 3. For each in turn: round up to a prefix (C46), allocate the next aligned block, advance the pointer
>    past its broadcast address.
>
> **Applies when** the stem gives several segments with different host counts and one address block.
>
> **Boundary:** largest-first matters because of **alignment**, not merely tidiness — a /26 must begin at
> a multiple of 64, so if two /30s are placed at .0 and .4 first, the earliest legal /26 start becomes
> .64 and addresses .8–.63 are stranded. Alignment is the whole reason for the ordering rule (C54).

**Wrong traces:** B = allocating smallest first strands address space through misalignment · C = that is
**FLSM**, which VLSM exists to improve on (Q100) · D = an average-based single mask both wastes on small
segments and fails on large ones.

---

**Q95.** A Class B network must be subnetted so each subnet supports **500 hosts**. The most efficient
prefix is `[Applied]`

- **A)** /23
- **B)** /24
- **C)** /25
- **D)** /26

**Answer: A) /23**

**Trace / Why:**

```
/23 → 2^9 − 2 = 510 hosts ≥ 500 ✔
/24 → 2^8 − 2 = 254 hosts <  500 ✘
```

So /23, wasting only 10 addresses per subnet.

**📘 CONCEPT — C46 (see Concept Index)** › Note that the **class is irrelevant to the host calculation** —
500 hosts needs /23 whether the parent is a Class A, B or C block (though a Class C cannot supply a /23 at
all, being only a /24). The class matters only for counting **subnets** (C30). Separating those two
dependencies is what keeps design questions straight: hosts depend on the new prefix alone, subnets depend
on the gap from the default.

**Wrong traces:** B = 254 hosts, short of 500 · C = 126 hosts · D = 62 hosts.

---

**Q96.** Using **/23** subnets on a Class B network, how many subnets are created? `[Applied]`

- **A)** 32
- **B)** 64
- **C)** 128
- **D)** 256

**Answer: C) 128**

**Trace / Why:** Class B default /16.

```
borrowed = 23 − 16 = 7
subnets  = 2^7 = 128
```

Conservation check: 128 subnets × 512 addresses = 65,536 = 2¹⁶ ✔ (C31).

**📘 CONCEPT — C30 (see Concept Index)** › Q95 and Q96 are the two halves of one design problem, and it is
worth doing them together: the host requirement fixes the prefix (**/23**), and the prefix then fixes the
subnet count (**128**). A complete design answer always states both numbers plus the conservation check,
because the check is what proves the split is exact.

**Wrong traces:** A = 2⁵, from a /21 · B = 2⁶, from a /22 · D = 2⁸, from a /24.

---

**Q97.** A **/19** block contains how many complete **/24** networks? `[Applied]`

- **A)** 16
- **B)** 32
- **C)** 64
- **D)** 128

**Answer: B) 32**

**Trace / Why:**

```
2^(24 − 19) = 2^5 = 32
check: /19 has 8,192 addresses ÷ 256 = 32 ✔
```

A /19 is therefore the CIDR way of saying "32 consecutive Class C networks" — which is exactly how such
blocks were allocated to ISPs, and why summarization works on them (Section 6).

**📘 CONCEPT — C52 · Short prefixes are conveniently counted in whole /24s**
> Because a /24 is 256 addresses, any prefix shorter than /24 contains a whole number of them:
> **/23 → 2** · **/22 → 4** · **/21 → 8** · **/20 → 16** · **/19 → 32** · **/18 → 64** · **/17 → 128** ·
> **/16 → 256**.
>
> **Applies when** the stem describes a block "equivalent to N Class C networks", or asks how many /24s
> fit.
>
> **Boundary:** this table doubles as the **summarization** table (C56) — combining 4 contiguous, aligned
> /24s produces a /22, combining 32 produces a /19. Reading it in one direction sizes a block; reading it
> in the other aggregates routes.

**Wrong traces:** A = 2⁴, from a /20 · C = 2⁶, from a /18 · D = 2⁷, from a /17.

---

**Q98.** When a design requires **20 subnets**, borrowing 4 bits gives 16 and borrowing 5 bits gives 32.
The correct choice is `[Trap]`

- **A)** 4 bits, because 16 is closer to 20 than 32 is
- **B)** either, since both are within a factor of two of the requirement
- **C)** 5 bits, because the count must be at least the requirement and 16 is insufficient
- **D)** 4 bits, because the remaining 4 subnets can share existing ones

**Answer: C) 5 bits, because the count must be at least the requirement and 16 is insufficient**

**Trace / Why:** the requirement is a **floor**, not a target. 16 < 20 leaves four segments with no
subnet, so it fails regardless of being numerically closer. Borrowing 5 bits gives 32 subnets — 12 spare,
which is normal and provides room for growth.

**📘 CONCEPT — C53 · Subnetting requirements are floors, and powers of two rarely match them exactly**
> Because counts are always powers of two, the chosen value is almost always **larger** than needed, and
> the spare capacity is a feature rather than waste. The rule is identical on both axes: round **up** for
> subnets (C47) and up for hosts (C46).
>
> **Applies when** a requirement falls between two powers of two — which is the usual case.
>
> **Boundary:** "closest" is never the criterion, and this is the single most common conceptual error in
> design questions. The one place rounding up can fail is when it breaches the capacity check (C49) — if
> 32 subnets of the required size will not fit in the parent block, the design is impossible rather than
> merely tight, and that is a different answer.

**Wrong traces:** A = applies "nearest" where "at least" is required — the intended trap · B = 16 does not
meet the requirement at all, so the two are not equivalent · D = sharing subnets defeats the purpose of
separating segments.

---
## Section 6 — VLSM (Variable Length Subnet Masking)

*Questions Q102–Q105, Q109 and Q110 all refer to the worked design in the box below.*

**The reference design.** The block **192.168.1.0/24** must serve five segments needing **60, 28, 12, 2
and 2** hosts. Allocating largest-first (C51):

| Order | Requirement | Prefix | Block | Usable hosts | Broadcast |
|---|---|---|---|---|---|
| 1 | 60 hosts | /26 | **192.168.1.0/26** | .1 – .62 | .63 |
| 2 | 28 hosts | /27 | **192.168.1.64/27** | .65 – .94 | .95 |
| 3 | 12 hosts | /28 | **192.168.1.96/28** | .97 – .110 | .111 |
| 4 | 2 hosts | /30 | **192.168.1.112/30** | .113 – .114 | .115 |
| 5 | 2 hosts | /30 | **192.168.1.116/30** | .117 – .118 | .119 |

Next free address: **192.168.1.120**. Addresses still unallocated: **136**.

---

**Q99.** VLSM stands for `[Core]`

- **A)** Variable Length Subnet Masking
- **B)** Virtual Local Subnet Mapping
- **C)** Variable Layer Subnet Multiplexing
- **D)** Virtual Link State Metric

**Answer: A) Variable Length Subnet Masking**

**Trace / Why:** each word is load-bearing. **Variable Length** — different subnets of the same network
may use different prefix lengths. **Subnet Masking** — it is a masking technique, not a routing protocol.

**📘 CONCEPT — C54 · VLSM: subnetting a subnet, so each segment is sized to its actual need**
> Classical (fixed-length) subnetting applies **one** mask to a whole network, so every subnet is the same
> size and a point-to-point link needing 2 addresses gets the same block as a floor needing 60. VLSM
> subnets the subnets: apply /26 to one part, /30 to another, all within one /24.
>
> **Applies when** the stem shows several segments with very different host counts, or mentions "different
> masks in the same network".
>
> **Boundary:** VLSM is a **planning technique**, not a protocol feature you enable. What it *requires*
> is that the routing protocol carry the mask with each advertisement (C57) — which classless protocols do
> and classful ones do not. So VLSM is a design choice enabled by a protocol capability.

**Wrong traces:** B = "Virtual Local" imports VLAN vocabulary · C = "Multiplexing" belongs to the physical
layer · D = a routing-metric term, unrelated to masking.

---

**Q100.** Compared with fixed-length subnet masking (FLSM), the principal advantage of VLSM is that it
`[Trap]`

- **A)** removes the need for a router between subnets
- **B)** increases the number of usable hosts in each individual subnet
- **C)** allows a subnet to use a mask shorter than its network's default
- **D)** wastes far fewer addresses, because each subnet is sized to its own requirement

**Answer: D) wastes far fewer addresses, because each subnet is sized to its own requirement**

**Trace / Why:** under FLSM every subnet in the reference design would need /26 (the largest requirement),
consuming 64 addresses even for a 2-host link. VLSM gives the 2-host links /30 blocks of 4 addresses — a
saving of 60 addresses per link.

**📘 CONCEPT — C55 · Quantifying the VLSM saving**
> For the reference design:
> - **FLSM at /26:** 5 × 64 = **320 addresses** required — which **does not fit** in a /24 at all (Q110).
> - **VLSM:** 64 + 32 + 16 + 4 + 4 = **120 addresses** used, leaving 136 free.
>
> The saving grows with the *spread* of requirements: where all segments are similar, VLSM gains little.
>
> **Applies when** the stem contrasts VLSM with a single fixed mask, or asks why an FLSM design does not
> fit.
>
> **Boundary:** VLSM does **not** create addresses — it stops wasting them. The usable count in each
> individual subnet is fixed by that subnet's own prefix exactly as before (option B inverts this), and
> inter-subnet traffic still needs a router (option A). What changes is only the *allocation*, which is
> why the benefit appears as free space at the end of the block rather than as larger subnets.

**Wrong traces:** A = routing between subnets is still required · B = per-subnet host counts come from the
prefix and are unchanged · C = subnetting always **lengthens** the prefix (C24); shortening it is
supernetting.

---

**Q101.** VLSM permits `[Applied]`

- **A)** two different networks to share one subnet mask
- **B)** different subnets of the same major network to use different prefix lengths
- **C)** a host to be configured with two subnet masks simultaneously
- **D)** the network portion of an address to be non-contiguous

**Answer: B) different subnets of the same major network to use different prefix lengths**

**Trace / Why:** that is the definition. In the reference design 192.168.1.0/24 carries a /26, a /27, a
/28 and two /30s at once — five different prefix lengths inside one Class C network.

**📘 CONCEPT — C54 (see Concept Index)** › The word "variable" applies **within one network**, which is
what makes VLSM different from merely choosing a mask. Note what stays fixed: each individual **subnet**
still has exactly one mask, and each **host** still has exactly one mask (option C). The variation is
across subnets, never within one.

**Wrong traces:** A = sharing one mask across networks is ordinary, not VLSM · C = a host has one address
and one mask · D = masks must always be contiguous (C22), VLSM or not.

---

**Q102.** In the reference design, the block allocated to the **first 2-host** segment is `[Applied]`

- **A)** 192.168.1.108/30
- **B)** 192.168.1.110/30
- **C)** 192.168.1.111/30
- **D)** 192.168.1.112/30

**Answer: D) 192.168.1.112/30**

**Trace / Why:** allocation is contiguous, so each block starts immediately after the previous
broadcast.

```
/28 block ends at broadcast .111
next free address        = .112
/30 boundaries are multiples of 4: … 108, 112, 116 …
112 is a multiple of 4  →  192.168.1.112/30  ✔ aligned
```

**📘 CONCEPT — C56 · Every VLSM block must start on a boundary that is a multiple of its own size**
> A /30 must begin at a multiple of 4, a /28 at a multiple of 16, a /27 at a multiple of 32, a /26 at a
> multiple of 64. So after finishing one block you advance to the next free address and then, if
> necessary, **round up** to the next legal boundary for the block you are about to place.
>
> **Applies when** you must state where a particular VLSM block begins.
>
> **Boundary:** the alignment requirement is why **largest-first** ordering matters (C51). Here .112
> happens to be aligned already, so nothing is wasted; had the previous block ended at .110, the /30 would
> have had to start at .112 anyway and .110–.111 would be stranded. Options A, B and C are all inside or
> below the free space, and only D is both free and aligned.

**Wrong traces:** A = .108/30 overlaps the /28 block, which runs to .111 · B = .110 is not a multiple of 4
and overlaps the /28 · C = .111 is the /28's broadcast address and is not a multiple of 4.

---

**Q103.** In the reference design, the block allocated to the **60-host** segment is `[Applied]`

- **A)** 192.168.1.0/26
- **B)** 192.168.1.0/25
- **C)** 192.168.1.0/27
- **D)** 192.168.1.64/26

**Answer: A) 192.168.1.0/26**

**Trace / Why:** 60 hosts needs 6 host bits (2⁶ − 2 = 62 ≥ 60), so /26 (C46). Being the largest
requirement it is allocated first, starting at the beginning of the block: **192.168.1.0/26**, usable
.1 – .62.

**📘 CONCEPT — C51 (see Concept Index)** › The first allocation always starts at the **base of the parent
block**, so its network address equals the parent's. Note the distractor design here: B is the correct
start with an over-generous prefix (/25 = 126 hosts, wasteful), C is the correct start with an insufficient
prefix (/27 = 30 hosts, fails), and D is the correct prefix at the wrong offset. A complete answer needs
both the right size and the right position.

**Wrong traces:** B = /25 gives 126 hosts — works but wastes 64 addresses, so not the VLSM choice ·
C = /27 gives only 30 hosts, insufficient for 60 · D = correct prefix, but .64 is where the *second*
block starts.

---

**Q104.** In the reference design, the block allocated to the **28-host** segment is `[Applied]`

- **A)** 192.168.1.60/27
- **B)** 192.168.1.64/27
- **C)** 192.168.1.64/26
- **D)** 192.168.1.96/27

**Answer: B) 192.168.1.64/27**

**Trace / Why:** 28 hosts needs 5 host bits (2⁵ − 2 = 30 ≥ 28), so /27. The /26 before it ended at
broadcast .63, so the next free address is **.64** — which is a multiple of 32 and therefore a legal /27
boundary.

```
192.168.1.64/27  →  usable .65 – .94, broadcast .95
```

**📘 CONCEPT — C56 (see Concept Index)** › Check alignment as a habit: 64 ÷ 32 = 2 exactly ✔. Option A
fails it (60 ÷ 32 = 1.875) and also overlaps the previous /26. Option C is aligned and free but is the
wrong *size* — a /26 would give 62 hosts where 30 suffice, wasting 32 addresses and defeating the point of
VLSM. Both size and alignment must hold.

**Wrong traces:** A = 60 is not a multiple of 32 and overlaps the /26 block · C = correct start, but /26
over-allocates by 32 addresses · D = .96 is where the *third* block starts.

---

**Q105.** After all five segments in the reference design are allocated, the next free address is
`[Applied]`

- **A)** 192.168.1.116
- **B)** 192.168.1.118
- **C)** 192.168.1.120
- **D)** 192.168.1.124

**Answer: C) 192.168.1.120**

**Trace / Why:** the fifth block is 192.168.1.116/30, whose broadcast is **.119**.

```
next free = last broadcast + 1 = 119 + 1 = 192.168.1.120
```

Running total consumed: 64 + 32 + 16 + 4 + 4 = **120 addresses**, .0 through .119 — so the next free
address conveniently equals the number of addresses used.

**📘 CONCEPT — C57 · The allocation pointer advances past the broadcast, not past the last host**
> ```
> next free address = previous block's broadcast + 1
> ```
> The commonest error is advancing from the last *usable host* instead — that would give .119 here, which
> is actually the previous block's broadcast and cannot start a new subnet.
>
> **Applies when** the stem asks for the next available block or the next free address.
>
> **Boundary:** the next free address is not necessarily where the next block **starts** — if that block
> needs a larger prefix, you must round up to its alignment boundary (C56). Here .120 is a multiple of 4
> and 8, so it could start a /30 or /29 immediately, but a /27 placed next would have to begin at .128,
> stranding .120–.127.

**Wrong traces:** A = the fifth block's own network address · B = the fifth block's last usable host ·
D = a /30 boundary one block too far, skipping .120–.123.

---

**Q106.** VLSM can only be deployed if the routing protocol in use `[Trap]`

- **A)** supports equal-cost load balancing
- **B)** uses a link-state rather than a distance-vector algorithm
- **C)** advertises the subnet mask along with each network address
- **D)** is configured with static routes only

**Answer: C) advertises the subnet mask along with each network address**

**Trace / Why:** with VLSM, a network address alone is ambiguous — 192.168.1.64 could be a /26, /27 or /30.
A receiving router cannot compute the correct network boundary unless the mask travels with the update.
Protocols that carry it are **classless**; those that do not are **classful** and assume the mask of the
receiving interface, which breaks as soon as masks differ.

**📘 CONCEPT — C58 · Classless routing protocols carry the mask; classful ones infer it**
> | | Classful | Classless |
> |---|---|---|
> | Mask in updates | **no** | **yes** |
> | Supports VLSM | no | yes |
> | Supports discontiguous subnets | no | yes |
> | Examples | **RIPv1, IGRP** | **RIPv2, EIGRP, OSPF, IS-IS, BGP** |
>
> **Applies when** the stem asks what VLSM requires, or names a protocol and asks whether VLSM works.
>
> **Boundary:** the requirement is about **carrying the mask**, not about the algorithm family — RIPv2 is
> a distance-vector protocol and fully supports VLSM, which is why option B is wrong despite sounding
> sophisticated. Link-state versus distance-vector is an orthogonal classification.

**Wrong traces:** A = load balancing is unrelated to mask propagation · B = RIPv2 is distance-vector and
classless, so the algorithm family does not decide it · D = static routes work, but VLSM does not *require*
them.

---

**Q107.** Which pair of routing protocols does **NOT** support VLSM? `[Applied]`

- **A)** OSPF and IS-IS
- **B)** RIPv2 and EIGRP
- **C)** BGP and OSPF
- **D)** RIPv1 and IGRP

**Answer: D) RIPv1 and IGRP**

**Trace / Why:** RIPv1 and IGRP are the two classic **classful** protocols — their update packets have no
field for a subnet mask, so a receiver must assume a mask and VLSM becomes impossible. Both are obsolete
for precisely this reason.

**📘 CONCEPT — C58 (see Concept Index)** › The two-name list is worth memorising as a pair, because every
other commonly examined protocol is classless: **RIPv1 and IGRP are classful; RIPv2, EIGRP, OSPF, IS-IS
and BGP are classless.** Note the version boundary within one protocol family — **RIPv1 is classful,
RIPv2 is classless** — which is the distinction the "v1/v2" in a question is always testing.

**Wrong traces:** A, B and C = all classless pairs that fully support VLSM; B is the sharpest distractor,
since RIPv**2** is one version away from the classful RIPv1.

---

**Q108.** In a VLSM plan, a **/27** block may begin at which of these addresses? `[Trap]`

- **A)** 192.168.5.16
- **B)** 192.168.5.40
- **C)** 192.168.5.100
- **D)** 192.168.5.128

**Answer: D) 192.168.5.128**

**Trace / Why:** a /27 has block size 32, so it must start at a multiple of 32: **0, 32, 64, 96, 128, 160,
192, 224**.

| Candidate | ÷ 32 | Legal? |
|---|---|---|
| 16 | 0.5 | ✘ |
| 40 | 1.25 | ✘ |
| 100 | 3.125 | ✘ |
| **128** | **4** | **✔** |

**📘 CONCEPT — C56 (see Concept Index)** › The divisibility test settles every alignment question in one
division, and it applies to any prefix: a **/26** start must be divisible by 64, a **/28** by 16, a
**/29** by 8, a **/30** by 4. Note that 16 is a legal **/28** start and 40 is a legal **/29**-adjacent
value but not a boundary at all — an address can be a valid start for a *smaller* block and still be
illegal for a larger one, because the alignment requirement tightens as blocks grow.

**Wrong traces:** A = 16 is a legal /28 boundary but not a /27 one · B = 40 is not a multiple of 8, let
alone 32 · C = 100 is not a multiple of 32.

---

**Q109.** In the reference design, how many addresses of the /24 remain **unallocated**? `[Applied]`

- **A)** 128
- **B)** 136
- **C)** 140
- **D)** 144

**Answer: B) 136**

**Trace / Why:**

```
consumed = 64 + 32 + 16 + 4 + 4 = 120   (addresses .0 through .119)
total    = 256
remaining = 256 − 120 = 136             (addresses .120 through .255)
```

Cross-check against Q105: the next free address is .120, and 255 − 120 + 1 = 136 ✔.

**📘 CONCEPT — C55 (see Concept Index)** › Sum the **block sizes**, never the usable host counts, when
measuring consumption — the network and broadcast addresses of each block are consumed too. Summing
usable counts here would give 62 + 30 + 14 + 2 + 2 = 110 and a remainder of 146, which is wrong by the 10
addresses lost to the five subnets' overhead (5 × 2). That distinction is the arithmetic heart of C35.

**Wrong traces:** A = 256 − 128, from rounding consumption up to a power of two · C = from summing usable
hosts and mis-correcting · D = 256 − 112, from stopping the count at the fourth block.

---

**Q110.** If the reference design's five segments were instead given a **single fixed /26 mask** (FLSM),
the result would be that `[Applied]`

- **A)** all five segments would fit, with one /26 left spare
- **B)** the design would not fit, since five /26 blocks need 320 addresses but only 256 are available
- **C)** all five would fit, but the 60-host segment would be one address short
- **D)** the design would fit only if the two 2-host links shared a single subnet

**Answer: B) the design would not fit, since five /26 blocks need 320 addresses but only 256 are available**

**Trace / Why:** FLSM must use one mask for every subnet, and it must be large enough for the **biggest**
requirement — 60 hosts, so /26.

```
5 subnets × 64 addresses = 320 > 256 available   ⇒ impossible
```

A /24 holds only **four** /26 blocks (Q90). The same five segments fit comfortably under VLSM using 120
addresses, which is the clearest possible demonstration of why VLSM exists.

**📘 CONCEPT — C55 (see Concept Index)** › The capacity check of C49 applies to FLSM directly: **number of
subnets × block size for the largest requirement ≤ parent block size.** When it fails, the choice is
either a larger parent block or VLSM. Note that FLSM is not merely wasteful here — it is *infeasible*,
which is a stronger statement and the one the question tests.

**Wrong traces:** A = a /24 holds four /26 blocks, not six · C = /26 gives 62 usable hosts, which is
enough for 60 — the shortfall is in block count, not host count · D = sharing a subnet between two
point-to-point links defeats the segmentation the design requires.

---
## Section 7 — CIDR, Supernetting, Route Summarization & Longest Prefix Match

*GATE's favourite corner of the chapter. Everything here shortens a prefix instead of lengthening it.*

**Q111.** Supernetting is the process of `[Core]`

- **A)** dividing one network into several smaller subnets
- **B)** translating private addresses into a single public address
- **C)** combining several contiguous smaller networks into one larger block with a shorter prefix
- **D)** assigning two subnet masks to a single interface

**Answer: C) combining several contiguous smaller networks into one larger block with a shorter prefix**

**Trace / Why:** supernetting moves the network boundary **left**, so the prefix gets shorter and the block
gets bigger. Four /24 networks become one /22. It is the exact inverse of subnetting, which moves the
boundary right (C24).

**📘 CONCEPT — C59 · Supernetting and subnetting are opposite directions along the same axis**
> | | Subnetting | Supernetting |
> |---|---|---|
> | Prefix | **longer** | **shorter** |
> | Block size | smaller | larger |
> | Bits move from | host → network | network → host |
> | Purpose | segment a network, cut broadcast domains | shrink routing tables (C60) |
> | Also called | — | **route aggregation / summarization** |
>
> **Applies when** the stem describes networks being combined, or a prefix being shortened.
>
> **Boundary:** supernetting is what makes CIDR's second benefit real — one advertised /22 replaces four
> /24 entries in every downstream routing table (C27). Note that "supernet", "aggregate" and "summary
> route" are three names for the same object, and examiners rotate them freely.

**Wrong traces:** A = **subnetting**, the opposite direction · B = NAT (Q130) · D = no interface takes two
masks (C54 boundary).

---

**Q112.** The four networks **172.16.8.0/24, 172.16.9.0/24, 172.16.10.0/24** and **172.16.11.0/24** can be
summarized as `[Applied]` `[Asked: CCNA / GATE-style]`

- **A)** 172.16.8.0/21
- **B)** 172.16.8.0/22
- **C)** 172.16.8.0/23
- **D)** 172.16.8.0/24

**Answer: B) 172.16.8.0/22**

**Trace / Why:** four blocks need 2 bits of aggregation, so shorten the prefix by 2: 24 − 2 = **/22**.

```
third octets 8, 9, 10, 11  → 4 consecutive values
/22 block size in 3rd octet = 256 − 252 = 4
8 ÷ 4 = 2 exactly            → 8 is a valid /22 boundary ✔
172.16.8.0/22 spans 172.16.8.0 – 172.16.11.255 — exactly the four networks ✔
```

**📘 CONCEPT — C60 · Summarization: shorten the prefix by log₂(number of blocks)**
> ```
> summary prefix = original prefix − log₂(count of blocks)
> summary address = the FIRST network address in the group
> ```
> 2 blocks → −1 bit · 4 blocks → −2 · 8 blocks → −3 · 16 blocks → −4 · 32 blocks → −5.
>
> **Applies when** the stem lists consecutive networks and asks for one summary route.
>
> **Boundary:** the arithmetic only works if **three conditions all hold** — the blocks are
> **contiguous**, their count is a **power of two**, and the first address is **aligned** to the summary
> block size. Break any one and the group cannot be summarized exactly (Q114, Q117). Always verify
> alignment with the divisibility test before answering.

**Wrong traces:** A = /21 covers eight /24s (8–15), so it over-includes four networks not in the group ·
C = /23 covers only two (8–9) · D = /24 is a single network, no aggregation at all.

---

**Q113.** The principal benefit of route summarization is that it `[Core]`

- **A)** reduces the number of entries a router must hold in its routing table
- **B)** increases the number of usable host addresses in each network
- **C)** allows two networks to share one broadcast domain
- **D)** removes the need for a subnet mask in routing updates

**Answer: A) reduces the number of entries a router must hold in its routing table**

**Trace / Why:** advertising 172.16.8.0/22 instead of four separate /24s cuts four table entries to one.
Multiplied across the Internet, aggregation is what keeps the global routing table tractable — without it
the table would carry an entry per /24 ever allocated.

**📘 CONCEPT — C61 · Smaller routing tables mean faster lookups, less memory, and faster convergence**
> Each removed entry saves router memory, shortens the longest-prefix-match search (C62), and reduces the
> update traffic and recomputation when a route changes. Aggregation also **hides internal instability**:
> if 172.16.9.0/24 flaps, neighbours advertising only the /22 never see it.
>
> **Applies when** the stem asks why summarization is done, or what CIDR achieved for routing.
>
> **Boundary:** the hiding of instability is a genuine benefit *and* a genuine drawback — a summary route
> is advertised as reachable even when one component network is down, so packets for the failed part are
> drawn in and then dropped. That trade-off is the honest answer to "are there disadvantages", and the
> reason aggregation is applied at administrative boundaries rather than everywhere.

**Wrong traces:** B = host counts come from each network's own prefix and are unchanged · C = summarization
is a routing operation; broadcast domains are unaffected · D = classless updates still carry masks (C58)
— indeed they must.

---

**Q114.** Can **172.16.16.0/20** and **172.16.32.0/20** be summarized into a single route? `[Trap]`

- **A)** Yes — 172.16.16.0/19
- **B)** Yes — 172.16.0.0/19
- **C)** No — 172.16.16.0 is not aligned to a /19 boundary, so no single /19 contains exactly these two
- **D)** No — the two blocks are not contiguous

**Answer: C) No — 172.16.16.0 is not aligned to a /19 boundary, so no single /19 contains exactly these two**

**Trace / Why:** the two blocks **are** contiguous (16.0–31.255 then 32.0–47.255), so contiguity is not the
problem. Alignment is.

```
/19 block size in 3rd octet = 256 − 224 = 32
/19 boundaries: 0, 32, 64, 96 …
16 ÷ 32 = 0.5  →  16 is NOT a /19 boundary ✘

The /19 blocks actually available are 172.16.0.0/19  (covering 0–31)
                                  and 172.16.32.0/19 (covering 32–63)
Our pair straddles them: it needs 16–47, which no single /19 provides.
```

**📘 CONCEPT — C62 · Contiguity is necessary but not sufficient — alignment decides**
> A group of blocks is summarizable only if the **first** address is divisible by the **summary** block
> size. Test it before anything else:
> ```
> first network's interesting octet ÷ summary block size  must be a whole number
> ```
> Here 16 ÷ 32 is not whole, so the answer is no regardless of how neatly the two blocks adjoin.
>
> **Applies when** the stem asks whether a group *can* be summarized, and especially when a "No" option
> is offered.
>
> **Boundary:** the same two block sizes summarize perfectly at a different starting point —
> 172.16.**0**.0/20 and 172.16.**16**.0/20 do combine into 172.16.0.0/19, because 0 is aligned. So
> summarizability is a property of **where** the group starts, not of the blocks' sizes. Note that option
> A is the trap for anyone who applies the prefix arithmetic (−1 bit for 2 blocks) without checking
> alignment.

**Wrong traces:** A = applies the bit arithmetic while skipping the alignment check; 172.16.16.0/19 would
actually span 16.0–47.255 only if 16 were a boundary, which it is not · B = 172.16.0.0/19 covers 0–31,
which excludes 172.16.32.0/20 entirely · D = the blocks **are** contiguous, so this reason is factually
wrong even though the verdict is right.

---

**Q115.** A router's forwarding table contains **0.0.0.0/0, 10.0.0.0/8, 10.1.0.0/16** and **10.1.1.0/24**.
A packet arrives for **10.1.1.5**. Which entry is used? `[Applied]` `[Asked: GATE-style]`

- **A)** 0.0.0.0/0
- **B)** 10.0.0.0/8
- **C)** 10.1.0.0/16
- **D)** 10.1.1.0/24

**Answer: D) 10.1.1.0/24**

**Trace / Why:** check every entry, then take the **longest** prefix among the matches.

| Entry | Contains 10.1.1.5? | Prefix length |
|---|---|---|
| 0.0.0.0/0 | ✔ (matches everything) | 0 |
| 10.0.0.0/8 | ✔ | 8 |
| 10.1.0.0/16 | ✔ | 16 |
| **10.1.1.0/24** | **✔** | **24 ← longest** |

All four match; the /24 is the most specific, so it wins.

**📘 CONCEPT — C63 · Longest prefix match: the most specific route always wins**
> A router does not stop at the first match — it finds **every** matching entry and selects the one with
> the **longest prefix**. Consequences worth carrying:
> - a **default route** (0.0.0.0/0) matches everything and therefore always loses to any other match,
>   which is exactly the behaviour wanted (C16);
> - a **/32 host route** is maximally specific and beats every other entry;
> - more specific routes can be injected to override an aggregate for part of its range.
>
> **Applies when** the stem shows a forwarding table and one destination address.
>
> **Boundary:** longest prefix match is decided **before** any metric or administrative distance is
> consulted — a /24 learned by a "worse" protocol still beats a /16 learned by a "better" one. Prefix
> length is the first tiebreaker, not the last. Note also that a *longer* prefix in the table is not
> automatically chosen if it does not **match**: specificity only breaks ties among genuine matches.

**Wrong traces:** A = matches but is the **shortest** prefix — chosen only if nothing else matched ·
B = matches, but /16 and /24 are more specific · C = matches, but the /24 is more specific still.

---

**Q116.** A router uses longest prefix match because `[Core]`

- **A)** shorter prefixes are more likely to be stale
- **B)** it guarantees the packet takes the path with the fewest hops
- **C)** entries are stored in ascending prefix order, so the last match found is used
- **D)** the most specific matching route reflects the most precise knowledge of where the destination is

**Answer: D) the most specific matching route reflects the most precise knowledge of where the destination is**

**Trace / Why:** a /24 entry says "I know where this particular 256-address block is"; a /8 says only "I
know roughly where this 16-million-address region is". The narrower claim is the better-informed one, so
honouring it delivers the packet more accurately.

**📘 CONCEPT — C63 (see Concept Index)** › Specificity is a proxy for **precision of knowledge**, which is
why the rule is about prefix length and not about metrics, age or table order. This is also what makes
aggregation safe: a router can advertise a broad summary while a downstream router still overrides part of
it with a more specific route, and the two coexist without conflict. Option C describes an implementation
detail that is not even true — real tables use tries or TCAMs and match all entries in parallel.

**Wrong traces:** A = prefix length says nothing about staleness · B = hop count is a **metric**, consulted
only after prefix length · C = table ordering is an implementation matter, not the reason for the rule.

---

**Q117.** Can **10.1.0.0/16, 10.2.0.0/16** and **10.3.0.0/16** be summarized into exactly one route that
covers these three and nothing else? `[Trap]`

- **A)** Yes — 10.1.0.0/14
- **B)** Yes — 10.0.0.0/16
- **C)** No — three blocks are not a power of two, so no single prefix covers exactly these three
- **D)** No — the three blocks are not contiguous

**Answer: C) No — three blocks are not a power of two, so no single prefix covers exactly these three**

**Trace / Why:** a CIDR block always contains a **power-of-two** number of sub-blocks. Three is not a power
of two, so no prefix length can enclose exactly three /16s.

```
/15 covers 2 blocks  → too few
/14 covers 4 blocks  → too many (and must start at a multiple of 4)
```

The nearest single route is **10.0.0.0/14**, covering 10.0, 10.1, 10.2, 10.3 — four networks, one of which
(10.0.0.0/16) was never in the original set. The practical answer is two routes: **10.2.0.0/15** plus
**10.1.0.0/16**.

**📘 CONCEPT — C64 · A CIDR block always spans a power-of-two count, starting on an aligned boundary**
> Summarizing an arbitrary set of networks into one exact route is usually **impossible**. The three
> conditions from C60 must all hold, and "count is a power of two" is the one that fails most often. When
> it fails you have two choices:
> - **over-summarize** — advertise a larger block that includes addresses you do not own (acceptable only
>   if nobody else owns them, Q118);
> - **advertise several summaries** — here 10.2.0.0/15 + 10.1.0.0/16, two entries instead of three.
>
> **Applies when** the stem gives a count that is not 2, 4, 8, 16 … or a group starting off-boundary.
>
> **Boundary:** the blocks here **are** contiguous (10.1, 10.2, 10.3 adjoin), so option D's reason is
> false — it is the *count* and the *alignment* that defeat summarization, not adjacency. Distinguishing
> the three failure conditions is what this question and Q114 test between them.

**Wrong traces:** A = 10.1.0.0/14 is not even a valid /14 boundary (1 ÷ 4 is not whole), and a /14 spans
four blocks · B = a /16 is one network, covering only 10.0.0.0 · D = the blocks are contiguous.

---

**Q118.** For the three networks 10.1.0.0/16, 10.2.0.0/16 and 10.3.0.0/16, the smallest **single** route
that covers all three is `[Applied]`

- **A)** 10.0.0.0/14, which also covers 10.0.0.0/16 — a network not in the original set
- **B)** 10.1.0.0/14, which covers exactly the three networks
- **C)** 10.0.0.0/8, which covers the whole Class A network
- **D)** 10.2.0.0/15, which covers 10.2.0.0 and 10.3.0.0 only

**Answer: A) 10.0.0.0/14, which also covers 10.0.0.0/16 — a network not in the original set**

**Trace / Why:** a /14 covers four /16s and must start at a multiple of 4 in the second octet.

```
/14 boundaries in 2nd octet: 0, 4, 8, 12 …
the group spans second octets 1, 2, 3
the only /14 containing all three is the one starting at 0
10.0.0.0/14 → covers 10.0, 10.1, 10.2, 10.3
```

So the summary works but is **over-inclusive** by one network. That is acceptable if the organisation also
owns 10.0.0.0/16 or nobody else advertises it; otherwise it would black-hole traffic for someone else's
network.

**📘 CONCEPT — C65 · Over-summarization: the smallest covering block, and the risk it carries**
> When an exact summary is impossible (C64), the smallest covering block is found by shortening the prefix
> until an aligned block encloses the whole range. The danger is **advertising reachability for addresses
> you do not own** — traffic for them is attracted to you and then dropped.
>
> **Applies when** the stem asks for the "smallest single route that covers" a set, rather than an exact
> summary.
>
> **Boundary:** "covers all three" and "covers exactly the three" are different questions with different
> answers — this one and Q117 respectively. Read which is being asked: the first always has an answer
> (worst case 0.0.0.0/0), the second usually does not.

**Wrong traces:** B = 10.1.0.0 is not a /14 boundary, and no /14 covers exactly three /16s · C = a valid
covering route but far from the **smallest** · D = covers only two of the three, omitting 10.1.0.0.

---

**Q119.** Four contiguous Class C networks are combined into one supernet. The resulting subnet mask is
`[Applied]`

- **A)** 255.255.248.0
- **B)** 255.255.252.0
- **C)** 255.255.254.0
- **D)** 255.255.255.0

**Answer: B) 255.255.252.0**

**Trace / Why:** each Class C is a /24; combining four needs 2 bits of aggregation.

```
24 − log2(4) = 24 − 2 = /22
/22 → 255.255.252.0
```

Check: /22 holds 1,024 addresses = 4 × 256 ✔ (C52).

**📘 CONCEPT — C60 (see Concept Index)** › The supernet masks for grouped Class C networks are worth
knowing directly, since ISP allocation questions use them constantly:
**2 networks → /23 → 255.255.254.0** · **4 → /22 → 255.255.252.0** · **8 → /21 → 255.255.248.0** ·
**16 → /20 → 255.255.240.0** · **32 → /19 → 255.255.224.0**.
Note that the masks *shorten* as the group grows, which is the reverse of the subnetting table and the
commonest direction error here.

**Wrong traces:** A = /21, which combines **eight** Class C networks · C = /23, which combines only two ·
D = /24, a single unaggregated Class C.

---

**Q120.** Which condition is **NOT** required for a group of networks to be summarized into one exact
route? `[Trap]`

- **A)** the networks must be contiguous
- **B)** the number of networks must be a power of two
- **C)** the networks must all belong to the same address class
- **D)** the first network address must be aligned to the summary block boundary

**Answer: C) the networks must all belong to the same address class**

**Trace / Why:** CIDR is **classless** — class boundaries are irrelevant to aggregation. A summary route
may freely span what classful addressing would have called different networks or even different classes,
provided the three real conditions hold: contiguous, power-of-two count, aligned start.

**📘 CONCEPT — C66 · The three summarization conditions, and the one that is not a condition**
> **Required:**
> 1. **Contiguous** — no gaps in the range.
> 2. **Power-of-two count** — 2, 4, 8, 16 … (C64).
> 3. **Aligned** — the first address divisible by the summary block size (C62).
>
> **Not required:** same class · same subnet mask originally · same routing protocol · same physical
> location.
>
> **Applies when** the stem asks what summarization requires, or offers a plausible extra condition.
>
> **Boundary:** the three conditions are **jointly** necessary, so a question can defeat a group through
> any one of them — Q114 fails on alignment, Q117 fails on count. Class membership never appears among
> them, because CIDR abolished the obligation to respect class boundaries (C27); leftover classful
> intuition is exactly what this question probes.

**Wrong traces:** A, B and D = the three genuine conditions, each of which a real group can fail.

---
## Section 8 — Special Cases, IPv6 Subnetting & Practical Diagnostics

**Q121.** A **/32** prefix is normally used for `[Core]`

- **A)** a single host route, or the address of a router's loopback interface
- **B)** a point-to-point link between two routers
- **C)** the default route in a routing table
- **D)** a multicast group address

**Answer: A) a single host route, or the address of a router's loopback interface**

**Trace / Why:** /32 leaves **zero** host bits, so the block contains exactly one address. That is precisely
what you want for a **host route** (a routing entry for one specific machine) and for a router's **loopback
interface** — a virtual interface that is always up and is used as the router's stable identity for OSPF,
BGP and management.

**📘 CONCEPT — C67 · /32 has no network or broadcast address, because it has no host field at all**
> One address, one host. The −2 rule (C9) does not apply and is not merely reduced to zero — there is no
> host field for all-zeros and all-ones patterns to exist in.
>
> | Prefix | Addresses | Usable | Typical use |
> |---|---|---|---|
> | /30 | 4 | 2 | point-to-point, any medium |
> | /31 | 2 | 2 | point-to-point only, RFC 3021 |
> | **/32** | **1** | **1** | host route, loopback interface |
>
> **Applies when** the stem mentions a loopback interface, a host route, or 255.255.255.255 used as a mask.
>
> **Boundary:** because /32 is maximally specific, a /32 entry **always wins longest prefix match** (C63) —
> which is how a single host can be routed differently from the rest of its subnet. Note the contrast with
> Q18: 255.255.255.255 as a *destination address* is the limited broadcast, but as a *mask* it is /32.
> Same bits, opposite meanings, decided by which field they occupy.

**Wrong traces:** B = a point-to-point link needs two addresses, so /30 or /31 · C = the default route is
0.0.0.0/**0**, the shortest prefix, not the longest · D = multicast is Class D, 224–239 (C6).

---

**Q122.** Compared with a **/30**, using a **/31** on a point-to-point link saves how many addresses per
link? `[Applied]`

- **A)** 1
- **B)** 2
- **C)** 3
- **D)** 4

**Answer: B) 2**

**Trace / Why:**

```
/30 → 4 addresses consumed, 2 usable  → 2 wasted (network + broadcast)
/31 → 2 addresses consumed, 2 usable  → 0 wasted
saving = 4 − 2 = 2 addresses per link
```

On a backbone with 500 point-to-point links that is 1,000 addresses recovered — the whole reason RFC 3021
exists.

**📘 CONCEPT — C68 · /31 achieves 100% utilisation by removing the broadcast address, which the medium does not need**
> On a link with exactly two endpoints, "broadcast" and "send to the other end" are the same operation, so
> a dedicated broadcast address is redundant. RFC 3021 therefore assigns **both** addresses of a /31 to
> interfaces.
>
> | | /30 | /31 |
> |---|---|---|
> | Addresses | 4 | 2 |
> | Usable | 2 | 2 |
> | Utilisation | 50% | **100%** |
> | Works on a shared LAN | yes | **no** |
>
> **Applies when** the stem compares point-to-point masks, or asks for the most address-efficient option.
>
> **Boundary:** the saving is available **only** on point-to-point media. On an Ethernet segment ARP
> requires broadcast, so /31 is unusable there and /30 remains the minimum — which is why /30 has not
> disappeared. Note that both prefixes give 2 usable addresses, so a question asking only "how many hosts"
> cannot distinguish them; it must ask about waste or utilisation.

**Wrong traces:** A = counts only the network address, forgetting the broadcast · C = 4 − 1 · D = the total
/30 block size, treating all four as saved.

---

**Q123.** In modern practice, the **all-ones subnet** (the subnet in which every borrowed subnet bit is 1)
is `[Core]`

- **A)** reserved and must never be assigned
- **B)** usable only for point-to-point links
- **C)** usable exactly like any other subnet
- **D)** automatically converted into a broadcast address by the router

**Answer: C) usable exactly like any other subnet**

**Trace / Why:** under classful routing the all-ones subnet's broadcast address collided with the *parent*
network's broadcast, so it was excluded. Classless routing carries the mask explicitly (C58), removing the
ambiguity, and RFC 1878 confirmed both the all-ones subnet and subnet zero as usable. For a Class C
subnetted to /27, the .224/27 subnet is entirely ordinary.

**📘 CONCEPT — C69 · Subnet zero and the all-ones subnet: both usable, and this is why the formula changed**
> The historical exclusions were the reason for the old **2^s − 2** subnet formula; their removal is why
> the modern formula is **2^s** (C34).
>
> | Subnet | Historical status | Modern status |
> |---|---|---|
> | **Subnet zero** (all subnet bits 0) | excluded — its address matched the parent network's | **usable** |
> | **All-ones subnet** (all subnet bits 1) | excluded — its broadcast matched the parent's | **usable** |
>
> **Applies when** the stem asks about subnet zero, the all-ones subnet, `ip subnet-zero`, or offers both
> subnet formulas.
>
> **Boundary:** what has **not** changed is that within *every* subnet — including these two — the first
> and last addresses remain the network and broadcast addresses and stay unusable (C9). One −2 was
> abolished; the other was never in question. Keeping the two straight is the whole point of this topic.

**Wrong traces:** A = the obsolete classful rule · B = no such restriction; that is /31 (Q122) ·
D = routers perform no such conversion.

---

**Q124.** In IPv6, subnetting a typical site allocation is performed by `[Core]`

- **A)** adjusting the 16-bit subnet field that lies between the /48 site prefix and the /64 interface boundary
- **B)** borrowing bits from the 64-bit interface identifier
- **C)** shortening the global routing prefix below /32
- **D)** applying a variable-length mask to the last 16 bits of the address

**Answer: A) adjusting the 16-bit subnet field that lies between the /48 site prefix and the /64 interface boundary**

**Trace / Why:** the standard IPv6 layout gives a site a **/48** and fixes the interface boundary at
**/64**, leaving a dedicated **16-bit subnet id** in between. A site therefore has 2¹⁶ = 65,536 subnets to
allocate without ever touching the host portion.

```
|  48 bits: global routing prefix  | 16 bits: subnet id | 64 bits: interface identifier |
```

**📘 CONCEPT — C70 · IPv6 subnetting is easier than IPv4 because the host field is never squeezed**
> - **/64 is the standard subnet size**, effectively always — required by SLAAC, which builds the
>   interface identifier from 64 bits.
> - A **/48** site holds **65,536** /64 subnets (Q125), so the address-conservation pressure that drives
>   IPv4 VLSM simply does not arise.
> - There is no −2 rule: a /64 has 2⁶⁴ addresses and **no broadcast address to reserve** (Q127).
>
> **Applies when** the stem mentions IPv6 prefixes, /48, /64, SLAAC, or asks how IPv6 subnetting differs.
>
> **Boundary:** going **longer than /64** breaks SLAAC and is done only on point-to-point links (/127, the
> IPv6 analogue of /31). So IPv6 subnetting means choosing how to use the 16 subnet bits, not how to
> divide the host field — the opposite of the IPv4 problem, where the host field is exactly what gets
> divided (C24).

**Wrong traces:** B = the 64-bit interface identifier is left intact; borrowing from it breaks SLAAC ·
C = /32 is the ISP-level allocation, not something a site adjusts · D = the last 16 bits are inside the
interface identifier, not a subnet field.

---

**Q125.** How many **/64** subnets does a single IPv6 **/48** allocation contain? `[Applied]`

- **A)** 256
- **B)** 4,096
- **C)** 16,384
- **D)** 65,536

**Answer: D) 65,536**

**Trace / Why:** the same nesting formula as IPv4 (C50), just with bigger numbers.

```
2^(64 − 48) = 2^16 = 65,536
```

So one /48 site can support 65,536 separate /64 LANs — more than any single organisation needs, which is
why IPv6 addressing plans are built around readability rather than conservation.

**📘 CONCEPT — C50 (see Concept Index)** › The nesting rule **2^(longer − shorter)** is version-independent.
The IPv6 allocations worth knowing: **/32** typical ISP · **/48** typical site · **/56** small site or
home · **/64** one subnet · **/128** one address. A /48 holds 65,536 /64s; a /56 holds 256; a /32 holds
65,536 /48s.

**Wrong traces:** A = 2⁸, the count of /64s in a /56 · B = 2¹² · C = 2¹⁴ — each a smaller power of two from
a shorter gap.

---

**Q126.** The IPv6 address **2001:0db8:0000:0000:0000:ff00:0042:8329** in fully compressed form is
`[Applied]`

- **A)** 2001:db8:0:0:0:ff00:42:8329
- **B)** 2001:db8::0:ff00:42:8329
- **C)** 2001:0db8::ff00:0042:8329
- **D)** 2001:db8::ff00:42:8329

**Answer: D) 2001:db8::ff00:42:8329**

**Trace / Why:** apply both compression rules together.

```
1. Drop leading zeros in each group:
   2001:0db8:0000:0000:0000:ff00:0042:8329
   → 2001:db8:0:0:0:ff00:42:8329

2. Replace the longest run of all-zero groups with "::" (once only):
   → 2001:db8::ff00:42:8329
```

**📘 CONCEPT — C71 · The two IPv6 compression rules, and the one-`::` restriction**
> 1. **Leading zeros** within a group may be omitted — `0db8` → `db8`, `0042` → `42`. Trailing zeros may
>    **not**: `8329` stays as it is.
> 2. **One or more consecutive all-zero groups** may be replaced by `::`, and this may be done **only
>    once** per address — otherwise the number of omitted groups would be ambiguous.
>
> **Applies when** the stem asks to compress or expand an IPv6 address.
>
> **Boundary:** the single-`::` rule is what makes `2001:db8::1:0:0:1` legal but
> `2001:db8::1::1` illegal. When there are two equal-length zero runs, convention compresses the
> **leftmost**. Note that `::` may also cover a single zero group, though writing `:0:` is equally valid —
> which is why "fully compressed" is the phrasing that forces one unique answer.

**Wrong traces:** A = rule 1 applied, rule 2 not · B = a redundant `0` left beside the `::` — not fully
compressed · C = rule 2 applied, rule 1 not (`0db8` and `0042` still padded).

---

**Q127.** Which statement about IPv6 is correct? `[Trap]`

- **A)** IPv6 uses a 32-bit subnet mask in dotted-decimal form
- **B)** IPv6 subnets reserve the first and last address exactly as IPv4 does
- **C)** IPv6 has no broadcast address; its role is taken by multicast
- **D)** IPv6 requires NAT because its address space is still limited

**Answer: C) IPv6 has no broadcast address; its role is taken by multicast**

**Trace / Why:** IPv6 deliberately removed broadcast. Where IPv4 would broadcast, IPv6 sends to a
well-defined multicast group — `ff02::1` for all-nodes, `ff02::2` for all-routers — so only interested
hosts process the packet. ARP is likewise replaced by **Neighbor Discovery** over multicast.

**📘 CONCEPT — C72 · The IPv4 habits that do not carry over to IPv6**
> | IPv4 | IPv6 |
> |---|---|
> | Broadcast address per subnet | **none** — multicast instead |
> | Network address reserved | **not reserved** — the all-zeros host address is the subnet-router anycast address, but ordinary hosts may use others |
> | −2 usable-host rule | **does not apply** |
> | Dotted-decimal mask | prefix length only, e.g. `/64` |
> | ARP | Neighbor Discovery |
> | NAT widely required | not required; addresses are abundant |
>
> **Applies when** the stem contrasts IPv4 and IPv6 addressing, or applies an IPv4 rule to IPv6.
>
> **Boundary:** the absence of broadcast is why an IPv6 /64 does not lose 2 addresses, and therefore why
> **the entire host-count arithmetic of this chapter is IPv4-specific**. Carrying the −2 rule into IPv6 is
> the standard error, and it is what option B tests.

**Wrong traces:** A = IPv6 uses prefix length notation only; there is no dotted-decimal mask ·
B = the −2 rule is IPv4-specific — the intended trap · D = IPv6's 128-bit space removes the need for NAT.

---

**Q128.** The IPv6 **link-local** address block is `[Applied]`

- **A)** ::1/128
- **B)** fc00::/7
- **C)** fe80::/10
- **D)** ff00::/8

**Answer: C) fe80::/10**

**Trace / Why:** every IPv6 interface automatically configures a link-local address from **fe80::/10**.
It is valid only on its own link — routers never forward it — and it is what Neighbor Discovery and
routing protocols use to talk to directly attached neighbours.

**📘 CONCEPT — C73 · The IPv6 prefixes examiners ask for**
> | Prefix | Type | IPv4 analogue |
> |---|---|---|
> | **::1/128** | loopback | 127.0.0.1 |
> | **::/128** | unspecified | 0.0.0.0 |
> | **fc00::/7** | unique local (private) | RFC 1918 ranges |
> | **fe80::/10** | link-local | 169.254.0.0/16 (APIPA) |
> | **ff00::/8** | multicast | 224.0.0.0/4 |
> | **2000::/3** | global unicast | public addresses |
>
> **Applies when** the stem gives an IPv6 prefix and asks its purpose, or asks for the IPv6 equivalent of an
> IPv4 special range.
>
> **Boundary:** link-local in IPv6 is **mandatory and always present**, unlike APIPA in IPv4 which appears
> only when DHCP fails (C14). So an fe80:: address is normal and healthy, whereas a 169.254 address is a
> fault symptom — the analogy holds for the address range but reverses for the diagnosis.

**Wrong traces:** A = loopback · B = unique local, the private-address analogue · D = multicast.

---

**Q129.** Which device forms the boundary of a broadcast domain? `[Applied]`

- **A)** a repeater
- **B)** an unmanaged hub
- **C)** a layer-2 switch
- **D)** a router

**Answer: D) a router**

**Trace / Why:** hubs and repeaters forward broadcasts to every port. A layer-2 switch forwards broadcasts
to all ports in the same VLAN — it separates **collision** domains but not broadcast domains. A **router**
does not forward broadcasts at all, so each of its interfaces bounds a separate broadcast domain, which is
exactly the containment subnetting is designed to achieve (C17).

**📘 CONCEPT — C74 · Collision domains and broadcast domains are bounded by different devices**
> | Device | Separates collision domains | Separates broadcast domains |
> |---|---|---|
> | Hub / repeater | **no** | no |
> | Switch / bridge | **yes** (per port) | no (per VLAN) |
> | Router | yes | **yes** |
>
> **Applies when** the stem asks how many broadcast or collision domains a topology has, or which device
> limits broadcasts.
>
> **Boundary:** a switch **can** separate broadcast domains once **VLANs** are configured — each VLAN is
> its own broadcast domain, and inter-VLAN traffic then needs a router or a layer-3 switch. So the strict
> answer is "a router, or a switch configured with VLANs", and a question offering an *unmanaged* or plain
> layer-2 switch is excluding that case deliberately. Counting rule: **broadcast domains = number of router
> interfaces (or VLANs)**; **collision domains = number of switch ports**.

**Wrong traces:** A and B = forward broadcasts everywhere, separating neither kind of domain · C = separates
collision domains only, unless VLANs are configured.

---

**Q130.** Network Address Translation (NAT) is required when `[Core]`

- **A)** hosts using RFC 1918 private addresses need to communicate across the public Internet
- **B)** two hosts on the same subnet need to exchange packets
- **C)** a router must forward a broadcast between two subnets
- **D)** an IPv6 host needs to reach another IPv6 host

**Answer: A) hosts using RFC 1918 private addresses need to communicate across the public Internet**

**Trace / Why:** private addresses (C12) are not routable on the Internet — no ISP will carry them. NAT
rewrites the private source address in outgoing packets to a public one, tracks the translation, and
reverses it for the replies. **PAT** (NAT overload) extends this by multiplexing many private hosts behind
one public address using distinct port numbers, which is how a whole household shares one IPv4 address.

**📘 CONCEPT — C75 · NAT is the third response to IPv4 exhaustion, alongside CIDR and private addressing**
> The three work together: **private addressing** (RFC 1918) lets an organisation use any size of internal
> space; **CIDR** lets public allocations be sized precisely; **NAT** bridges the two. Without NAT, private
> addressing would leave hosts unable to reach the Internet at all.
>
> **Applies when** the stem mentions private addresses reaching the Internet, address sharing, or PAT.
>
> **Boundary:** NAT breaks **end-to-end addressing** — inbound connections to a private host require
> explicit port forwarding, and protocols that embed addresses in their payload need application-layer
> gateways. IPv6's abundant addresses remove the motivation entirely, which is why option D is wrong and
> why NAT is regarded as a workaround rather than a feature.

**Wrong traces:** B = same-subnet traffic is delivered directly, no translation involved · C = routers do
not forward broadcasts (C74), and NAT is unrelated to broadcast · D = IPv6 has no address shortage and
needs no NAT.

---
## Answer Key — All 130 Questions

**Part 1 (Q1–Q84)**

| Q | A | Q | A | Q | A | Q | A | Q | A | Q | A |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | D | 15 | D | 29 | A | 43 | C | 57 | B | 71 | A |
| 2 | A | 16 | C | 30 | A | 44 | B | 58 | A | 72 | A |
| 3 | C | 17 | A | 31 | A | 45 | D | 59 | B | 73 | C |
| 4 | C | 18 | D | 32 | C | 46 | C | 60 | B | 74 | A |
| 5 | B | 19 | C | 33 | D | 47 | D | 61 | D | 75 | B |
| 6 | C | 20 | B | 34 | A | 48 | D | 62 | A | 76 | C |
| 7 | C | 21 | B | 35 | D | 49 | A | 63 | B | 77 | C |
| 8 | A | 22 | C | 36 | D | 50 | A | 64 | A | 78 | B |
| 9 | B | 23 | B | 37 | A | 51 | B | 65 | C | 79 | C |
| 10 | B | 24 | B | 38 | A | 52 | D | 66 | D | 80 | D |
| 11 | B | 25 | D | 39 | D | 53 | B | 67 | B | 81 | A |
| 12 | D | 26 | C | 40 | C | 54 | B | 68 | A | 82 | C |
| 13 | D | 27 | C | 41 | B | 55 | B | 69 | D | 83 | A |
| 14 | C | 28 | A | 42 | D | 56 | C | 70 | A | 84 | B |

**Part 2 (Q85–Q130)**

| Q | A | Q | A | Q | A | Q | A |
|---|---|---|---|---|---|---|---|
| 85 | D | 97 | B | 109 | B | 121 | A |
| 86 | D | 98 | C | 110 | B | 122 | B |
| 87 | A | 99 | A | 111 | C | 123 | C |
| 88 | A | 100 | D | 112 | B | 124 | A |
| 89 | D | 101 | B | 113 | A | 125 | D |
| 90 | B | 102 | D | 114 | C | 126 | D |
| 91 | D | 103 | A | 115 | D | 127 | C |
| 92 | B | 104 | B | 116 | D | 128 | C |
| 93 | B | 105 | C | 117 | C | 129 | D |
| 94 | A | 106 | C | 118 | A | 130 | A |
| 95 | A | 107 | D | 119 | B | | |
| 96 | C | 108 | D | 120 | C | | |

**Answer distribution (counted, not estimated):** A = 33 · B = 33 · C = 32 · D = 32 — as even as 130
divides. The letters were generated as a shuffled balanced sequence **before** any option was written and
re-verified against that sequence afterwards; three questions that drifted during drafting were fixed by
**reordering their options**, never by changing an answer.

**Difficulty mix (counted):** Core 26 · Applied 81 · Trap 23. The skill's nominal target is 50/35/15, and
this chapter departs from it sharply in the Applied direction — Sections 3 and 4 alone are 44 consecutive
calculation questions, and subnetting simply has more to *compute* than to *recall*. Reported as counted;
relabelling calculation questions as "Core" to hit a quota would make the tags useless for the learner
deciding what to practise.

**Worked calculations:** 79 of the 130 questions show the arithmetic step by step in the explanation.
Every figure was computed and cross-checked with Python's `ipaddress` module before being written —
including the full VLSM allocation of Section 6 and the two summarization cases that turn out to be
**impossible** (Q114, Q117), which were verified by confirming that `collapse_addresses` refuses to
merge them.

---

## Coverage Report

Every inventory item with the questions that test it. No row is empty — that is the completeness
guarantee, and it is why this chapter needed no second call.

| # | Concept | Questions |
|---|---|---|
| 1 | IPv4 address length is 32 bits | Q1 |
| 2 | Octet range 0–255; the 128–1 place values | Q2 |
| 3 | Total IPv4 space = 2³² | Q3 |
| 4 | Class A first-octet range 1–126 | Q4 |
| 5 | Classifying an address from its first octet | Q5 |
| 6 | Class B range and default mask | Q5, Q10 |
| 7 | Class C default mask 255.255.255.0 | Q6 |
| 8 | Class D multicast range 224–239 | Q7 |
| 9 | Unicast / broadcast / multicast / anycast | Q7 |
| 10 | Class E reserved range 240–255 | Q8 |
| 11 | Loopback 127.0.0.0/8 and what it tests | Q9 |
| 12 | Why 127 is excluded from Class A | Q9 |
| 13 | Mask-to-prefix conversion by counting one-bits | Q10, Q40 |
| 14 | Class C usable hosts = 254 | Q11 |
| 15 | The −2 rule (network + broadcast) | Q11 |
| 16 | Class B usable hosts = 65,534 | Q12 |
| 17 | Class A networks = 2⁷ − 2 = 126 | Q13 |
| 18 | Networks per class; only Class A subtracts 2 | Q13 |
| 19 | Network portion vs host portion under a mask | Q14 |
| 20 | RFC 1918 private ranges, all three | Q15 |
| 21 | 172.16.0.0/12 ends at 172.31.255.255 | Q16 |
| 22 | Block size = 256 − interesting mask octet | Q16, Q46, Q47, Q59 |
| 23 | APIPA 169.254.0.0/16 signals DHCP failure | Q17 |
| 24 | Limited vs directed broadcast | Q18 |
| 25 | 0.0.0.0 meanings; 0.0.0.0/0 default route | Q19 |
| 26 | Why subnet: smaller broadcast domains | Q20 |
| 27 | Subnetting costs usable addresses | Q20, Q51, Q60 |
| 28 | Same-subnet test = IP AND mask | Q21, Q80, Q81, Q82 |
| 29 | Class D has no network/host split | Q22 |
| 30 | Purpose of the subnet mask | Q23 |
| 31 | The mask is local, not carried in the IP header | Q23 |
| 32 | Prefix-to-mask: /26 → 255.255.255.192 | Q24 |
| 33 | The eight legal partial-octet values | Q24, Q29 |
| 34 | Mask-to-prefix: 255.255.255.248 → /29 | Q25 |
| 35 | Which octet a partial mask value belongs in | Q26, Q38 |
| 36 | Host bits = 32 − prefix | Q27, Q34 |
| 37 | Invalid (non-contiguous) subnet masks | Q28 |
| 38 | Wildcard mask = 255 − each octet | Q30 |
| 39 | Borrowed bits: host → network | Q31 |
| 40 | Binary form of a mask octet | Q32 |
| 41 | Longer prefix ⇒ fewer hosts, more subnets | Q33 |
| 42 | /25 splits a /24 into two halves | Q35 |
| 43 | Classful addressing vs CIDR | Q36 |
| 44 | CIDR full form and purpose | Q37 |
| 45 | /31 is a valid mask (RFC 3021) | Q39 |
| 46 | /27 usable hosts = 30; the /24–/30 host table | Q41 |
| 47 | /22 usable hosts = 1,022; prefixes above /24 | Q42 |
| 48 | Subnets = 2^(borrowed bits) | Q43, Q44 |
| 49 | Class C → /28 gives 16 subnets | Q43 |
| 50 | Class C → /27 gives 8 subnets | Q44 |
| 51 | Class B → /22 gives 64 subnets | Q45 |
| 52 | Conservation check: subnets × block = parent | Q45 |
| 53 | /30 usable hosts = 2 | Q48 |
| 54 | /29 usable hosts = 6 | Q49 |
| 55 | Modern subnet formula is 2^s, not 2^s − 2 | Q50 |
| 56 | Total usable hosts after a uniform split | Q51 |
| 57 | Borrowed bits from a class default | Q52 |
| 58 | Class A → /16 gives 256 subnets | Q53 |
| 59 | Same mask, different class, different subnet count | Q54 |
| 60 | Class A usable hosts = 16,777,214 | Q55 |
| 61 | Subnets begin at multiples of the block size | Q56 |
| 62 | /25 usable hosts = 126 | Q57 |
| 63 | /28 usable hosts = 14 | Q58 |
| 64 | Addresses lost to per-subnet overhead | Q60 |
| 65 | Network address by stepping down to a multiple | Q61, Q65, Q67, Q68, Q70, Q71, Q73, Q74, Q76 |
| 66 | Broadcast = next network address − 1 | Q62, Q64, Q66, Q75, Q77 |
| 67 | Valid host range = network+1 to broadcast−1 | Q63, Q69, Q72 |
| 68 | Boundary in the third octet | Q64, Q65, Q75 |
| 69 | Boundary in the second octet; trailing octets fill | Q66 |
| 70 | Divisibility test for a valid network address | Q65, Q71, Q76 |
| 71 | Divide-and-floor to find the block | Q74, Q76 |
| 72 | /23 merges two third-octet values | Q68, Q69 |
| 73 | /22 merges four third-octet values | Q73 |
| 74 | /30 blocks: the middle two addresses are usable | Q71, Q72 |
| 75 | Which subnet does a given host belong to | Q78 |
| 76 | .0 and .255 are not always reserved | Q79 |
| 77 | Whether a specific address is assignable | Q79, Q84 |
| 78 | Last usable vs broadcast vs next network | Q83 |
| 79 | Validating a candidate host address (two checks) | Q84 |
| 80 | Host requirement → longest usable prefix | Q85, Q86, Q95 |
| 81 | Subnet requirement → minimum borrowed bits | Q87 |
| 82 | Two requirements bracket the prefix | Q88 |
| 83 | Infeasible designs; the capacity check | Q89 |
| 84 | Nesting count = 2^(prefix difference) | Q90, Q91, Q92, Q97 |
| 85 | Direction check: longer fits inside shorter | Q93 |
| 86 | VLSM design procedure, largest first | Q94 |
| 87 | Class B → /23 gives 128 subnets | Q96 |
| 88 | Short prefixes counted in whole /24s | Q97 |
| 89 | Requirements are floors, not targets | Q98 |
| 90 | VLSM full form and definition | Q99 |
| 91 | VLSM vs FLSM waste, quantified | Q100, Q110 |
| 92 | Different prefix lengths within one network | Q101 |
| 93 | VLSM allocation worked, block by block | Q102, Q103, Q104 |
| 94 | Alignment: blocks start at multiples of their size | Q102, Q104, Q108 |
| 95 | The allocation pointer advances past the broadcast | Q105 |
| 96 | VLSM requires the mask in routing updates | Q106 |
| 97 | Classful protocols: RIPv1 and IGRP | Q107 |
| 98 | Unallocated remainder after a VLSM design | Q109 |
| 99 | Supernetting definition and direction | Q111 |
| 100 | Summary prefix = original − log₂(count) | Q112, Q119 |
| 101 | Summarization shrinks routing tables | Q113 |
| 102 | Alignment can defeat summarization | Q114 |
| 103 | Longest prefix match | Q115, Q116 |
| 104 | Power-of-two count condition | Q117 |
| 105 | Over-summarization and its risk | Q118 |
| 106 | Supernet masks for grouped Class C networks | Q119 |
| 107 | The three conditions — class is not one of them | Q120 |
| 108 | /32 as a host route or loopback interface | Q121 |
| 109 | /31 saves 2 addresses per point-to-point link | Q122 |
| 110 | The all-ones subnet is usable | Q123 |
| 111 | IPv6 subnetting: the 16-bit field between /48 and /64 | Q124 |
| 112 | A /48 contains 65,536 /64 subnets | Q125 |
| 113 | IPv6 zero-compression rules | Q126 |
| 114 | IPv6 has no broadcast; the −2 rule does not apply | Q127 |
| 115 | IPv6 special prefixes (fe80::/10 and the rest) | Q128 |
| 116 | Broadcast domains vs collision domains | Q129 |
| 117 | NAT and PAT for private-to-public translation | Q130 |

**117 inventory items, 117 covered, 0 empty.**

---

## High-Yield Revision Sheet

The night-before page. Twenty lines that carry the most marks in this chapter.

1. **Class ranges:** A = 1–126 (/8) · B = 128–191 (/16) · C = 192–223 (/24) · D = 224–239 multicast ·
   E = 240–255 reserved. **127 is loopback**, which is why A stops at 126.
2. **Usable hosts = 2^h − 2.** Forgetting the −2 is the most common error in the entire topic.
   Class A = 16,777,214 · B = 65,534 · C = 254.
3. **Subnets = 2^s** (modern). The old 2^s − 2 excluded subnet zero and the all-ones subnet; both are
   usable now. **The host −2 never changed.**
4. **Block size = 256 − interesting mask octet.** It is simultaneously the increment between subnets and
   the number of addresses in each.
5. **The eight legal mask octets:** 128, 192, 224, 240, 248, 252, 254, 255. Nothing else is legal.
6. **Host table:** /24→254 · /25→126 · /26→62 · /27→30 · /28→14 · /29→6 · /30→2 · /31→2 (RFC 3021) ·
   /32→1.
7. **Above /24:** /23→510 · /22→1,022 · /21→2,046 · /20→4,094 · /19→8,190 · /18→16,382.
8. **Network address** = step **down** to the nearest multiple of the block size.
   **Broadcast** = next network − 1. **Usable** = network+1 to broadcast−1.
9. **A valid network address is divisible by the block size.** 40 can never begin a /20; 92 can never
   begin a /21.
10. **Same subnet?** AND both addresses with the mask and compare. Matching leading octets prove nothing.
11. **.0 and .255 are only special relative to a mask.** Under /23, `172.16.2.255` and `172.16.3.0` are
    ordinary usable hosts.
12. **Private ranges:** 10.0.0.0/8 · 172.16.0.0/12 (**ends at 172.31**.255.255) · 192.168.0.0/16.
    **169.254.x.x = APIPA, DHCP failed.** 127.0.0.1 = loopback.
13. **Sizing:** round **up** always. 50 hosts → /26 · 1,000 hosts → /22 · 14 subnets → 4 borrowed bits.
    Requirements are floors, never targets.
14. **Two constraints bracket the prefix:** subnets set the minimum length, hosts set the maximum. An empty
    window means the design is impossible — check `subnets × block size ≤ parent block`.
15. **Nesting:** number of smaller blocks = **2^(longer prefix − shorter prefix)**. /24 into /26 = 4 ·
    /24 into /30 = 64 · /48 into /64 = 65,536.
16. **VLSM: allocate largest first**, and every block must start at a multiple of its own size. It needs a
    **classless** protocol — **RIPv1 and IGRP cannot**; RIPv2, EIGRP, OSPF, IS-IS, BGP can.
17. **Summarization:** summary prefix = original − log₂(count); summary address = the **first** network.
    Requires **contiguous + power-of-two count + aligned start**. Same class is *not* required.
18. **Longest prefix match:** every matching entry is considered and the **most specific** wins. A /32
    always wins; 0.0.0.0/0 always loses to any other match.
19. **/30 = 4 addresses, 2 usable (50% waste). /31 = 2 addresses, both usable** — point-to-point only.
    **/32 = one address**, host route or loopback.
20. **IPv6:** /64 is the standard subnet · /48 site holds 65,536 of them · **no broadcast, so no −2 rule** ·
    fe80::/10 link-local · one `::` per address.

---

## Concept Index

The transferable content, one line per rule. Revise from this, not from the questions.

| id | Concept | Rule in one line | Drilled by |
|---|---|---|---|
| C1 | Address sizes | IPv4 32 bits, IPv6 128, MAC 48 | Q1 |
| C2 | Octet values | 8 bits = 256 values, numbered 0–255 | Q2 |
| C3 | Space arithmetic | Addresses = 2^(free bits); addresses ≠ usable hosts | Q3 |
| C4 | Classful ranges | Read the first octet; 0 and 127 are the holes in Class A | Q4, Q5, Q8 |
| C5 | Default masks | A /8, B /16, C /24; 255.255.255.255 is not a class default | Q6 |
| C6 | Delivery models | Class D has no host field, so it cannot be subnetted | Q7, Q22 |
| C7 | Loopback | 127.0.0.0/8 tests the local stack only, and nothing beyond it | Q9 |
| C8 | Prefix length | Count the one-bits — but check contiguity first | Q10, Q40 |
| C9 | The −2 rule | Usable = 2^h − 2; exceptions are /31 and /32 only | Q11, Q12, Q55 |
| C10 | Networks per class | Fix the leading bits; only Class A subtracts 2 | Q13 |
| C11 | Network/host split | The mask decides the boundary, never the address alone | Q14 |
| C12 | Private ranges | 10/8, 172.16/12 (to 172.31), 192.168/16 — and 169.254 is not one | Q15 |
| C13 | Block size | 256 − interesting octet = increment = addresses per subnet | Q16, Q46, Q47, Q59 |
| C14 | APIPA | 169.254.x.x means DHCP failed; it is a symptom, not a config | Q17 |
| C15 | Broadcast kinds | Limited never leaves the link; directed names a remote subnet | Q18 |
| C16 | 0.0.0.0 | Meaning depends on context; /0 is the default route | Q19 |
| C17 | Why subnet | Buys broadcast containment, costs 2 addresses per subnet | Q20 |
| C18 | The AND test | Same network address ⇒ same subnet ⇒ no router needed | Q21, Q80, Q82 |
| C19 | Mask purpose | Supplies the boundary; local only, not in the IP header | Q23 |
| C20 | Partial octets | 128/192/224/240/248/252/254/255, in the octet the prefix names | Q24, Q25, Q26, Q32, Q38 |
| C21 | Host bits | 32 − prefix; network and host bits always sum to 32 | Q27, Q34 |
| C22 | Contiguity | Legal masks are unbroken left-aligned 1s, then 0s | Q28, Q29 |
| C23 | Wildcard mask | 255 − each octet; 0 means "must match" — the inverse semantics | Q30 |
| C24 | Borrowed bits | Host → network; more subnets always means fewer hosts | Q31 |
| C25 | Direction rule | Longer prefix = smaller block = more subnets | Q33, Q93 |
| C26 | Octet placement | /17–/24 works in the third octet, /25–/30 in the fourth | Q35 |
| C27 | CIDR | Prefix is explicit, so the boundary is no longer tied to class | Q36, Q37 |
| C28 | /31 | Point-to-point links need no broadcast, so both addresses are usable | Q39 |
| C29 | Host table | Memorise /24–/30 and /23–/18; addresses ≠ usable hosts | Q41, Q42, Q49, Q57, Q58 |
| C30 | Subnet count | 2^(new prefix − class default); the class decides the answer | Q43, Q44, Q53, Q54, Q96 |
| C31 | Conservation | subnets × addresses per subnet = parent block, always | Q45 |
| C32 | Third-octet boundary | Each block spans whole fourth octets, ending .255 | Q47 |
| C33 | /30 | Smallest conventional subnet; 4 addresses, 2 usable | Q48 |
| C34 | Subnet zero | Modern formula is 2^s; the historical −2 is obsolete | Q50 |
| C35 | Split overhead | 2 addresses lost per subnet; overhead grows as subnets shrink | Q51, Q60 |
| C36 | Subnet multiples | Subnets start at 0, b, 2b, 3b … | Q56 |
| C37 | Network address | Step **down** to the nearest multiple; verify by divisibility | Q61, Q65, Q70, Q74, Q76 |
| C38 | Broadcast | Next network − 1; the −1 is what separates it from the next subnet | Q62, Q77 |
| C39 | Four boundaries | network / first / last / broadcast — both ends excluded | Q63, Q69, Q83 |
| C40 | Short prefixes | Broadcast fills every octet right of the boundary with 255 | Q66, Q75 |
| C41 | Merged octets | /23 merges 2 third-octet values, /22 four, /21 eight, /20 sixteen | Q68, Q73, Q81 |
| C42 | /30 blocks | Step in fours; the middle two addresses are the usable pair | Q71, Q72 |
| C43 | "Which subnet" | Same question as "what is the network address" | Q78 |
| C44 | Reserved addresses | Decided by the mask, never by how the last octet looks | Q79 |
| C45 | Validating an address | Check the subnet **and** check it is not network or broadcast | Q84 |
| C46 | Sizing for hosts | Smallest h with 2^h − 2 ≥ requirement; round **up** | Q85, Q86, Q95 |
| C47 | Sizing for subnets | Smallest s with 2^s ≥ requirement; no −2 here | Q87 |
| C48 | Two constraints | Subnets set the minimum prefix, hosts the maximum | Q88 |
| C49 | Capacity check | subnets × block size ≤ parent, or the design is impossible | Q89 |
| C50 | Nesting | 2^(longer − shorter); version-independent | Q90, Q91, Q92, Q97, Q125 |
| C51 | VLSM procedure | Sort descending, allocate contiguously, respect alignment | Q94, Q103 |
| C52 | Whole /24s | /23→2, /22→4, /21→8, /20→16, /19→32, /18→64 | Q97 |
| C53 | Floors not targets | Counts are powers of two, so spare capacity is normal | Q98 |
| C54 | VLSM definition | Different prefix lengths within one network; one mask per subnet | Q99, Q101 |
| C55 | VLSM saving | Sum block sizes, not usable counts, to measure consumption | Q100, Q109, Q110 |
| C56 | Alignment | A block must start at a multiple of its own size | Q102, Q104, Q108 |
| C57 | Allocation pointer | Next free = previous broadcast + 1, then round up if needed | Q105 |
| C58 | Classless protocols | The mask must travel with the update; RIPv1 and IGRP cannot | Q106, Q107 |
| C59 | Supernetting | Shortens the prefix — the inverse of subnetting | Q111 |
| C60 | Summarization | Prefix − log₂(count), address = the first network | Q112, Q119 |
| C61 | Table size | Fewer entries, faster lookup — and hidden instability as the cost | Q113 |
| C62 | Alignment defeats it | Contiguity is necessary but not sufficient | Q114 |
| C63 | Longest prefix match | Most specific match wins, before any metric | Q115, Q116 |
| C64 | Power-of-two count | A CIDR block spans 2^n sub-blocks; three can never be exact | Q117 |
| C65 | Over-summarization | Smallest covering block may include what you do not own | Q118 |
| C66 | Three conditions | Contiguous + power-of-two + aligned; same class is not required | Q120 |
| C67 | /32 | One address, no network or broadcast; always wins LPM | Q121 |
| C68 | /31 utilisation | 100% on point-to-point, unusable on a shared LAN | Q122 |
| C69 | All-ones subnet | Usable; only the per-subnet −2 survives from the old rules | Q123 |
| C70 | IPv6 subnetting | Use the 16 bits between /48 and /64; never touch the host field | Q124 |
| C71 | IPv6 compression | Drop leading zeros; one `::` per address | Q126 |
| C72 | IPv4 habits | No broadcast in IPv6, so no −2 rule and no dotted mask | Q127 |
| C73 | IPv6 prefixes | ::1 loopback · fc00::/7 private · fe80::/10 link-local · ff00::/8 multicast | Q128 |
| C74 | Domain boundaries | Switch bounds collision domains; router bounds broadcast domains | Q129 |
| C75 | NAT | Bridges private addressing to the public Internet; breaks end-to-end | Q130 |

---

## Status

**The Subnetting chapter is covered in full: 117 inventory items, 117 covered, across 130 questions in
this single run.** Nothing was held back — IPv4 addressing and classes, special and private addresses,
masks and CIDR conversions, the three core formulas, network/broadcast/host-range calculation, subnet
design, VLSM, supernetting and route summarization, longest prefix match, the /31, /32, subnet-zero and
all-ones-subnet exceptions, IPv6 subnetting, and the broadcast-domain and NAT topics that exam papers
attach to this chapter.

Available as reformatting of the same material:

- `/mcq Subnetting --hard` → the 30 trap-tier and the harder applied questions only, for final revision.
- `/mcq Subnetting --practice` → all 130 questions first, explanations moved to the end, for timed
  self-testing under exam conditions.

*Research note: the section ordering follows the frequency signal from IndiaBIX and ExamVeda subnetting
sets, GATE network-layer PYQ collections, CCNA practice banks, and BCS/bank ICT compilations. Those sources
agree that calculation questions dominate and that the omitted −2 is the most common candidate error, which
is why Sections 3 and 4 are the longest and why the −2 rule is drilled from several angles. Every question
was written fresh and every numerical figure independently computed; nothing is reproduced from any source.*
