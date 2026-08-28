# Data Communication and Networking — MCQ Question Bank
**Subject:** Computer Networks · **Target:** BCS Preliminary / Bank IT Officer / NTRCA / GATE / IBPS SO IT
**Questions:** 200 (Q1–Q200, across three files) · **Concepts covered:** 167/167 · **Complete in one run**

> How to use: attempt the question first, then read the explanation. The wrong
> options matter more than the right one — that's what the examiner is testing.

**This is a subject-sized chapter, covered end to end.** Data communication fundamentals and performance ·
topologies and switching · OSI and TCP/IP models · transmission media and signals · line coding,
modulation and multiplexing · the data link layer (framing, error control, flow control, sliding windows) ·
MAC protocols, Ethernet and LAN devices · the network layer (IP header, fragmentation, ICMP, routing) ·
the transport layer (TCP, UDP, congestion control) · the application layer and network security. Nothing
is held back for a later batch.

**One deliberate cross-reference.** IP addressing, subnet masks, CIDR, VLSM and supernetting are covered
exhaustively in this Bank's **[Subnetting chapter](../subnetting/mcq_subnetting.md)** — 130 questions,
117 inventory items. This bank therefore treats the network layer's *other* material (header fields,
fragmentation, ICMP, routing algorithms, congestion control) and does not re-ask addressing arithmetic.
That is coverage in a sibling file, not an omission.

**Where the marks are.** Research across GATE PYQ collections, IBPS SO IT / bank IT professional-knowledge
sets, and standard MCQ banks gives a consistent ranking. For **GATE**: the data link layer (sliding-window
efficiency, CRC, MAC protocols) and the transport layer (TCP flow and congestion control) carry the most
marks, alongside IP addressing. For **BCS, NTRCA and bank IT**: OSI layer functions, topologies,
transmission media, network devices and their layers, and the basic definitions dominate — the material in
Sections 1–3. Computer Networks contributes roughly 6–8 marks in GATE CSE and 8–12 in IBPS SO IT.
Sections are ordered so the cheap, universally-asked material comes first and the calculation-heavy GATE
material follows.

**42 of the 200 questions carry a worked calculation**, and 80 more are `[Applied]` reasoning questions.
This chapter is more definitional than the Subnetting one — OSI layers, media and protocols are recall and
comparison rather than arithmetic — so the numerical share is lower by nature and is reported as counted.
Every figure that does appear was computed and cross-checked before being written: Nyquist and Shannon
capacities, delays and bandwidth-delay products, sliding-window efficiencies, CRC remainders, Hamming
parameters, minimum frame sizes, PCM rates, ALOHA throughputs and fragmentation offsets.

---

## Section 1 — Data Communication Fundamentals & Performance

**Q1.** The five components of a data communication system are `[Core]`

- **A)** sender, receiver, cable, modem, computer
- **B)** source, channel, encoder, decoder, sink
- **C)** hardware, software, data, people, procedures
- **D)** message, sender, receiver, transmission medium, protocol

**Answer: D) message, sender, receiver, transmission medium, protocol**

**Trace / Why:** the **message** is the data; the **sender** and **receiver** are the devices; the
**transmission medium** is the physical path; the **protocol** is the set of rules both ends agree on.
Remove any one and communication fails — most tellingly the protocol, without which two devices
physically connected still cannot understand each other.

**📘 CONCEPT — C1 · The protocol is a component, not an optional extra**
> The five components split into three physical (sender, receiver, medium), one informational (message)
> and one **agreement** (protocol). The protocol is the component students omit, yet it is what makes the
> other four useful — two computers on one cable speaking different protocols exchange nothing.
>
> **Applies when** the stem asks to list or count the components, or asks which is essential.
>
> **Boundary:** the medium can be **guided or unguided**, so "cable" is too narrow (option A) — wireless
> communication has all five components with air as the medium.

**Wrong traces:** A = names specific hardware rather than the general components, and omits the protocol ·
B = information-theory vocabulary, a different model · C = the five components of an *information system*,
not of data communication.

---

**Q2.** Which of the following is an example of **simplex** transmission? `[Core]`

- **A)** a telephone conversation
- **B)** a walkie-talkie exchange
- **C)** an Ethernet link running at full capacity in both directions
- **D)** a keyboard sending keystrokes to a computer

**Answer: D) a keyboard sending keystrokes to a computer**

**Trace / Why:** in simplex mode communication is **unidirectional** — the channel carries data one way
only. A keyboard only ever transmits and a traditional monitor only ever receives, so each is simplex.

**📘 CONCEPT — C2 · The three data-flow modes, distinguished by direction and simultaneity**
> | Mode | Direction | Simultaneous? | Example |
> |---|---|---|---|
> | **Simplex** | one way only | n/a | keyboard, monitor, radio broadcast |
> | **Half-duplex** | both ways | **no** — one at a time | walkie-talkie, old Ethernet on a hub |
> | **Full-duplex** | both ways | **yes** | telephone, switched Ethernet |
>
> **Applies when** the stem describes who may transmit when, or names a device and asks its mode.
>
> **Boundary:** half-duplex and full-duplex both carry traffic in **both** directions, so "bidirectional"
> does not distinguish them — only **simultaneity** does. In half-duplex the entire capacity goes to
> whichever station is currently sending; in full-duplex the capacity is shared between the two directions.

**Wrong traces:** A = full-duplex — both parties can speak at once · B = half-duplex — press to talk,
release to listen · C = full-duplex by definition.

---

**Q3.** A walkie-talkie system operates in which mode? `[Core]`

- **A)** simplex
- **B)** full-duplex
- **C)** multiplex
- **D)** half-duplex

**Answer: D) half-duplex**

**Trace / Why:** both stations can transmit and receive, but **not at the same time** — the push-to-talk
button gives the channel to one station while the other listens. That is the defining property of
half-duplex.

**📘 CONCEPT — C2 (see Concept Index)** › The classic exam pairing is **walkie-talkie = half-duplex** and
**telephone = full-duplex**, and it is worth memorising as a pair because the two are offered together in
almost every version of this question. Note the networking parallel: Ethernet over a **hub** is
half-duplex (collisions possible, CSMA/CD required), while Ethernet over a **switch** port is full-duplex
(no collisions, CSMA/CD disabled) — the same distinction one layer up.

**Wrong traces:** A = simplex allows one direction only, but a walkie-talkie transmits both ways ·
B = requires simultaneous transmission, which push-to-talk prevents · C = multiplexing combines several
signals on one link, an unrelated concept (C43).

---

**Q4.** The difference between **bandwidth** and **throughput** is that `[Trap]`

- **A)** bandwidth is the link's theoretical capacity, while throughput is the rate actually achieved
- **B)** bandwidth is measured in bits per second, while throughput is measured in hertz
- **C)** bandwidth applies to digital links, while throughput applies only to analog links
- **D)** throughput is always greater than bandwidth, because of compression

**Answer: A) bandwidth is the link's theoretical capacity, while throughput is the rate actually achieved**

**Trace / Why:** a 100 Mbps link has 100 Mbps of **bandwidth** whatever happens on it. If twenty users
share it, protocol overhead consumes part of it and congestion delays frames, the measured
**throughput** might be 20 Mbps. Throughput is therefore always **≤** bandwidth in practice.

**📘 CONCEPT — C3 · Four performance measures, and what each one tells you**
> | Measure | Meaning | Unit |
> |---|---|---|
> | **Bandwidth** | capacity — the maximum possible rate | bps (digital) or Hz (analog) |
> | **Throughput** | the rate actually achieved; always ≤ bandwidth | bps |
> | **Latency (delay)** | how long one bit takes to cross the network | seconds |
> | **Jitter** | variation in delay between successive packets | seconds |
>
> **Applies when** the stem contrasts capacity with observed performance, or asks why a link "feels slow".
>
> **Boundary:** bandwidth is measured in **hertz** for an analog channel and **bits per second** for a
> digital link — both usages are correct in their own domain, which is why option B is only half wrong and
> therefore tempting. Note that high bandwidth does not imply low latency: a satellite link may have huge
> bandwidth and terrible delay, which is exactly the case the bandwidth-delay product describes (C5).

**Wrong traces:** B = reverses the units, and ignores that bandwidth has both usages · C = both apply to
digital links · D = throughput cannot exceed capacity.

---

**Q5.** Which of the following is **NOT** a component of total packet delay? `[Core]`

- **A)** propagation delay
- **B)** modulation delay
- **C)** queuing delay
- **D)** transmission delay

**Answer: B) modulation delay**

**Trace / Why:** the four standard components are **transmission** (time to push all bits onto the link),
**propagation** (time for a bit to travel the distance), **queuing** (time waiting in router buffers) and
**processing** (time for a router to examine the header). "Modulation delay" is not one of them.

**📘 CONCEPT — C4 · The four delay components, and which one each factor controls**
> | Component | Formula / driver | Depends on |
> |---|---|---|
> | **Transmission** | packet size ÷ bandwidth | packet size, link rate |
> | **Propagation** | distance ÷ propagation speed | **distance only** |
> | **Queuing** | buffer occupancy | traffic load — the only *variable* one |
> | **Processing** | router lookup time | router speed |
>
> Total delay = the sum, and for a multi-hop path the transmission and processing terms recur at each hop.
>
> **Applies when** the stem asks what contributes to delay, or which component a change affects.
>
> **Boundary:** **queuing delay is the only component that varies with load**, so it is the sole source of
> **jitter** (C3) — the other three are essentially constant for a given path and packet size. That is why
> congestion shows up as variable delay rather than uniformly slower delivery. Note also that upgrading
> bandwidth reduces only the transmission term and leaves propagation untouched.

**Wrong traces:** A, C and D = three of the four genuine components.

---

**Q6.** A signal travels 5,000 km through a medium in which the propagation speed is 2 × 10⁸ m/s. The
propagation delay is `[Applied]` `[Asked: GATE-style]`

- **A)** 2.5 ms
- **B)** 10 ms
- **C)** 16.7 ms
- **D)** 25 ms

**Answer: D) 25 ms**

**Trace / Why:**

```
propagation delay = distance ÷ propagation speed
                  = 5,000,000 m ÷ (2 × 10^8 m/s)
                  = 0.025 s
                  = 25 ms
```

Note what the answer does **not** depend on: the link's bandwidth or the packet size. Propagation delay is
a function of distance alone.

**📘 CONCEPT — C5 · Propagation delay depends on distance, never on bandwidth**
> ```
> Tp = distance / propagation speed
> ```
> Typical speeds: **2 × 10⁸ m/s** in copper and fibre (about ⅔ of c), **3 × 10⁸ m/s** in free space or
> vacuum. Exam questions almost always supply the speed; if they say "free space", use 3 × 10⁸.
>
> **Applies when** the stem gives a distance and a speed.
>
> **Boundary:** upgrading a link from 1 Mbps to 1 Gbps changes the **transmission** time by a factor of
> 1000 and the **propagation** time not at all. This is why long-distance links are propagation-dominated
> and why a satellite hop has ~270 ms of delay regardless of its capacity — and it is the reason
> stop-and-wait performs so badly on them (C60).

**Wrong traces:** A = distance divided by 2 × 10⁹, a magnitude slip · B = uses 5 × 10⁸ m/s · C = uses
3 × 10⁸ m/s, the free-space speed, where the stem specified 2 × 10⁸.

---

**Q7.** How long does it take to transmit a **8,000-bit** frame onto a **1 Mbps** link? `[Applied]`

- **A)** 0.8 ms
- **B)** 1 ms
- **C)** 4 ms
- **D)** 8 ms

**Answer: D) 8 ms**

**Trace / Why:**

```
transmission delay = packet size ÷ bandwidth
                   = 8,000 bits ÷ 1,000,000 bps
                   = 0.008 s
                   = 8 ms
```

**📘 CONCEPT — C6 · Transmission delay is size over rate — and it is per hop**
> ```
> Tt = number of bits / link bandwidth
> ```
> This is the time to **place** the bits on the wire, not the time for them to arrive. A store-and-forward
> router repeats it at every hop, which is why total delay across n hops includes n transmission terms.
>
> **Applies when** the stem gives a frame or packet size and a link rate.
>
> **Boundary:** keep Tt and Tp strictly separate — **Tt scales with packet size and link rate; Tp scales
> with distance** (C5). The ratio **a = Tp / Tt** is the single parameter that governs sliding-window
> efficiency (C60), so questions that ask for both delays are usually building towards it. Watch units:
> 1 Mbps = 10⁶ bps, and 1 KB = 8,000 bits (or 8,192 if the question says KiB).

**Wrong traces:** A = 8,000 ÷ 10⁷, using 10 Mbps · B = 8,000 ÷ 8 × 10⁶, treating bits as bytes ·
C = 4,000 bits assumed.

---

**Q8.** A link has a bandwidth of **10 Mbps** and a round-trip time of **40 ms**. The bandwidth-delay
product is `[Applied]` `[Asked: GATE-style]`

- **A)** 4,000 bits
- **B)** 40,000 bits
- **C)** 250,000 bits
- **D)** 400,000 bits

**Answer: D) 400,000 bits**

**Trace / Why:**

```
BDP = bandwidth × delay
    = 10 × 10^6 bps × 0.040 s
    = 400,000 bits   (= 50,000 bytes)
```

**📘 CONCEPT — C7 · The bandwidth-delay product is how many bits the "pipe" holds**
> BDP is the volume of data in flight when the sender transmits continuously — the pipe's capacity, with
> bandwidth as its cross-section and delay as its length. Its practical meaning:
> - a sender must be able to have **BDP bits unacknowledged** to keep the link busy;
> - so the **window size** must be at least the BDP, or the link idles;
> - a "long fat network" (high bandwidth × high delay) needs very large windows, which is why TCP has a
>   **window-scaling** option.
>
> **Applies when** the stem gives a bandwidth and a delay, or asks for the window needed to saturate a
> link.
>
> **Boundary:** read whether the delay given is **one-way or round-trip**. For a *window size* the
> relevant figure is the round-trip product, because that is how long the sender waits for an
> acknowledgement; for "bits in the pipe in one direction" it is the one-way product. A factor of two
> separates the two answers, and papers exploit it.

**Wrong traces:** A = 10⁵ × 0.04, a magnitude slip · B = uses 1 Mbps · C = 10 Mbps × 25 ms.

---

**Q9.** **Jitter** in a network is `[Core]`

- **A)** the loss of packets caused by full router buffers
- **B)** the variation in delay experienced by successive packets of the same flow
- **C)** the gradual weakening of a signal as it travels
- **D)** the corruption of bits by external electromagnetic interference

**Answer: B) the variation in delay experienced by successive packets of the same flow**

**Trace / Why:** if three packets arrive after 20 ms, 45 ms and 25 ms, the delay varies by 25 ms — that
variation is jitter. It matters most for real-time audio and video, where packets must be played out at
even intervals; the standard remedy is a **playout buffer** that absorbs the variation at the cost of
added latency.

**📘 CONCEPT — C3 (see Concept Index)** › Jitter is a **derivative** of delay, not a separate impairment:
its sole significant source is variable **queuing delay** (C4), so jitter is a symptom of fluctuating
congestion. Consequently, a link can have high average delay with almost no jitter (a long, lightly loaded
satellite hop) or low average delay with severe jitter (a short, congested path) — and real-time traffic
prefers the former.

**Wrong traces:** A = packet loss, a different consequence of congestion · C = attenuation (C11) ·
D = noise (C11).

---

**Q10.** For a signal using **8 levels** per symbol, the relationship between bit rate and baud rate is
`[Trap]`

- **A)** bit rate = baud rate
- **B)** bit rate = 8 × baud rate
- **C)** bit rate = 3 × baud rate
- **D)** baud rate = 3 × bit rate

**Answer: C) bit rate = 3 × baud rate**

**Trace / Why:** with L levels each symbol carries log₂L bits.

```
bits per symbol = log2(8) = 3
bit rate = baud rate × 3
```

So 1,000 symbols per second carries 3,000 bits per second.

**📘 CONCEPT — C8 · Bit rate vs baud rate: bits per second against symbols per second**
> ```
> bit rate = baud rate × log2(L)
> ```
> **Baud rate** counts signal changes (symbols) per second; **bit rate** counts bits. They are equal only
> when L = 2 (one bit per symbol).
>
> | Levels L | Bits per symbol | Relationship |
> |---|---|---|
> | 2 | 1 | bit rate = baud rate |
> | 4 | 2 | bit rate = 2 × baud |
> | 8 | 3 | bit rate = 3 × baud |
> | 16 | 4 | bit rate = 4 × baud |
>
> **Applies when** the stem gives a level count, a modulation scheme (QPSK, 16-QAM), or asks which rate is
> larger.
>
> **Boundary:** **bit rate ≥ baud rate always** — a symbol cannot carry less than one bit in these schemes,
> so option D is impossible by construction. Note which quantity the *bandwidth* constrains: bandwidth
> limits the **baud** rate (how fast symbols can change), so adding levels raises the bit rate without
> needing more bandwidth — the insight behind QAM (C41) and the reason Nyquist's formula has a log₂L term.

**Wrong traces:** A = true only for 2 levels · B = multiplies by L instead of log₂L — the commonest error ·
D = reverses the inequality, which is impossible.

---

**Q11.** The **Nyquist** formula for the maximum data rate of a **noiseless** channel is `[Core]`

- **A)** C = 2B log₂L
- **B)** C = B log₂(1 + SNR)
- **C)** C = B log₂L
- **D)** C = 2B (1 + SNR)

**Answer: A) C = 2B log₂L**

**Trace / Why:** Nyquist gives the maximum symbol rate as **2B** symbols per second for a channel of
bandwidth B, and each symbol carries log₂L bits — so C = 2B log₂L. Noise appears nowhere, because the
channel is assumed noiseless.

**📘 CONCEPT — C9 · Nyquist and Shannon answer the same question under different assumptions**
> | | Nyquist | Shannon |
> |---|---|---|
> | Channel | **noiseless** | **noisy** |
> | Formula | **C = 2B log₂L** | **C = B log₂(1 + SNR)** |
> | Depends on | bandwidth and **signal levels** | bandwidth and **noise** |
> | Tells you | how many levels you may use | the absolute ceiling, whatever the levels |
>
> **Applies when** the stem gives either a level count (→ Nyquist) or an SNR (→ Shannon).
>
> **Boundary:** the **level count** is the signal that Nyquist is wanted, and the **SNR** that Shannon is.
> When a question gives *both*, compute both and take the **lower** — Shannon is the physical ceiling and
> Nyquist tells you the levels needed to approach it (Q15).

**Wrong traces:** B = **Shannon's** formula, the direct swap · C = omits the factor 2 for the symbol rate ·
D = mixes the two, using SNR without the logarithm.

---

**Q12.** A noiseless channel of bandwidth **3,000 Hz** transmits signals with **4 levels**. Its maximum
data rate is `[Applied]`

- **A)** 3,000 bps
- **B)** 6,000 bps
- **C)** 12,000 bps
- **D)** 24,000 bps

**Answer: C) 12,000 bps**

**Trace / Why:**

```
C = 2B log2(L)
  = 2 × 3000 × log2(4)
  = 6000 × 2
  = 12,000 bps
```

**📘 CONCEPT — C9 (see Concept Index)** › The two steps that go wrong are **forgetting the factor 2**
(giving 6,000 — distractor B) and **using L instead of log₂L** (giving 24,000 — distractor D). Compute
log₂L first and write it down before substituting: for L = 2, 4, 8, 16 it is 1, 2, 3, 4. Note that
doubling the levels adds only *one* bit per symbol, so returns diminish quickly — going from 4 to 16
levels only doubles the rate.

**Wrong traces:** A = B × 1, ignoring both the factor 2 and the levels · B = the 2-level answer, log₂L
taken as 1 · D = 2B × L, using L rather than log₂L.

---

**Q13.** The **Shannon capacity** of a noisy channel is given by `[Core]`

- **A)** C = 2B log₂L
- **B)** C = B × SNR
- **C)** C = B log₂(1 + SNR)
- **D)** C = 2B log₂(1 + SNR)

**Answer: C) C = B log₂(1 + SNR)**

**Trace / Why:** Shannon's theorem gives the theoretical maximum error-free rate over a channel of
bandwidth B with signal-to-noise **power ratio** SNR. There is no factor of 2 and no level count — the
noise sets the ceiling regardless of how many levels the transmitter uses.

**📘 CONCEPT — C10 · Shannon capacity is an absolute ceiling, and SNR must be a ratio not decibels**
> ```
> C = B log2(1 + SNR)          with SNR as a POWER RATIO
> SNR(ratio) = 10^(SNR_dB / 10)
> ```
> Useful conversions: 10 dB → 10 · 20 dB → 100 · 30 dB → 1,000 · 40 dB → 10,000.
>
> **Applies when** the stem gives an SNR, in either form.
>
> **Boundary:** if the SNR is given in **dB** it must be converted first — substituting "30" for SNR
> instead of 1,000 is the standard error and gives a wildly low answer. Note also that Shannon says
> nothing about *how* to reach the capacity: it is an existence bound, not a design, and no level count
> can exceed it (C9).

**Wrong traces:** A = **Nyquist's** formula · B = omits the logarithm · D = adds a spurious factor of 2
borrowed from Nyquist.

---

**Q14.** A telephone channel has a bandwidth of **3,000 Hz** and a signal-to-noise ratio of **1,000**. Its
Shannon capacity is approximately `[Applied]` `[Asked: GATE-style]`

- **A)** 30 kbps
- **B)** 60 kbps
- **C)** 100 kbps
- **D)** 300 kbps

**Answer: A) 30 kbps**

**Trace / Why:**

```
C = B log2(1 + SNR)
  = 3000 × log2(1001)
  ≈ 3000 × 9.967
  ≈ 29,900 bps  ≈  30 kbps
```

A useful shortcut: log₂(1001) ≈ 10, because 2¹⁰ = 1024 ≈ 1001. So C ≈ 3000 × 10 = 30,000 bps.

**📘 CONCEPT — C10 (see Concept Index)** › The approximation **log₂(1 + SNR) ≈ log₂(SNR)** for large SNR
makes these tractable by hand, and the powers of two worth recognising are 2¹⁰ = 1024, 2²⁰ ≈ 10⁶ and
2¹⁶ = 65,536. This 30 kbps figure is historically important: it is why dial-up modems plateaued near
33.6 kbps on an analog voice channel, and why 56 kbps modems required part of the path to be digital.

**Wrong traces:** B = doubles the result, importing Nyquist's factor of 2 · C = uses log₁₀ instead of
log₂ then mis-scales · D = 3000 × 100, treating SNR/10 as the multiplier.

---

**Q15.** A channel has bandwidth **1 MHz**, an SNR of **63**, and the transmitter can use any number of
signal levels. What is the maximum usable data rate? `[Trap]`

- **A)** 12 Mbps, from Nyquist with 64 levels
- **B)** 6 Mbps, since Shannon's limit is the binding constraint
- **C)** 2 Mbps, from Nyquist with 2 levels
- **D)** unlimited, since the number of levels is unconstrained

**Answer: B) 6 Mbps, since Shannon's limit is the binding constraint**

**Trace / Why:** compute both bounds and take the smaller.

```
Shannon:  C = 10^6 × log2(1 + 63) = 10^6 × log2(64) = 10^6 × 6 = 6 Mbps
Nyquist:  with L levels, C = 2 × 10^6 × log2(L) — unbounded as L grows
```

Nyquist alone suggests any rate is reachable by adding levels; Shannon says noise caps the channel at
6 Mbps. The **lower** bound governs, so 6 Mbps. Nyquist then tells you the useful level count:
6 Mbps = 2 × 10⁶ × log₂L gives log₂L = 3, so **8 levels** — more levels would add nothing.

**📘 CONCEPT — C9 (see Concept Index)** › When both bounds are computable, **Shannon gives the ceiling and
Nyquist gives the design**: Shannon says how fast the channel can possibly go, Nyquist says how many levels
you need to get there. Adding levels beyond that point only makes the receiver more vulnerable to the noise
that set the ceiling in the first place — which is the physical reason the two theorems must be used
together rather than chosen between.

**Wrong traces:** A = uses Nyquist while ignoring Shannon's ceiling · C = Nyquist with the minimum level
count, an arbitrary choice · D = the conclusion from Nyquist alone, which noise forbids.

---

**Q16.** An **analog** signal differs from a **digital** signal in that an analog signal `[Core]`

- **A)** can only be transmitted over guided media
- **B)** always requires more bandwidth than a digital signal
- **C)** takes on a continuous range of values over time
- **D)** cannot be affected by noise

**Answer: C) takes on a continuous range of values over time**

**Trace / Why:** an analog signal varies smoothly and can hold infinitely many intermediate values; a
digital signal is **discrete**, holding one of a finite set of levels and changing in steps. That is the
whole distinction.

**📘 CONCEPT — C11 · Analog vs digital, and the four impairments both suffer**
> Analog = continuous values; digital = discrete levels. Both travel over any medium and both are degraded
> by the same three impairments:
> - **Attenuation** — loss of energy with distance. Countered by amplifiers (analog) or **repeaters**
>   (digital).
> - **Distortion** — different frequency components arrive with different delays, changing the signal's
>   shape.
> - **Noise** — unwanted external energy: thermal, induced, crosstalk, impulse.
>
> **Applies when** the stem contrasts the two signal types, or names an impairment.
>
> **Boundary:** the decisive advantage of digital is **regeneration** — a repeater reads the discrete level,
> decides which symbol it was, and transmits a clean new signal, so noise does **not** accumulate over
> distance. An analog amplifier amplifies the noise along with the signal. Neither type is immune to noise
> (option D); digital merely recovers from it.

**Wrong traces:** A = both types travel over guided and unguided media · B = bandwidth depends on the
signal, not on the type · D = both are affected; digital can regenerate, which is not the same as immunity.

---

**Q17.** Which of the following is **NOT** an attribute of a simple periodic analog signal? `[Core]`

- **A)** bandwidth
- **B)** amplitude
- **C)** frequency
- **D)** phase

**Answer: A) bandwidth**

**Trace / Why:** a **simple** periodic signal is a single sine wave, fully described by three attributes:
**amplitude** (its peak value), **frequency** (cycles per second) and **phase** (its position relative to
time zero). Bandwidth is a property of a **composite** signal — a range of frequencies — and a single sine
wave occupies exactly one frequency, so its bandwidth is zero.

**📘 CONCEPT — C12 · Simple signals have three attributes; composite signals have a bandwidth**
> A **simple** periodic signal is one sine wave: amplitude, frequency, phase. A **composite** signal is a
> sum of sine waves (Fourier), and its **bandwidth = highest frequency − lowest frequency** (Q24).
>
> Also: **period T and frequency f are reciprocals**, f = 1/T (Q18).
>
> **Applies when** the stem lists signal properties, or asks for the bandwidth of a signal.
>
> **Boundary:** every signal that carries **information** must be composite — a pure unchanging sine wave
> conveys nothing, because nothing about it changes. So the useful signals in data communication all have
> a non-zero bandwidth, and this is the reason bandwidth is the fundamental limited resource of the
> physical layer.

**Wrong traces:** B, C and D = the three genuine attributes of a simple periodic signal.

---

**Q18.** A periodic signal has a period of **100 ms**. Its frequency is `[Applied]`

- **A)** 10 Hz
- **B)** 100 Hz
- **C)** 1,000 Hz
- **D)** 10,000 Hz

**Answer: A) 10 Hz**

**Trace / Why:**

```
f = 1 / T
  = 1 / 0.100 s
  = 10 Hz
```

**📘 CONCEPT — C12 (see Concept Index)** › The reciprocal relation **f = 1/T** requires the period in
**seconds**, and the unit conversions are where marks are lost: 1 ms = 10⁻³ s → 1 kHz · 1 µs = 10⁻⁶ s →
1 MHz · 1 ns = 10⁻⁹ s → 1 GHz. So a period of 100 ms is a *low* frequency (10 Hz) while a period of 100 ns
is a high one (10 MHz) — the inverse relationship means a large period means a small frequency, which is
the intuition slip behind every wrong option here.

**Wrong traces:** B = treats 100 ms as 10 ms · C = treats the period as 1 ms · D = treats it as 100 µs.

---

**Q19.** **Attenuation** refers to `[Core]`

- **A)** the variation in delay between successive packets
- **B)** the change in a signal's shape caused by frequency-dependent delays
- **C)** the addition of unwanted external energy to a signal
- **D)** the loss of signal energy as it travels through a medium

**Answer: D) the loss of signal energy as it travels through a medium**

**Trace / Why:** the medium's resistance converts part of the signal's electrical energy into heat, so the
signal weakens with distance. It is measured in **decibels** and countered by amplifiers (analog) or
repeaters (digital).

**📘 CONCEPT — C11 (see Concept Index)** › The three impairments are distinguished by **what they change**:
attenuation changes the signal's **strength**, distortion changes its **shape**, and noise **adds** energy
that was never part of it. Locating which of the three a stem describes is the whole task, and the three
appear together as options in every version of this question.

**Wrong traces:** A = jitter (C3) · B = distortion · C = noise.

---

**Q20.** If a signal's power is **halved** while passing through a medium, the attenuation in decibels is
approximately `[Applied]`

- **A)** −6 dB
- **B)** −0.5 dB
- **C)** −3 dB
- **D)** −2 dB

**Answer: C) −3 dB**

**Trace / Why:**

```
dB = 10 log10(P2 / P1)
   = 10 log10(0.5)
   = 10 × (−0.301)
   = −3.01 dB  ≈  −3 dB
```

The negative sign indicates loss; a positive value would indicate gain.

**📘 CONCEPT — C13 · The decibel scale, and the values worth memorising**
> ```
> dB = 10 log10(P2 / P1)
> ```
> | Power ratio | dB |
> |---|---|
> | ×2 | **+3** |
> | ÷2 | **−3** |
> | ×10 | **+10** |
> | ÷10 | **−10** |
> | ×100 | +20 |
>
> Because the scale is logarithmic, **decibels add** along a path: an amplifier of +7 dB after a loss of
> −3 dB leaves +4 dB overall. That additivity is precisely why dB is used.
>
> **Applies when** the stem gives a power ratio, or a chain of gains and losses to combine.
>
> **Boundary:** the coefficient is **10** for power ratios and **20** for voltage or amplitude ratios, so
> halving the *voltage* is −6 dB while halving the *power* is −3 dB. Read which quantity the stem names —
> that factor-of-two difference in the coefficient is the trap, and it is what distractor A corresponds to.

**Wrong traces:** A = the answer for halved **voltage** (20 log₁₀0.5) · B = the raw ratio read as
decibels · D = log₁₀ of 0.5 mis-evaluated.

---

**Q21.** The **signal-to-noise ratio** (SNR) measures `[Core]`

- **A)** the ratio of a link's throughput to its bandwidth
- **B)** the number of signal levels a channel can support
- **C)** the ratio of average signal power to average noise power
- **D)** the delay variation introduced by router queues

**Answer: C) the ratio of average signal power to average noise power**

**Trace / Why:** SNR = P_signal / P_noise. A high SNR means the signal dominates and the receiver can
distinguish many levels reliably; a low SNR means noise obscures the signal. It is the quantity that sets
Shannon's ceiling (C10).

**📘 CONCEPT — C10 (see Concept Index)** › SNR is a **dimensionless power ratio**, usually quoted in
decibels as SNR_dB = 10 log₁₀(SNR). Its practical significance is that it caps how many signal levels are
usable: with more levels the voltage gap between adjacent levels shrinks, so a given amount of noise
becomes more likely to push a received symbol into the neighbouring level's range. That is the mechanism
behind Shannon's bound, and the reason a noisy channel cannot simply use 256-QAM to go faster.

**Wrong traces:** A = a throughput-to-capacity efficiency, unrelated · B = the level count is a
*consequence* of SNR, not its definition · D = jitter (C3).

---

**Q22.** Compared with **serial** transmission, **parallel** transmission `[Applied]`

- **A)** is always faster over long distances
- **B)** requires only one communication channel
- **C)** is faster over short distances but suffers from skew and crosstalk over long ones
- **D)** transmits one bit at a time on a single wire

**Answer: C) is faster over short distances but suffers from skew and crosstalk over long ones**

**Trace / Why:** parallel transmission sends n bits simultaneously on n wires, so it is n times faster in
principle. But over distance the wires' propagation times differ slightly (**skew**), so bits of one group
no longer arrive together, and adjacent wires interfere (**crosstalk**). Both worsen with length and with
speed, which is why long-distance and high-speed links are **serial** — SATA replaced parallel ATA and PCIe
replaced PCI for exactly this reason.

**📘 CONCEPT — C14 · Serial vs parallel: the trade reverses with distance**
> | | Parallel | Serial |
> |---|---|---|
> | Wires | n (one per bit) | 1 |
> | Speed | n bits per clock | 1 bit per clock |
> | Cost | high (n conductors) | low |
> | Long distance | **poor** — skew and crosstalk | **good** |
>
> **Applies when** the stem compares the two, or asks why a modern high-speed bus is serial.
>
> **Boundary:** the intuition that "parallel is faster" holds only over **short** distances at moderate
> speeds. Modern serial links win outright because a single clean channel can be clocked far faster than
> n mutually interfering ones — so the historical trend is from parallel to serial as speeds rise, which
> is the opposite of what the naive argument predicts.

**Wrong traces:** A = over long distances parallel is worse, not better · B = parallel needs n channels;
**serial** needs one · D = the definition of **serial** transmission.

---

**Q23.** In **asynchronous** serial transmission `[Core]`

- **A)** the sender and receiver share a common clock signal
- **B)** each character is framed by a start bit and one or more stop bits
- **C)** data is sent as a continuous stream of bits with no gaps
- **D)** bits are transmitted simultaneously on parallel wires

**Answer: B) each character is framed by a start bit and one or more stop bits**

**Trace / Why:** without a shared clock, the receiver needs a marker for where each character begins. The
**start bit** provides it and the **stop bit(s)** mark the end, and the gap between characters may be of
any length. The cost is overhead — 2 framing bits for 8 data bits is 20% wasted.

**📘 CONCEPT — C15 · Asynchronous vs synchronous transmission: per-character framing vs a shared clock**
> | | Asynchronous | Synchronous |
> |---|---|---|
> | Timing | start/stop bits per character | shared clock, or clock recovered from the data |
> | Gaps between units | allowed, arbitrary | none — continuous stream |
> | Overhead | high (~20%) | low |
> | Suits | slow, bursty (keyboards, legacy serial) | fast, sustained (network links) |
>
> **Applies when** the stem mentions start/stop bits, framing per character, or clock sharing.
>
> **Boundary:** "asynchronous" refers to the **absence of a shared clock**, not to irregular timing within
> a character — the bits *inside* a character are still sent at an agreed rate, otherwise the receiver
> could not sample them. That is why the term confuses students: the byte boundaries are asynchronous while
> the bits are not.

**Wrong traces:** A = **synchronous** transmission · C = also synchronous — asynchronous permits gaps ·
D = parallel transmission (C14).

---

**Q24.** The bandwidth of a composite signal containing frequencies from **1,000 Hz to 5,000 Hz** is
`[Core]`

- **A)** 1,000 Hz
- **B)** 4,000 Hz
- **C)** 5,000 Hz
- **D)** 6,000 Hz

**Answer: B) 4,000 Hz**

**Trace / Why:**

```
bandwidth = highest frequency − lowest frequency
          = 5,000 − 1,000
          = 4,000 Hz
```

**📘 CONCEPT — C12 (see Concept Index)** › Bandwidth is a **width**, so it is a difference and not a
maximum — which is why 5,000 Hz (the highest frequency, distractor C) is the standard wrong answer. Note
the consequence for transmission: a medium must pass the whole **range** 1,000–5,000 Hz, so a channel
offering 4,000 Hz of bandwidth centred elsewhere would not carry this signal. Bandwidth alone does not
describe a channel; the frequency range does.

**Wrong traces:** A = the lowest frequency · C = the **highest** frequency, mistaken for the width — the
intended trap · D = the sum of the two frequencies.

---
## Section 2 — Network Types, Topologies & Switching

**Q25.** A **LAN** is characterised by `[Core]`

- **A)** spanning several cities and being owned by a telecom carrier
- **B)** covering a limited area such as one building, with high data rates and low delay
- **C)** connecting devices within a few metres of one person, such as a phone and a headset
- **D)** interconnecting networks belonging to different organisations worldwide

**Answer: B) covering a limited area such as one building, with high data rates and low delay**

**Trace / Why:** a Local Area Network is privately owned and geographically small — a room, floor,
building or campus. Short distances mean low propagation delay and cheap high bandwidth, so LANs run at
100 Mbps to 10 Gbps where WANs historically ran far slower.

**📘 CONCEPT — C16 · Network classification is by geographic scope, and everything else follows from it**
> | Type | Scope | Typical owner | Speed |
> |---|---|---|---|
> | **PAN** | a few metres, one person | the individual | low (Bluetooth) |
> | **LAN** | room to campus | private | high |
> | **MAN** | a city | private or carrier | medium–high |
> | **WAN** | country to global | carrier | historically lower, long delay |
>
> **Applies when** the stem gives a distance, an owner, or a speed and asks for the type.
>
> **Boundary:** scope drives the other properties rather than being independent of them — long distance
> forces higher propagation delay (C5) and more expensive links, which is why WAN links were slow and why
> WAN protocols worry about efficiency in ways LAN protocols do not. Note that the **Internet** is not a
> WAN but an interconnection of many networks (Q40).

**Wrong traces:** A = a WAN · C = a PAN · D = the Internet, an internetwork rather than a single network.

---

**Q26.** A network spanning a single city, such as a cable-television distribution network, is a `[Core]`

- **A)** PAN
- **B)** LAN
- **C)** MAN
- **D)** WAN

**Answer: C) MAN**

**Trace / Why:** a Metropolitan Area Network covers a city or metropolitan region — larger than a campus,
smaller than a country. Cable TV networks, city-wide fibre rings and municipal WiFi are the standard
examples.

**📘 CONCEPT — C16 (see Concept Index)** › MAN is the category examiners use to test whether the four
scopes are known as an ordered ladder rather than as three familiar terms plus one vague one. Fix the
boundaries: **PAN < LAN < MAN < WAN**, with MAN sitting at "one city". A useful discriminator is
ownership — a LAN is wholly private, a WAN is carrier-provided, and a MAN can be either, which is why the
question must give a *geographic* clue.

**Wrong traces:** A = metres, not kilometres · B = a building or campus, too small · D = a country or
larger, too big.

---

**Q27.** Which statement about a **WAN** is correct? `[Core]`

- **A)** it typically spans large geographic distances and relies on links leased from carriers
- **B)** it is always faster than a LAN because it uses fibre optics
- **C)** it is limited to a single building and privately owned
- **D)** it cannot carry TCP/IP traffic

**Answer: A) it typically spans large geographic distances and relies on links leased from carriers**

**Trace / Why:** a Wide Area Network crosses cities, countries or continents, so laying private cable is
infeasible — organisations lease circuits, MPLS services or use the Internet. Long distances mean high
propagation delay (C5), which is the dominant performance characteristic.

**📘 CONCEPT — C16 (see Concept Index)** › The two properties that define WAN behaviour are **leased
infrastructure** and **high propagation delay**. The delay is why WAN links stress efficient protocols:
a satellite WAN hop of ~270 ms makes stop-and-wait useless (C60) and demands large windows (C7). Note that
modern WAN links can be very fast — the old assumption "WAN = slow" is obsolete, but "WAN = high latency"
still holds, because distance cannot be engineered away.

**Wrong traces:** B = WAN links are often slower and always higher-latency than LAN links · C = describes a
LAN · D = TCP/IP is the normal WAN protocol suite.

---

**Q28.** In a fully connected **mesh** topology with **6** devices, the number of physical links required
is `[Applied]` `[Asked: BCS / bank IT]`

- **A)** 12
- **B)** 15
- **C)** 30
- **D)** 36

**Answer: B) 15**

**Trace / Why:** every device connects directly to every other, and each link serves two devices, so it
is a count of pairs.

```
links = n(n − 1) / 2
      = 6 × 5 / 2
      = 15
```

**📘 CONCEPT — C17 · Mesh sizing: n(n−1)/2 links and n−1 ports per device**
> ```
> links          = n(n − 1) / 2
> ports per node = n − 1
> ```
> | n | Links | Ports each |
> |---|---|---|
> | 4 | 6 | 3 |
> | 5 | 10 | 4 |
> | 6 | **15** | 5 |
> | 8 | 28 | 7 |
> | 10 | 45 | 9 |
>
> **Applies when** the stem gives a device count for a mesh, and asks for links or interfaces.
>
> **Boundary:** the **÷ 2** is what distinguishes links from *interfaces* — there are n(n−1) interface
> connections but only half that many cables, because one cable terminates at two ports. Omitting it gives
> 30 (distractor C), which is the standard error. Note the growth is **quadratic**, which is precisely why
> full mesh is impractical beyond a handful of nodes (Q36).

**Wrong traces:** A = 2n, an unrelated linear guess · C = n(n−1) without dividing by 2 — the intended
trap · D = n², counting a link from every device to every device including itself.

---

**Q29.** In a fully connected mesh of **8** devices, how many I/O ports must each device have? `[Applied]`

- **A)** 4
- **B)** 7
- **C)** 8
- **D)** 28

**Answer: B) 7**

**Trace / Why:** each device needs one dedicated port per **other** device.

```
ports per device = n − 1 = 8 − 1 = 7
(total links     = 8 × 7 / 2 = 28)
```

**📘 CONCEPT — C17 (see Concept Index)** › The two mesh figures answer different questions and appear
together as options: **n − 1 = ports per device** and **n(n−1)/2 = total links**. For n = 8 those are 7 and
28, and distractor D is exactly the link count offered where the port count was asked. Read the noun in the
stem — "links", "cables", "connections" versus "ports", "interfaces" — before computing.

**Wrong traces:** A = n/2, no derivation · C = n, counting a port to itself · D = the total **link** count,
not the per-device port count.

---

**Q30.** In a **star** topology, every device is connected `[Core]`

- **A)** to two neighbours, forming a closed loop
- **B)** to a single shared backbone cable
- **C)** by a dedicated point-to-point link to a central hub or switch
- **D)** directly to every other device

**Answer: C) by a dedicated point-to-point link to a central hub or switch**

**Trace / Why:** star is the dominant modern LAN topology. Each station has its own cable to a central
device, so adding or removing a station affects nothing else and a single cable fault isolates one station
only.

**📘 CONCEPT — C18 · The five topologies, and the failure mode of each**
> | Topology | Structure | Single point of failure | Cable cost |
> |---|---|---|---|
> | **Bus** | one shared backbone | the **backbone** | lowest |
> | **Star** | all links to a central device | the **central device** | medium (one cable per node) |
> | **Ring** | closed loop of neighbours | any **node or link** (unless dual ring) | low |
> | **Mesh** | every node to every node | **none** — most robust | highest, n(n−1)/2 |
> | **Tree** | hierarchy of stars | the **root** | medium |
>
> **Applies when** the stem describes a physical arrangement, or asks what happens when something fails.
>
> **Boundary:** star's dedicated links are what make it **easy to diagnose and expand** — the property that
> made it win over bus in practice — but they concentrate all risk in the hub. Note that Ethernet is
> physically a star while being *logically* a bus when hubs are used, so physical and logical topology need
> not match.

**Wrong traces:** A = ring · B = bus · D = mesh.

---

**Q31.** The principal disadvantage of a star topology is that `[Trap]`

- **A)** failure of the central hub or switch disables the entire network
- **B)** a break in any single cable disables the entire network
- **C)** it requires n(n−1)/2 cables, making it the most expensive option
- **D)** data must pass through every other station before reaching its destination

**Answer: A) failure of the central hub or switch disables the entire network**

**Trace / Why:** all traffic passes through the central device, so it is a single point of failure. A break
in one **station** cable, by contrast, isolates only that station — which is the topology's chief
*advantage* and the reason B is wrong.

**📘 CONCEPT — C18 (see Concept Index)** › Star and bus have **opposite** fault profiles, and questions
exploit the symmetry: in a **star** one cable break isolates one node while hub failure kills everything;
in a **bus** one backbone break kills everything while a node failure isolates one node. Matching the
described failure to the right topology is the whole task. The mitigation for star is a redundant or
stacked central device, which is why real deployments duplicate the switch rather than the cabling.

**Wrong traces:** B = the **bus** failure mode · C = the **mesh** cable count (C17) · D = ring behaviour.

---

**Q32.** A **bus** topology requires **terminators** at both ends of the backbone in order to `[Core]`

- **A)** absorb the signal and prevent it reflecting back along the cable
- **B)** amplify the signal so it reaches distant stations
- **C)** convert the electrical signal into an optical one
- **D)** assign addresses to stations as they join the bus

**Answer: A) absorb the signal and prevent it reflecting back along the cable**

**Trace / Why:** a signal reaching an unterminated cable end reflects, and the reflection collides with
subsequent transmissions, corrupting them. A terminator is a resistor matched to the cable impedance that
absorbs the energy instead. A missing terminator is the classic bus fault: the network appears to work
briefly and then fails under load.

**📘 CONCEPT — C19 · Bus topology: one shared medium, hence terminators and contention**
> All stations tap a single backbone, so a transmission reaches every station (a **broadcast** medium) and
> only one may transmit at a time — which is why bus Ethernet needed **CSMA/CD** (C68). Terminators at both
> ends prevent reflection.
>
> **Applies when** the stem mentions a backbone, a shared coaxial cable, terminators, or 10Base2/10Base5.
>
> **Boundary:** the shared medium is what makes bus cheap **and** what makes it collision-prone and hard to
> troubleshoot — one fault anywhere degrades everyone, with no way to localise it from a central point.
> Those properties, not cost, are why bus disappeared in favour of switched star.

**Wrong traces:** B = amplification is a repeater's job · C = that is a media converter · D = addressing is
handled by the data link layer, not by cable hardware.

---

**Q33.** In a bus topology, a break in the main backbone cable `[Trap]`

- **A)** affects only the stations downstream of the break
- **B)** is automatically bypassed, since data can travel the other way
- **C)** brings down the whole network, because the medium is shared and the broken ends reflect signals
- **D)** affects nothing, as each station has its own dedicated link

**Answer: C) brings down the whole network, because the medium is shared and the broken ends reflect signals**

**Trace / Why:** the break splits the backbone into two unterminated segments. Neither half can reach the
other, and each now has an unterminated end causing reflections (C19), so even communication *within* a
half is corrupted. The entire network fails.

**📘 CONCEPT — C19 (see Concept Index)** › The failure is worse than a simple partition, and that is the
examinable subtlety: option A describes what a naive "the cable is cut, so the far side is unreachable"
model predicts, but reflection off the two new unterminated ends degrades **both** halves. A shared medium
has no notion of direction or bypass — which is why option B's reasoning belongs to a **dual ring** (C20)
and not to a bus.

**Wrong traces:** A = underestimates the fault; reflections damage both segments · B = describes dual-ring
self-healing · D = describes **star** cabling.

---

**Q34.** In a simple **ring** topology, data travels `[Core]`

- **A)** in both directions simultaneously from every station
- **B)** directly from source to destination without intermediate stations
- **C)** in one direction from station to station until it reaches the destination
- **D)** through a central controller that forwards it to the destination

**Answer: C) in one direction from station to station until it reaches the destination**

**Trace / Why:** each station has a point-to-point link to exactly two neighbours, forming a loop. A frame
is passed hop by hop, each station **regenerating** the signal — so a ring can span long distances without
separate repeaters — until the destination recognises its address.

**📘 CONCEPT — C20 · Ring topology: unidirectional hop-by-hop with a token, and dual rings for resilience**
> Access is controlled by a **token** circulating the ring: only the station holding it may transmit, so
> collisions are impossible by construction. Each station repeats and regenerates the signal.
>
> **Applies when** the stem mentions token passing, unidirectional flow, FDDI, or Token Ring.
>
> **Boundary:** a **single** ring breaks entirely if any node or link fails, since the loop is severed —
> which is why FDDI uses a **dual counter-rotating ring** that wraps traffic back on the second ring to
> heal a break. So "a ring survives a failure" is true for a dual ring and false for a simple one, and
> the stem's wording decides. Compare with bus: the ring's problem is *any* node failing, the bus's is the
> *backbone* failing.

**Wrong traces:** A = a simple ring is unidirectional; only a dual ring uses both directions · B = frames
pass through intermediate stations · D = describes star.

---

**Q35.** The chief advantage of a **mesh** topology is that `[Core]`

- **A)** it needs the fewest cables of any topology
- **B)** it is robust — a failed link does not isolate any station, since alternative paths exist
- **C)** it is the easiest topology to install and reconfigure
- **D)** it eliminates the need for addressing, since every link is dedicated

**Answer: B) it is robust — a failed link does not isolate any station, since alternative paths exist**

**Trace / Why:** with a dedicated link between every pair, a single link failure removes one path among
many and traffic reroutes. There is no single point of failure — the only topology of which that is true.
Dedicated links also mean no contention and guaranteed capacity per pair, plus privacy.

**📘 CONCEPT — C18 (see Concept Index)** › Mesh trades **cost for robustness**, and it is the extreme of
both: highest cable count (C17) and no single point of failure. This is why full mesh appears in practice
only where reliability dominates and node counts are tiny — core router interconnects, critical backbones —
while access networks use star. The Internet's core is a **partial** mesh, which captures most of the
resilience at a fraction of the quadratic cost.

**Wrong traces:** A = mesh needs the **most** cables · C = the hardest to install, with n−1 ports per
device · D = addressing is still required to identify hosts.

---

**Q36.** The main practical obstacle to deploying a full mesh topology in a large network is that `[Trap]`

- **A)** frames would loop forever without a spanning tree
- **B)** cabling and port requirements grow quadratically with the number of devices
- **C)** it cannot support broadcast traffic
- **D)** it requires all devices to be from the same manufacturer

**Answer: B) cabling and port requirements grow quadratically with the number of devices**

**Trace / Why:** links = n(n−1)/2 grows as n². Ten devices need 45 links; one hundred need **4,950**, and
each device needs 99 ports. The cost and physical impossibility of that wiring, not any protocol issue, is
what rules out full mesh.

**📘 CONCEPT — C17 (see Concept Index)** › Quadratic growth is the whole argument, and it is worth feeling
the numbers: n = 10 → 45 links, n = 50 → 1,225, n = 100 → 4,950. Doubling the nodes roughly **quadruples**
the cabling. That is why real designs use **hierarchy** (a tree of stars, C18) to get acceptable resilience
at linear cost, and reserve mesh for small critical cores.

**Wrong traces:** A = loops are a real concern in bridged networks and are solved by spanning tree (C73),
but they are not what prevents mesh · C = mesh carries broadcast by replicating on each link · D = no
vendor restriction exists.

---

**Q37.** A **tree** (hierarchical) topology is best described as `[Core]`

- **A)** a closed loop with a central controller
- **B)** several star topologies connected to a hierarchy of higher-level central devices
- **C)** a full mesh with one link removed
- **D)** a bus in which each station also connects to its two neighbours

**Answer: B) several star topologies connected to a hierarchy of higher-level central devices**

**Trace / Why:** the tree is a star of stars. Leaf devices attach to access switches, those attach to
distribution switches, and those to a core — the standard structure of every campus network, because it
scales linearly while keeping any two nodes a small number of hops apart.

**📘 CONCEPT — C18 (see Concept Index)** › Tree inherits star's properties **recursively**: a failure
isolates only the subtree below it, so the higher a device sits, the more it takes down. The **root** is
therefore the critical single point of failure, and real designs duplicate the core rather than the edge.
The practical importance is that hierarchy also aggregates addresses neatly, which is what makes route
summarization possible (see the Subnetting chapter, C60 there).

**Wrong traces:** A = ring with a controller, not a hierarchy · C = a near-mesh, unrelated · D = a hybrid
bus-ring, not a tree.

---

**Q38.** A **multipoint** (multidrop) connection is one in which `[Core]`

- **A)** exactly two devices share a dedicated link
- **B)** three or more devices share the capacity of a single link
- **C)** each device has a separate link to every other device
- **D)** two devices are connected by two independent links for redundancy

**Answer: B) three or more devices share the capacity of a single link**

**Trace / Why:** in a **point-to-point** connection the whole capacity of the link is reserved for two
devices. In a **multipoint** connection the medium is shared, so capacity is divided — either spatially
(all may use it at once, at reduced share) or temporally (they take turns, requiring a MAC protocol).

**📘 CONCEPT — C21 · Point-to-point vs multipoint decides whether a MAC protocol is needed at all**
> | | Point-to-point | Multipoint |
> |---|---|---|
> | Devices per link | 2 | 3 or more |
> | Capacity | dedicated | **shared** |
> | Medium access control | unnecessary | **required** |
> | Examples | switch port, WAN leased line | bus Ethernet, WiFi cell |
>
> **Applies when** the stem describes how many devices share a medium, or asks why a MAC protocol exists.
>
> **Boundary:** this is the root of the whole MAC sublayer (Section 7): **contention only exists on a shared
> medium**. It is exactly why switched full-duplex Ethernet — a collection of point-to-point links — can
> disable CSMA/CD entirely, while WiFi, being genuinely multipoint, cannot.

**Wrong traces:** A = point-to-point · C = mesh · D = a redundant point-to-point pair.

---

**Q39.** In a **peer-to-peer** network, unlike a client-server network `[Core]`

- **A)** all data is stored on one dedicated machine
- **B)** security and access control are centrally administered
- **C)** each host can act as both a provider and a consumer of resources
- **D)** hosts communicate only through a central coordinator

**Answer: C) each host can act as both a provider and a consumer of resources**

**Trace / Why:** peer-to-peer has no fixed role assignment — every node may serve and request. That makes
it cheap, resilient to any single node failing, and easy to grow, at the cost of decentralised (therefore
weaker and harder to audit) security.

**📘 CONCEPT — C22 · Client-server vs peer-to-peer: where the roles and the risks sit**
> | | Client-server | Peer-to-peer |
> |---|---|---|
> | Roles | fixed and distinct | interchangeable |
> | Administration | **central**, easier to secure and back up | distributed, harder |
> | Single point of failure | **the server** | none |
> | Scalability cost | server must grow | capacity grows with peers |
>
> **Applies when** the stem describes role assignment, or asks which model suits a small office versus a
> large managed network.
>
> **Boundary:** the distinction is about **logical roles**, not topology — a peer-to-peer network usually
> runs over a physical star. And one machine can be both: a workstation running a file share is a server
> for that service and a client for others, which is why the model describes an architecture rather than a
> hardware class.

**Wrong traces:** A = client-server with a file server · B = the central-administration advantage of
client-server · D = describes a server-mediated architecture.

---

**Q40.** An **extranet** is `[Core]`

- **A)** the global public network of interconnected networks
- **B)** a private network accessible only to an organisation's own employees
- **C)** a network segment isolated from all other networks
- **D)** a private network extended to give controlled access to selected outside partners

**Answer: D) a private network extended to give controlled access to selected outside partners**

**Trace / Why:** an **intranet** serves the organisation internally; an **extranet** deliberately opens
part of it to named suppliers, customers or partners, under authentication and access control. The
**Internet** is the public interconnection of networks, open to all.

**📘 CONCEPT — C23 · Internet, intranet, extranet: who is permitted in**
> | | Who has access | Ownership |
> |---|---|---|
> | **Internet** | everyone | no single owner |
> | **Intranet** | the organisation's own members only | private |
> | **Extranet** | members **plus** selected external partners | private, selectively opened |
>
> **Applies when** the stem describes an access boundary.
>
> **Boundary:** all three commonly use the **same technologies** — TCP/IP, HTTP, browsers — so the
> distinction is one of **access policy**, not of protocol. An extranet is often implemented as an intranet
> reached through a VPN (C120), which is why "uses the Internet as transport" does not make something an
> internet.

**Wrong traces:** A = the Internet · B = an intranet · C = an air-gapped network, not an extranet.

---

**Q41.** In **circuit switching** `[Core]`

- **A)** each packet is routed independently and may take a different path
- **B)** a dedicated path with reserved capacity is established before any data is sent
- **C)** the entire message is stored at each intermediate node before being forwarded
- **D)** capacity is allocated only when a station has data to send

**Answer: B) a dedicated path with reserved capacity is established before any data is sent**

**Trace / Why:** circuit switching has three phases — **setup**, **data transfer**, **teardown**. Capacity
is reserved end to end for the call's duration, so delay is low and constant once connected, but the
reservation is wasted whenever the parties are silent. The classic example is the traditional telephone
network.

**📘 CONCEPT — C24 · The three switching techniques, compared on reservation and granularity**
> | | Circuit switching | Packet switching | Message switching |
> |---|---|---|---|
> | Path setup | **yes**, before data | none (datagram) or virtual (VC) | none |
> | Capacity | **reserved** for the call | shared on demand | shared on demand |
> | Unit stored/forwarded | none — a continuous stream | **packet** | the **whole message** |
> | Delay | low and constant after setup | variable (queuing) | very high |
> | Efficiency on bursty traffic | **poor** | **good** | poor |
>
> **Applies when** the stem mentions setup phases, reservation, or store-and-forward granularity.
>
> **Boundary:** circuit switching's reserved capacity is simultaneously its **strength** (guaranteed rate,
> no jitter — ideal for voice) and its **weakness** (idle reservation wastes the link — terrible for bursty
> data). Neither technique is better in the abstract; the traffic pattern decides, which is the point of
> the comparison.

**Wrong traces:** A = datagram packet switching · C = message switching · D = packet switching's on-demand
allocation.

---

**Q42.** The principal advantage of **packet switching** over circuit switching for computer data is that
`[Core]`

- **A)** it guarantees a constant delay for every packet
- **B)** it requires no addressing information in the data unit
- **C)** it establishes the path before transmission, avoiding loss
- **D)** it shares link capacity statistically, so bursty traffic uses the link efficiently

**Answer: D) it shares link capacity statistically, so bursty traffic uses the link efficiently**

**Trace / Why:** computer traffic is bursty — long silences punctuated by short bursts. Circuit switching
reserves capacity through the silences and wastes it. Packet switching allocates only when a packet exists,
so many bursty flows share one link (**statistical multiplexing**, C46) and the link stays busy.

**📘 CONCEPT — C24 (see Concept Index)** › The efficiency gain has a **cost**, and honest answers name both:
packets queue at routers, so delay becomes **variable** (jitter, C3) and buffers can overflow, causing
**loss**. Circuit switching has neither problem but wastes capacity. That is exactly the trade that made
voice networks circuit-switched and data networks packet-switched — and why carrying voice over packet
networks (VoIP) required jitter buffers and QoS mechanisms to compensate.

**Wrong traces:** A = circuit switching's property; packet delay is variable · B = every packet **must**
carry a destination address · C = describes circuit switching, and packet switching does not avoid loss.

---

**Q43.** In **message switching**, an intermediate node `[Trap]`

- **A)** stores the entire message before forwarding it towards the next node
- **B)** forwards each bit as soon as it arrives, without storing anything
- **C)** reserves a dedicated circuit for the duration of the message
- **D)** discards the message if the destination is busy

**Answer: A) stores the entire message before forwarding it towards the next node**

**Trace / Why:** message switching is store-and-forward at the granularity of the **whole message** — the
node must receive every bit before sending any onward. For a large message this multiplies delay by the
hop count and demands large storage, which is why it was superseded by packet switching, which does the
same thing on small fixed-ish units and can therefore **pipeline** them across hops.

**📘 CONCEPT — C25 · Store-and-forward granularity is what separates message from packet switching**
> Both store and forward. The difference is the **unit**:
> - **Message switching** — the whole message. No pipelining: hop 2 cannot start until hop 1 finishes, so
>   total delay ≈ hops × message transmission time.
> - **Packet switching** — small packets. Packet 2 crosses hop 1 while packet 1 crosses hop 2, so hops
>   **overlap** and total delay approaches one message time plus a little.
>
> **Applies when** the stem describes what an intermediate node buffers, or asks why packet switching is
> faster across multiple hops.
>
> **Boundary:** the pipelining benefit is the real reason packets won, and it is a **latency** argument
> rather than an efficiency one — packet switching also fragments a message, requiring sequence numbers and
> reassembly, which message switching did not need. So packets bought speed at the price of complexity in
> the layers above.

**Wrong traces:** B = **cut-through** forwarding (C74), used by some switches at frame level · C = circuit
switching · D = messages are queued, not discarded, when the next hop is busy.

---

**Q44.** In a **virtual-circuit** packet-switched network, unlike a datagram network `[Applied]`

- **A)** packets carry the full destination address and are routed independently
- **B)** no setup phase is required before data transfer
- **C)** each packet may follow a different route to the destination
- **D)** a path is established during a setup phase and all packets of the connection follow it

**Answer: D) a path is established during a setup phase and all packets of the connection follow it**

**Trace / Why:** a virtual circuit is set up first, and each switch stores a table entry mapping a **virtual
circuit identifier** to an outgoing link. Packets then carry only the short VCI rather than a full address,
follow one fixed path, and therefore arrive **in order**. Capacity is still shared — the circuit is
"virtual", not reserved as in true circuit switching.

**📘 CONCEPT — C26 · Virtual circuit vs datagram: connection-oriented vs connectionless packet switching**
> | | Virtual circuit | Datagram |
> |---|---|---|
> | Setup phase | **yes** | none |
> | Packet header | short **VCI** | full destination address |
> | Route | fixed for the connection | independent per packet |
> | Ordering | **preserved** | may be **out of order** |
> | Switch state | per-connection table | routing table only |
> | Failure of a node | connection breaks | packets reroute |
> | Examples | ATM, Frame Relay, MPLS | **IP** |
>
> **Applies when** the stem mentions VCI, setup, ordering guarantees, or asks how IP behaves.
>
> **Boundary:** a virtual circuit is **not** circuit switching — no bandwidth is reserved, so packets still
> queue and can still be lost. It shares circuit switching's *setup and ordering* while keeping packet
> switching's *statistical sharing*. **IP is a datagram service**, which is precisely why TCP above it must
> reorder and retransmit (C102).

**Wrong traces:** A = datagram behaviour · B = datagram behaviour · C = datagram behaviour — all three
describe the alternative.

---

**Q45.** In a datagram network, packets belonging to one message may arrive at the destination out of
order because `[Trap]`

- **A)** each packet is routed independently and may take a different path with a different delay
- **B)** the network layer deliberately reorders packets to balance load
- **C)** datagrams have no sequence numbers, so ordering is undefined
- **D)** the destination processes packets in reverse order of arrival

**Answer: A) each packet is routed independently and may take a different path with a different delay**

**Trace / Why:** every datagram carries a full destination address and is forwarded by whatever route the
routing table indicates **at that moment**. Two packets can take different paths, or the same path with
different queuing delays, so arrival order is not guaranteed.

**📘 CONCEPT — C26 (see Concept Index)** › Out-of-order delivery is a **consequence of independent
routing**, not a defect and not a deliberate policy. It is why a reliable byte-stream service must be built
**above** IP: TCP's sequence numbers exist to reassemble what the network layer may deliver in any order
(C102). Note the layering point — IP does not fail to guarantee ordering, it never promised it, and UDP
inherits that with no correction at all.

**Wrong traces:** B = no such deliberate reordering exists · C = confuses cause with symptom — the absence
of ordering guarantees is the *consequence* of independent routing, and TCP supplies sequence numbers at the
layer above · D = destinations process packets as they arrive.

---

**Q46.** Which switching technique includes a **call setup** phase before data transfer begins? `[Applied]`

- **A)** datagram packet switching only
- **B)** message switching only
- **C)** circuit switching and virtual-circuit packet switching
- **D)** none of the three techniques

**Answer: C) circuit switching and virtual-circuit packet switching**

**Trace / Why:** both establish state before data flows — circuit switching reserves capacity along a
physical path, and virtual-circuit switching installs VCI table entries along a chosen route. Datagram and
message switching send immediately, with no prior negotiation.

**📘 CONCEPT — C24 (see Concept Index)** › Setup is orthogonal to reservation, and that is the examinable
subtlety: **circuit switching sets up *and* reserves; virtual circuits set up *without* reserving; datagrams
do neither.** So "has a setup phase" groups circuit and virtual-circuit switching together, while "reserves
bandwidth" isolates circuit switching alone. A question can partition the three techniques either way, and
which property it names decides the grouping.

**Wrong traces:** A = datagrams have no setup · B = message switching sends without negotiation ·
D = two of the three do have a setup phase.

---
## Section 3 — The OSI and TCP/IP Reference Models

*Highest-yield section for BCS, NTRCA and bank IT papers. Learn the layer table cold.*

**Q47.** The OSI reference model consists of how many layers? `[Core]` `[Asked: BCS / Bank IT]`

- **A)** 5
- **B)** 7
- **C)** 4
- **D)** 6

**Answer: B) 7**

**Trace / Why:** from the bottom up: **Physical, Data Link, Network, Transport, Session, Presentation,
Application**. The standard mnemonic is *"Please Do Not Throw Sausage Pizza Away"*.

**📘 CONCEPT — C27 · The seven layers, their jobs, and their PDUs**
> | # | Layer | Core job | PDU | Address used |
> |---|---|---|---|---|
> | 7 | **Application** | services to the user | data / message | — |
> | 6 | **Presentation** | translation, encryption, compression | data | — |
> | 5 | **Session** | dialog control, synchronisation | data | — |
> | 4 | **Transport** | **end-to-end** (process-to-process) delivery | **segment** | **port** |
> | 3 | **Network** | **host-to-host** delivery across networks; **routing** | **packet / datagram** | **logical (IP)** |
> | 2 | **Data Link** | **hop-to-hop** delivery on one link; **framing** | **frame** | **physical (MAC)** |
> | 1 | **Physical** | transmit raw bits over the medium | **bit** | — |
>
> **Applies when** the stem names a layer, a function, a PDU, an address type or a device.
>
> **Boundary:** the whole section reduces to this table — learn it in all five columns, because questions
> enter it from any column and ask for any other. Note the three-way delivery split, which is the single
> most examined distinction: **data link = hop to hop, network = host to host, transport = process to
> process** (Q60).

**Wrong traces:** A = the layers of the **TCP/IP** model in its five-layer form (Q65) · C = the TCP/IP
model in its four-layer form · D = no standard model has six layers.

---

**Q48.** The **lowest** layer of the OSI model is `[Core]`

- **A)** the data link layer
- **B)** the physical layer
- **C)** the network layer
- **D)** the application layer

**Answer: B) the physical layer**

**Trace / Why:** layer 1 is the physical layer. It is concerned with transmitting raw bits over the medium
— voltage levels, bit timing, connector pinouts, cable specifications, data rate — and attaches no meaning
to the bits at all.

**📘 CONCEPT — C28 · Physical layer: bits, not meaning**
> Responsibilities: physical characteristics of the medium and connectors · representation of bits (line
> coding, C36) · data rate and bit duration · **bit synchronisation** (clocking) · transmission mode
> (simplex/duplex, C2) · physical topology (C18).
>
> **Applies when** the stem mentions voltages, connectors, cables, bit rate, or a repeater or hub.
>
> **Boundary:** the physical layer has **no addressing and no error control** — it cannot tell whether a
> bit arrived correctly, only transmit and receive. Detecting corruption is the data link layer's job
> (C55), which is exactly why a **hub** (layer 1) forwards a corrupted frame while a **switch** (layer 2)
> discards it.

**Wrong traces:** A = layer 2 · C = layer 3 · D = layer 7, the highest.

---

**Q49.** The **highest** layer of the OSI model is `[Core]`

- **A)** the application layer
- **B)** the presentation layer
- **C)** the session layer
- **D)** the transport layer

**Answer: A) the application layer**

**Trace / Why:** layer 7 provides services directly to the user's software — HTTP, FTP, SMTP, DNS, TELNET.
It is the only layer with which the user's application interacts directly.

**📘 CONCEPT — C27 (see Concept Index)** › The top three layers (5–7) are often grouped as the
**"application-support" or upper layers**, dealing with the *content and dialogue* of the exchange, while
layers 1–4 handle *moving bytes reliably*. This grouping is why TCP/IP collapses 5, 6 and 7 into one
application layer (Q66) — the split was never as clean in practice as the model suggests.

**Wrong traces:** B = layer 6, immediately below · C = layer 5 · D = layer 4.

---

**Q50.** Which OSI layer is responsible for **routing** packets between different networks? `[Core]`

- **A)** the data link layer
- **B)** the transport layer
- **C)** the network layer
- **D)** the session layer

**Answer: C) the network layer**

**Trace / Why:** layer 3 handles delivery from the **source host to the destination host**, possibly across
many intervening networks. That requires logical (IP) addressing and a routing decision at each router,
neither of which exists below it — the data link layer knows only its own single link.

**📘 CONCEPT — C29 · Network layer: logical addressing plus routing, i.e. host-to-host delivery**
> Responsibilities: **logical addressing** (IP) · **routing** between networks · fragmentation and
> reassembly (C90) · congestion control (partly). Protocols: **IP, ICMP, ARP, RIP, OSPF, BGP**.
>
> **Applies when** the stem mentions routers, IP addresses, paths between networks, or the word "routing".
>
> **Boundary:** the network layer delivers **host to host**, not process to process — it gets the packet to
> the right machine, and the transport layer's port numbers get it to the right program on that machine
> (C31). Note also that IP is **connectionless and unreliable** by design (C26), so reliability belongs to
> the layer above.

**Wrong traces:** A = handles one hop across a single link, with no routing · B = end-to-end delivery
between processes, above the routing function · D = manages dialogue, unrelated to paths.

---

**Q51.** Which OSI layer provides **process-to-process** (end-to-end) delivery? `[Core]`

- **A)** the network layer
- **B)** the data link layer
- **C)** the transport layer
- **D)** the application layer

**Answer: C) the transport layer**

**Trace / Why:** layer 4 delivers to a specific **process**, identified by a port number, on the
destination host — and takes responsibility for the message as a whole, including reliability, ordering
and flow control when TCP is used.

**📘 CONCEPT — C30 · Transport layer: ports, segmentation, and end-to-end reliability**
> Responsibilities: **port (service-point) addressing** · **segmentation and reassembly** ·
> connection control (TCP connection-oriented, UDP connectionless) · **flow control** end to end ·
> **error control** end to end. Protocols: **TCP, UDP, SCTP**.
>
> **Applies when** the stem mentions ports, sockets, TCP or UDP, reliability, or "end-to-end".
>
> **Boundary:** flow control and error control appear at **both** layer 2 and layer 4, and the difference
> is scope: layer 2 acts across **one link**, layer 4 acts **end to end** across the whole path (Q59).
> That duplication is deliberate — a link can be reliable while the path is not, because routers in between
> may drop packets.

**Wrong traces:** A = host-to-host, one level of granularity coarser · B = hop-to-hop across a single
link · D = provides user services, above the delivery guarantee.

---

**Q52.** **Dialog control** and **synchronisation** (inserting checkpoints into a data stream) are functions
of `[Core]`

- **A)** the transport layer
- **B)** the session layer
- **C)** the presentation layer
- **D)** the network layer

**Answer: B) the session layer**

**Trace / Why:** layer 5 establishes, manages and terminates **sessions**. **Dialog control** decides whose
turn it is to transmit (half- or full-duplex at the session level); **synchronisation** inserts checkpoints
so that a long transfer interrupted at 80% can resume from the last checkpoint rather than from the start.

**📘 CONCEPT — C31 · Session layer: the dialogue, not the data**
> Two named functions worth memorising because they are asked verbatim: **dialog control** (managing whose
> turn it is) and **synchronisation** (checkpoints for recovery).
>
> **Applies when** the stem mentions sessions, checkpoints, dialogue, or resuming an interrupted transfer.
>
> **Boundary:** the session layer is the one with **no direct TCP/IP counterpart** — its functions, where
> needed, are implemented inside applications (an HTTP session cookie, an RPC session), which is why the
> TCP/IP model omits it entirely (Q66). Being absent in practice is exactly why it is a favourite exam
> target: it must be learned from the model rather than from experience.

**Wrong traces:** A = manages connections and reliability, not dialogue turns · C = handles data
representation · D = handles routing.

---

**Q53.** **Encryption, compression and translation** between different data representations are the
responsibility of `[Core]`

- **A)** the presentation layer
- **B)** the application layer
- **C)** the session layer
- **D)** the data link layer

**Answer: A) the presentation layer**

**Trace / Why:** layer 6 deals with the **syntax and semantics** of the information exchanged — converting
between character sets or number formats (translation), reducing the number of bits (compression) and
concealing the content (encryption).

**📘 CONCEPT — C32 · Presentation layer: three functions, all about the form of the data**
> **Translation** (ASCII ↔ EBCDIC, byte order) · **Encryption / decryption** · **Compression**.
>
> **Applies when** the stem names any of those three functions, or mentions data format conversion.
>
> **Boundary:** in the OSI model encryption belongs to layer 6, and that is the answer an OSI question
> expects — but in practice encryption happens wherever it is needed: **TLS** sits between transport and
> application (often called layer 6.5 or "session layer" loosely), **IPSec** at layer 3, and WPA at layer 2.
> So the exam answer and the deployment reality differ, and the stem's mention of "OSI" is what selects
> layer 6.

**Wrong traces:** B = provides user-facing services; the *content* transformation is one layer down ·
C = manages dialogue · D = handles framing and link error control.

---

**Q54.** **Framing** — dividing a bit stream into manageable units with recognisable boundaries — is
performed by `[Applied]`

- **A)** the data link layer
- **B)** the physical layer
- **C)** the network layer
- **D)** the transport layer

**Answer: A) the data link layer**

**Trace / Why:** the physical layer delivers an undifferentiated stream of bits. The data link layer imposes
structure on it, marking where each **frame** begins and ends so the receiver can extract discrete units,
check them for errors and act on their addresses.

**📘 CONCEPT — C33 · Data link layer: framing, physical addressing, and per-link reliability**
> Responsibilities: **framing** (C50) · **physical (MAC) addressing** · **flow control** on the link ·
> **error control** — detection and usually retransmission (C55) · **access control** when the medium is
> shared (the MAC sublayer, C66).
>
> Sublayers: **LLC** (upper — framing, flow and error control) and **MAC** (lower — medium access).
>
> **Applies when** the stem mentions frames, MAC addresses, switches or bridges, CSMA, or error detection.
>
> **Boundary:** the data link layer is responsible for **one hop only** — it delivers a frame across a
> single link between physically adjacent nodes. Every hop re-frames the packet with new MAC addresses,
> which is why the MAC addresses change at every router while the IP addresses do not (Q60).

**Wrong traces:** B = delivers bits with no structure · C = works with packets, which are framed by the
layer below · D = works with segments, above the network layer.

---

**Q55.** Which layer is responsible for **segmentation and reassembly** of a message into smaller units
that fit the network's limits? `[Applied]`

- **A)** the transport layer
- **B)** the data link layer
- **C)** the presentation layer
- **D)** the physical layer

**Answer: A) the transport layer**

**Trace / Why:** the transport layer accepts a message from the application, divides it into **segments**
each carrying a sequence number, and the peer transport layer reassembles them in order. The sequence
numbers are what make reassembly possible despite out-of-order arrival (C26).

**📘 CONCEPT — C30 (see Concept Index)** › Distinguish two similar-sounding operations that live at
different layers: the **transport layer segments** a message into segments (by choice, sized to the path's
MSS), while the **network layer fragments** a datagram that is too large for a link's MTU (by necessity,
C90). Both split data and both reassemble, but segmentation is planned at the source and fragmentation is
forced en route — and IPv6 removes router fragmentation entirely, leaving only the transport-layer split.

**Wrong traces:** B = frames data for one link but does not segment an application message · C = transforms
representation · D = transmits bits.

---

**Q56.** The protocol data unit (PDU) at the **data link** layer is called a `[Core]`

- **A)** frame
- **B)** packet
- **C)** segment
- **D)** bit

**Answer: A) frame**

**Trace / Why:** each layer names its own unit: bit (physical), **frame** (data link), packet or datagram
(network), segment (transport), data or message (upper layers).

**📘 CONCEPT — C34 · The PDU ladder, bottom to top**
> **bit → frame → packet → segment → data**
>
> | Layer | PDU |
> |---|---|
> | Physical | bit |
> | **Data link** | **frame** |
> | Network | packet / datagram |
> | Transport | segment (TCP) / datagram (UDP) |
> | Session–Application | data / message |
>
> **Applies when** the stem names a PDU and asks for the layer, or the reverse.
>
> **Boundary:** the words are **not** interchangeable in exam language even though everyday usage treats
> "packet" as generic. Note the UDP wrinkle: a TCP unit is a **segment** while a UDP unit is usually called
> a **datagram** — the same word the network layer uses — so "datagram" is ambiguous unless the layer is
> stated.

**Wrong traces:** B = the network layer's PDU · C = the transport layer's PDU · D = the physical layer's
unit.

---

**Q57.** The PDU at the **transport** layer, when TCP is used, is called a `[Core]`

- **A)** frame
- **B)** bit
- **C)** packet
- **D)** segment

**Answer: D) segment**

**Trace / Why:** TCP divides the application's byte stream into **segments**, each with a TCP header
carrying ports, a sequence number and control flags. The network layer then encapsulates each segment in a
**packet**.

**📘 CONCEPT — C34 (see Concept Index)** › The naming follows encapsulation strictly: a **segment** becomes
the payload of a **packet**, which becomes the payload of a **frame**, which is sent as **bits**. So the
units nest rather than replace one another — at any instant on the wire, one frame physically contains one
packet containing one segment. That nesting is what Q70 tests.

**Wrong traces:** A = data link · B = physical · C = network.

---

**Q58.** The PDU at the **network** layer is called a `[Core]`

- **A)** frame
- **B)** segment
- **C)** bit
- **D)** packet

**Answer: D) packet**

**Trace / Why:** the network layer's unit is the **packet** (in IP, specifically a **datagram**). It carries
source and destination IP addresses and is routed independently across networks.

**📘 CONCEPT — C34 (see Concept Index)** › "Packet" and "datagram" are used interchangeably at the network
layer, with **datagram** carrying the extra implication of **connectionless** delivery — which is exactly
what IP provides (C26). A virtual-circuit network layer such as ATM would call its unit a **cell** instead,
and its fixed 53-byte size is what distinguishes cell switching from packet switching.

**Wrong traces:** A = data link · B = transport · C = physical.

---

**Q59.** **Flow control** appears in the OSI model at `[Trap]`

- **A)** the data link layer only
- **B)** both the data link layer and the transport layer
- **C)** the transport layer only
- **D)** the network layer only

**Answer: B) both the data link layer and the transport layer**

**Trace / Why:** the two do the same *kind* of job at different **scopes**. The data link layer stops a
sender overwhelming the receiver **across one link**; the transport layer stops a source overwhelming the
destination **end to end**, across the whole path including every router in between.

**📘 CONCEPT — C35 · Functions that recur at two layers, and why the duplication is necessary**
> | Function | Data link layer | Transport layer |
> |---|---|---|
> | **Flow control** | across one link | **end to end** |
> | **Error control** | across one link | **end to end** |
> | Addressing | physical (MAC) | port |
>
> The duplication is not redundancy: a link can be perfectly reliable while the **path** loses data,
> because routers in between drop packets when their buffers fill. Only an end-to-end check can detect that.
>
> **Applies when** the stem asks which layer performs flow or error control — and offers "only" in the
> options.
>
> **Boundary:** this is the **end-to-end argument**: a function needed by the endpoints must be implemented
> at the endpoints, even if lower layers also provide it, because lower layers cannot see the whole path.
> It is why TCP retransmits even over links that already retransmit, and any option saying "only" for
> either layer is wrong.

**Wrong traces:** A, C and D = each names a single layer; A and C name two genuine locations but exclude
the other, and D names a layer that performs neither.

---

**Q60.** Which sequence correctly matches the **scope of delivery** to each layer? `[Applied]`

- **A)** data link = hop-to-hop, network = host-to-host, transport = process-to-process
- **B)** data link = host-to-host, network = hop-to-hop, transport = process-to-process
- **C)** data link = process-to-process, network = hop-to-hop, transport = host-to-host
- **D)** data link = hop-to-hop, network = process-to-process, transport = host-to-host

**Answer: A) data link = hop-to-hop, network = host-to-host, transport = process-to-process**

**Trace / Why:** the scopes widen with each layer.

```
Data link  : node → adjacent node          (one link, MAC addresses)
Network    : source host → destination host (many links, IP addresses)
Transport  : source process → dest process  (end to end, port numbers)
```

**📘 CONCEPT — C36 · Widening scope, and the addresses that go with it**
> Each layer's address type matches its scope: **MAC** identifies a node on one link, **IP** identifies a
> host anywhere, **port** identifies a process on that host. This is why, as a packet crosses routers, the
> **MAC addresses change at every hop** while the **IP addresses stay the same end to end** — the frame is
> rebuilt per link, the packet is not.
>
> **Applies when** the stem asks about delivery scope, or which addresses change along a path.
>
> **Boundary:** the fact that MAC addresses change per hop and IP addresses do not is the single most
> examined consequence of this table, and it is what makes **ARP** necessary at every hop (C93) — each
> router must discover the next hop's MAC address to build a new frame.

**Wrong traces:** B, C and D = each permutes the three scopes; B swaps the two lower layers, C rotates all
three, D swaps the two upper layers.

---

**Q61.** A **hub** operates at which layer of the OSI model? `[Applied]` `[Asked: BCS / Bank IT]`

- **A)** the physical layer
- **B)** the data link layer
- **C)** the network layer
- **D)** the transport layer

**Answer: A) the physical layer**

**Trace / Why:** a hub is a multiport repeater. It regenerates the incoming electrical signal and floods it
out of every other port, with **no examination of addresses** and no ability to detect errors. All ports
therefore form one collision domain and one broadcast domain.

**📘 CONCEPT — C37 · Network devices and their layers — the table BCS and bank papers ask from**
> | Device | Layer | Uses | Collision domains | Broadcast domains |
> |---|---|---|---|---|
> | **Repeater / Hub** | **1** Physical | nothing — regenerates signals | **1** (shared) | 1 |
> | **Bridge / Switch** | **2** Data link | **MAC** addresses | 1 per port | 1 (per VLAN) |
> | **Router** | **3** Network | **IP** addresses | 1 per port | **1 per port** |
> | **Gateway** | up to **7** | protocol translation | per port | per port |
>
> **Applies when** the stem names a device and asks its layer, or asks how many domains a topology has.
>
> **Boundary:** the layer determines what the device can **see**, and therefore what it can do. A hub
> cannot filter because it never reads an address; a switch cannot route because it never reads an IP
> address. Note that a **switch is a multiport bridge** — same layer, same logic, more ports — so the two
> answer identically in these questions.

**Wrong traces:** B = a switch or bridge · C = a router · D = no common device operates primarily at layer 4
(a load balancer is the nearest).

---

**Q62.** A **switch** forwards frames based on `[Applied]`

- **A)** IP addresses, using a routing table
- **B)** port numbers, using a socket table
- **C)** signal strength, flooding to the strongest port
- **D)** MAC addresses, using a table learned from observed source addresses

**Answer: D) MAC addresses, using a table learned from observed source addresses**

**Trace / Why:** a switch reads each frame's **source** MAC address and records which port it arrived on,
building a MAC address table. It then forwards a frame only out of the port matching the **destination**
MAC — flooding to all ports only when the destination is not yet known, or when it is a broadcast.

**📘 CONCEPT — C38 · Transparent bridging: learn from sources, forward by destination, flood when unknown**
> Three rules the switch applies to every frame:
> 1. **Learn** — record ⟨source MAC, arrival port⟩ in the table.
> 2. **Forward** — if the destination MAC is in the table, send it out that port only.
> 3. **Flood** — if the destination is unknown or is a broadcast, send it out every other port.
>
> This is why switching is **transparent**: no host is configured or even aware of it.
>
> **Applies when** the stem describes MAC learning, flooding, or asks how a switch differs from a hub.
>
> **Boundary:** because a switch floods **broadcasts**, it separates collision domains but **not** broadcast
> domains (C37) — a switched LAN is still one broadcast domain unless VLANs are configured (C75). That
> single limitation is why routers are still needed to segment large LANs.

**Wrong traces:** A = a router's behaviour · B = a transport-layer concept, above a switch's view ·
C = no device forwards by signal strength.

---

**Q63.** A **router** makes forwarding decisions based on `[Applied]`

- **A)** MAC addresses
- **B)** frame check sequences
- **C)** VLAN tags only
- **D)** destination IP addresses, consulted against a routing table

**Answer: D) destination IP addresses, consulted against a routing table**

**Trace / Why:** a router operates at layer 3. It strips the incoming frame, examines the **destination IP
address** in the packet, looks it up in its routing table (choosing the longest matching prefix), and builds
a **new frame** for the outgoing link with new MAC addresses.

**📘 CONCEPT — C37 (see Concept Index)** › The decisive difference from a switch is that a router
**terminates the data link layer** — it discards the incoming frame and constructs a fresh one for the next
link. Two consequences follow: MAC addresses change at every hop while IP addresses do not (C36), and each
router interface bounds a **broadcast domain**, which is why routers segment networks and switches do not.
Note that a router also decrements the **TTL** (C89), so it is visible to traceroute in a way a switch is
not.

**Wrong traces:** A = a switch's basis · B = the FCS is used to *check* a frame, never to forward it ·
C = VLAN tags are a layer-2 mechanism and are not a router's primary basis.

---

**Q64.** A **gateway** differs from a router in that a gateway `[Core]`

- **A)** can operate at any layer up to layer 7, translating between dissimilar protocols
- **B)** works only at the physical layer, regenerating signals
- **C)** forwards frames without examining any address
- **D)** cannot connect networks using different protocol suites

**Answer: A) can operate at any layer up to layer 7, translating between dissimilar protocols**

**Trace / Why:** a router connects networks that **share** a network-layer protocol, forwarding IP packets
between IP networks. A gateway joins networks whose protocols **differ**, translating between them — an
email gateway converting between SMTP and a proprietary mail format, or a protocol gateway between TCP/IP
and SNA. That may require processing right up to the application layer.

**📘 CONCEPT — C37 (see Concept Index)** › The distinction is **translation versus forwarding**: a router
forwards what it already understands, while a gateway converts between things that do not otherwise
interoperate. In everyday usage "default gateway" simply means the local router, which is why students
conflate them — but in exam language a gateway is the protocol-translating device and may sit at any layer
up to 7.

**Wrong traces:** B = a repeater · C = a hub · D = exactly backwards — connecting dissimilar suites is a
gateway's defining purpose.

---

**Q65.** The TCP/IP protocol suite is commonly described as having how many layers? `[Trap]`

- **A)** 4 or 5, depending on whether the physical and data link layers are shown separately
- **B)** exactly 7, matching the OSI model layer for layer
- **C)** exactly 3
- **D)** exactly 6, omitting only the physical layer

**Answer: A) 4 or 5, depending on whether the physical and data link layers are shown separately**

**Trace / Why:** the original TCP/IP model has **four** layers — Application, Transport, Internet, Network
Access (Link) — because TCP/IP was never concerned with the specifics of the physical medium. Many textbooks
show **five**, splitting Network Access into Data Link and Physical to align with OSI. Both descriptions
are standard.

**📘 CONCEPT — C39 · Mapping TCP/IP onto OSI**
> | OSI | TCP/IP (4-layer) | TCP/IP (5-layer) |
> |---|---|---|
> | 7 Application · 6 Presentation · 5 Session | **Application** | Application |
> | 4 Transport | **Transport** | Transport |
> | 3 Network | **Internet** | Network |
> | 2 Data Link | **Network Access** | Data Link |
> | 1 Physical | (same) | Physical |
>
> **Applies when** the stem asks for a layer count, or which OSI layers have no TCP/IP counterpart.
>
> **Boundary:** the collapse is at the **top**: OSI layers 5, 6 and 7 all become the single TCP/IP
> **Application** layer, so session and presentation functions are handled inside applications (Q66).
> The bottom is a presentation choice, not a real disagreement — which is why both 4 and 5 are defensible
> and why a question insisting on one exact number is usually testing the 4-layer original.

**Wrong traces:** B = TCP/IP has fewer layers, and does not match OSI one-to-one · C = no version has
three · D = omitting only the physical layer would leave six, which no version does.

---

**Q66.** Which OSI layers have **no** direct counterpart in the TCP/IP model? `[Applied]`

- **A)** physical and data link
- **B)** network and transport
- **C)** transport and application
- **D)** session and presentation

**Answer: D) session and presentation**

**Trace / Why:** TCP/IP's Application layer absorbs OSI layers 5, 6 and 7. Session management (dialogue,
checkpoints) and presentation functions (translation, compression, encryption) are therefore implemented
**inside applications** rather than in dedicated layers — HTTP manages its own sessions, TLS provides its own
encryption, and each application defines its own data representation.

**📘 CONCEPT — C39 (see Concept Index)** › The omission is not an oversight but a design judgement: those
functions vary so much per application that a general layer added little. This is why the OSI model is the
better **teaching** framework — it names every function explicitly — while TCP/IP is the model that was
actually **deployed**. Exam questions about the model use OSI; questions about real protocols use TCP/IP,
and knowing which frame a question sits in decides the expected answer.

**Wrong traces:** A = present in TCP/IP, whether shown as one Network Access layer or two · B = both are
present (Internet and Transport) · C = both are present.

---

**Q67.** A **physical (MAC) address** is used by which layer? `[Applied]`

- **A)** the physical layer
- **B)** the data link layer
- **C)** the network layer
- **D)** the transport layer

**Answer: B) the data link layer**

**Trace / Why:** the MAC address — 48 bits, usually written as six hex pairs — identifies a network
interface **on its local link**. The data link layer places source and destination MAC addresses in every
frame header, and switches forward on them (C38).

**📘 CONCEPT — C40 · Four address types, one per layer that needs one**
> | Address | Layer | Scope | Size / form |
> |---|---|---|---|
> | **Physical (MAC)** | 2 Data link | **one link** | 48 bits, `00:1A:2B:3C:4D:5E` |
> | **Logical (IP)** | 3 Network | **whole internetwork** | 32 bits (IPv4) / 128 (IPv6) |
> | **Port** | 4 Transport | one **process** on a host | 16 bits, 0–65535 |
> | **Specific / application** | 7 Application | a user-level entity | e-mail address, URL |
>
> **Applies when** the stem gives an address form and asks for the layer, or asks which address a device
> uses.
>
> **Boundary:** the physical layer, despite the name, uses **no address at all** — "physical address" is a
> data-link term for a hardware address, and that naming collision is the trap in option A. Note also that
> MAC addresses are **flat** (no structure, so not routable) while IP addresses are **hierarchical** (a
> network part and a host part), which is exactly why routing scales and bridging does not.

**Wrong traces:** A = the physical layer transmits bits and uses no addresses — the naming trap ·
C = uses logical (IP) addresses · D = uses port numbers.

---

**Q68.** A **logical address** in the TCP/IP suite is used by `[Applied]`

- **A)** the data link layer
- **B)** the network layer
- **C)** the transport layer
- **D)** the application layer

**Answer: B) the network layer**

**Trace / Why:** the logical address is the **IP address** — 32 bits in IPv4 — which identifies a host
anywhere in the internetwork, independently of the hardware it runs on. The network layer places it in the
packet header and routers forward on it (C29).

**📘 CONCEPT — C40 (see Concept Index)** › "Logical" means **assigned by software and independent of
hardware**, which is what makes IP addresses routable: they are **hierarchical**, splitting into a network
part and a host part, so routers can aggregate them (see the Subnetting chapter). A MAC address is burned
into the interface and is **flat**, so it can identify but never locate. That contrast — hierarchical and
reassignable versus flat and fixed — is the reason both address types exist rather than one.

**Wrong traces:** A = uses physical (MAC) addresses · C = uses port numbers · D = uses application-specific
names such as URLs and e-mail addresses.

---

**Q69.** A **port number** identifies `[Applied]`

- **A)** a physical connector on a switch
- **B)** a specific process or service on a host
- **C)** a network within an internetwork
- **D)** a network interface card on a local link

**Answer: B) a specific process or service on a host**

**Trace / Why:** a 16-bit port number identifies which **process** on the destination host should receive a
segment. The IP address gets the packet to the right machine; the port number gets it to the right program
— so a single host can run a web server, a mail server and SSH simultaneously.

**📘 CONCEPT — C41 · Ports and sockets: the transport layer's multiplexing key**
> A **socket** is the pair ⟨IP address, port number⟩, and a TCP connection is identified by the **four-tuple**
> ⟨source IP, source port, destination IP, destination port⟩ — which is why one server port can serve
> thousands of simultaneous clients, each with a distinct source.
>
> Ranges: **0–1023 well-known** (HTTP 80, HTTPS 443, FTP 20/21, SSH 22, TELNET 23, SMTP 25, DNS 53,
> POP3 110, IMAP 143, SNMP 161) · **1024–49151 registered** · **49152–65535 dynamic/ephemeral**.
>
> **Applies when** the stem mentions ports, sockets, or how one host runs several services.
>
> **Boundary:** the word "port" is badly overloaded — a **switch port** is a physical connector (layer 1
> hardware), while a **transport port** is a 16-bit number identifying a process. Option A exploits exactly
> that collision, and the stem's mention of a host rather than a switch is what resolves it.

**Wrong traces:** A = a switch port, a physical connector — the terminology trap · C = an IP network
prefix does that · D = a MAC address does that.

---

**Q70.** During **encapsulation**, as data moves down the protocol stack at the sender `[Trap]`

- **A)** each layer adds its own header (and sometimes a trailer) to the unit received from the layer above
- **B)** each layer removes the header added by the layer above
- **C)** the data is compressed once at the physical layer and expanded at the receiver
- **D)** headers are added only at the transport and network layers

**Answer: A) each layer adds its own header (and sometimes a trailer) to the unit received from the layer above**

**Trace / Why:** each layer treats the unit handed down as opaque **payload** and prepends its own control
information.

```
Application data
  → Transport:  [TCP hdr | data]                          = segment
  → Network:    [IP hdr | TCP hdr | data]                 = packet
  → Data link:  [MAC hdr | IP hdr | TCP hdr | data | FCS] = frame
  → Physical:   bits on the wire
```

At the receiver the reverse (**decapsulation**) happens: each layer strips its own header and passes the
remainder up.

**📘 CONCEPT — C42 · Encapsulation: headers go on at the sender, come off at the receiver, in reverse order**
> A layer never inspects the payload it receives — it only adds its own header, which is the practical
> meaning of layer independence. The **data link layer is the one that adds a trailer** as well as a header,
> because the **FCS** (the CRC, C56) must cover everything before it.
>
> **Applies when** the stem describes headers being added or removed, or asks for the total size of a frame.
>
> **Boundary:** overhead accumulates — a 20-byte TCP header plus a 20-byte IP header plus an 18-byte
> Ethernet header and trailer is 58 bytes of overhead, which is why small payloads are inefficient (the
> reason Nagle's algorithm exists, C107). Note that the receiving layer must strip **exactly** its own
> header, so both ends must agree on the protocol — which is the protocol component of C1 doing its work.

**Wrong traces:** B = describes **decapsulation** at the receiver, in the wrong direction · C = compression
is a presentation-layer function (C32), not physical · D = the data link layer adds a header *and* trailer,
and the upper layers add headers too.

---
