# Data Communication and Networking — MCQ Question Bank · Part 2
**Subject:** Computer Networks · **Target:** BCS Preliminary / Bank IT Officer / NTRCA / GATE / IBPS SO IT
**Questions:** 70 (Q71–Q140) · **Batch:** Part 2 of 3, written in the same run as Parts 1 and 3

> How to use: attempt the question first, then read the explanation. The wrong
> options matter more than the right one — that's what the examiner is testing.

**Continues Part 1 directly.** Part 1 (Q1–Q70) covered data communication fundamentals and performance,
network types and topologies, switching techniques, and the OSI and TCP/IP models. This file covers
**transmission media and signals, line coding, modulation, multiplexing, and the data link layer** —
framing, error detection and correction, and flow control including the sliding-window protocols that carry
the most GATE marks in this chapter. Concept ids continue from Part 1, starting at **C43**.

---

## Section 4 — Transmission Media

**Q71.** Which of the following is an example of **guided** transmission media? `[Core]`

- **A)** microwave
- **B)** infrared
- **C)** coaxial cable
- **D)** satellite radio

**Answer: C) coaxial cable**

**Trace / Why:** guided media provide a **physical conduit** that confines and directs the signal — twisted
pair, coaxial cable and optical fibre. Unguided media transmit through free space with no conduit: radio,
microwave and infrared.

**📘 CONCEPT — C43 · Guided vs unguided media, and the three of each worth knowing**
> | Guided (bounded, wired) | Unguided (unbounded, wireless) |
> |---|---|
> | Twisted pair (UTP, STP) | Radio waves |
> | Coaxial cable | Microwave |
> | **Optical fibre** | Infrared |
>
> **Applies when** the stem asks to classify a medium, or contrasts wired with wireless.
>
> **Boundary:** the classification is about whether a **physical path** exists, not about whether the signal
> is electrical or optical — fibre carries light and is still guided, because the glass confines it. Note
> that unguided media are inherently **broadcast** (anyone in range receives), which is why wireless needs
> encryption where wired links often do not.

**Wrong traces:** A, B and D = all unguided; each propagates through free space with no physical conduit.

---

**Q72.** **STP** cable differs from **UTP** cable in that STP `[Core]`

- **A)** uses a single solid conductor instead of twisted pairs
- **B)** adds a metallic foil or braid shield around the pairs to reduce interference
- **C)** carries light rather than electrical signals
- **D)** supports only half-duplex transmission

**Answer: B) adds a metallic foil or braid shield around the pairs to reduce interference**

**Trace / Why:** both are twisted pair. **STP** (Shielded Twisted Pair) wraps a conducting shield around the
pairs, which intercepts external electromagnetic noise and drains it to ground. That gives better noise
immunity at the cost of higher price, greater weight and more difficult installation — the shield must be
properly grounded or it can make matters worse.

**📘 CONCEPT — C44 · Twisted pair: the twist itself is the noise defence, and the shield is an extra**
> Two wires are twisted so that any external interference induces a **nearly equal** voltage in both. Since
> the receiver reads the **difference** between them, the common noise cancels. More twists per unit length
> means better cancellation — which is exactly what the category numbers rank.
>
> | Category | Rated for |
> |---|---|
> | Cat 3 | 10 Mbps (10BaseT), telephone |
> | Cat 5 | 100 Mbps |
> | **Cat 5e** | 1 Gbps |
> | **Cat 6 / 6a** | 1–10 Gbps |
> | Cat 7 | 10 Gbps, shielded |
>
> **Applies when** the stem names UTP, STP, a category number, or asks why wires are twisted.
>
> **Boundary:** **UTP dominates in practice** despite being less immune to noise — it is cheap, flexible and
> good enough with sufficient twisting, and structured cabling is overwhelmingly UTP. So "STP is better" is
> true only for noise immunity, and false for cost, handling and deployment share.

**Wrong traces:** A = a single conductor describes coax's core, not STP · C = optical fibre · D = shielding
has no bearing on duplex mode.

---

**Q73.** The wires in a twisted-pair cable are twisted around each other in order to `[Core]`

- **A)** increase the cable's tensile strength
- **B)** allow more pairs to fit in a given cable diameter
- **C)** make the cable easier to bend around corners
- **D)** ensure interference affects both wires equally, so it cancels at the receiver

**Answer: D) ensure interference affects both wires equally, so it cancels at the receiver**

**Trace / Why:** if the two wires ran parallel, one would sit closer to a noise source than the other and
pick up more interference — the difference between them would then include the noise. Twisting keeps their
average positions relative to any external source identical, so both pick up the **same** induced voltage
and the differential receiver subtracts it away.

**📘 CONCEPT — C44 (see Concept Index)** › This is **differential signalling** and it is why the twist rate
matters: higher frequencies need tighter twists for the cancellation to hold, which is the physical basis of
the category ladder. The same principle explains **crosstalk** (C48) — interference between adjacent pairs
inside one cable is reduced by giving each pair a *different* twist rate, so they do not couple
systematically.

**Wrong traces:** A, B and C = incidental mechanical properties, none of which is the electrical reason for
twisting.

---

**Q74.** Which cable category is the minimum normally specified for **1 Gbps** Ethernet? `[Core]`

- **A)** Cat 3
- **B)** Cat 4
- **C)** Cat 5
- **D)** Cat 5e

**Answer: D) Cat 5e**

**Trace / Why:** Cat 5 was rated for 100 Mbps. **Cat 5e** ("enhanced") tightened the crosstalk
specifications enough to carry 1000BaseT over 100 m, and Cat 6 raised the margin further. Cat 3 belongs to
10 Mbps Ethernet and telephony.

**📘 CONCEPT — C44 (see Concept Index)** › The rule to carry is that **higher categories mean tighter
crosstalk and attenuation specifications**, not different connectors or a different physical principle — all
of Cat 3 through Cat 6a use the same RJ-45 plug and the same 100 m limit for Ethernet. That is why an
installation can be upgraded in speed only by replacing cable, never by replacing connectors.

**Wrong traces:** A = 10 Mbps · B = 16 Mbps, Token Ring era · C = 100 Mbps, one step short.

---

**Q75.** A coaxial cable consists of `[Core]`

- **A)** two insulated copper wires twisted together
- **B)** a glass core surrounded by a glass cladding
- **C)** a central conductor surrounded by insulation, a braided outer conductor and a protective jacket
- **D)** eight colour-coded wires in four pairs

**Answer: C) a central conductor surrounded by insulation, a braided outer conductor and a protective jacket**

**Trace / Why:** the inner conductor carries the signal; the outer braided conductor acts as both the return
path and a **shield**; the two share a common axis, which is what "co-axial" means. That geometry gives coax
much higher bandwidth and better noise immunity than twisted pair, and it is why coax carried early
Ethernet (10Base5, 10Base2) and still carries cable television.

**📘 CONCEPT — C45 · The three guided media compared on the properties exams ask about**
> | | Twisted pair | Coaxial | Optical fibre |
> |---|---|---|---|
> | Signal | electrical | electrical | **light** |
> | Bandwidth | low–medium | medium–high | **very high** |
> | Attenuation | high | medium | **very low** |
> | EMI immunity | poor–fair | fair | **complete** |
> | Cost | lowest | medium | highest |
> | Security (tapping) | easy to tap | easy to tap | **hard to tap** |
>
> **Applies when** the stem asks which medium is best or worst on a named property.
>
> **Boundary:** fibre wins on every **technical** axis and loses only on **cost and handling** — it needs
> precise splicing, special connectors and cannot be bent sharply. That single trade explains the deployment
> pattern: fibre in backbones and between buildings, UTP to the desk.

**Wrong traces:** A = twisted pair · B = optical fibre · D = a UTP Ethernet cable.

---

**Q76.** Optical fibre transmits data by `[Core]`

- **A)** guiding light along a glass core by total internal reflection
- **B)** carrying electrical current through a copper core
- **C)** radiating radio waves along a glass waveguide
- **D)** using magnetic induction between adjacent strands

**Answer: A) guiding light along a glass core by total internal reflection**

**Trace / Why:** the **core** has a higher refractive index than the surrounding **cladding**. A light ray
striking the boundary at an angle beyond the critical angle is reflected entirely back into the core, so it
propagates along the fibre by repeated total internal reflection rather than escaping.

**📘 CONCEPT — C46 · Fibre construction and the two propagation modes**
> Structure: **core** (glass, higher index) → **cladding** (glass, lower index) → **buffer / jacket**
> (protection).
>
> | | Multimode | Single-mode |
> |---|---|---|
> | Core diameter | large (50–62.5 µm) | very small (8–10 µm) |
> | Light paths | **many** | essentially **one** |
> | Dispersion | higher — paths differ in length | very low |
> | Distance | shorter (up to ~2 km) | **very long** (tens of km) |
> | Source | LED | **laser** |
> | Cost | lower | higher |
>
> **Applies when** the stem mentions core diameter, modes, dispersion, LED versus laser, or reach.
>
> **Boundary:** multimode's limitation is **modal dispersion** — rays taking different paths arrive at
> different times, smearing the pulse and limiting distance and rate. Single-mode's narrow core admits
> essentially one path, removing that smearing, which is why long-haul and high-rate links are single-mode
> (Q77).

**Wrong traces:** B = copper conduction, not fibre · C = fibre carries light, not radio waves · D = no
inductive coupling is involved.

---

**Q77.** **Single-mode** fibre differs from multimode fibre in that single-mode fibre `[Trap]`

- **A)** has a larger core, allowing more light and therefore greater bandwidth
- **B)** has a very narrow core, so light follows essentially one path, giving far greater distance
- **C)** uses an LED source rather than a laser, reducing cost
- **D)** carries electrical as well as optical signals

**Answer: B) has a very narrow core, so light follows essentially one path, giving far greater distance**

**Trace / Why:** an 8–10 µm core is narrow enough that only one propagation mode is supported. With a single
path there is no modal dispersion, so pulses stay sharp over tens of kilometres. Multimode's 50–62.5 µm core
admits many paths of differing length, which smears the pulse and caps the usable distance.

**📘 CONCEPT — C46 (see Concept Index)** › The intuition that "bigger core = better" is exactly wrong, and
that inversion is the trap. A **larger** core is easier and cheaper to couple light into (hence LEDs and
lower cost for multimode) but admits more modes and therefore more dispersion. **Smaller is better for
performance, worse for cost** — the same shape of trade as UTP versus STP (C44), where the cheaper option is
technically inferior and wins on economics anyway.

**Wrong traces:** A = the core is **smaller**, and the reasoning is inverted · C = single-mode requires a
**laser**; LEDs belong to multimode · D = fibre carries light only.

---

**Q78.** Which is a genuine advantage of optical fibre over copper media? `[Applied]`

- **A)** it is cheaper to install and terminate
- **B)** it can be spliced with simple mechanical crimping tools
- **C)** it carries electrical power to remote devices along with data
- **D)** it is completely immune to electromagnetic interference and crosstalk

**Answer: D) it is completely immune to electromagnetic interference and crosstalk**

**Trace / Why:** fibre carries **light**, and light is unaffected by electromagnetic fields. So a fibre can
run beside a power cable, through a factory full of motors, or between buildings with different ground
potentials — all situations that corrupt or damage copper. It also cannot be tapped without detectable loss
of light, which makes it more secure.

**📘 CONCEPT — C45 (see Concept Index)** › Fibre's four real advantages are **immunity to EMI**, **enormous
bandwidth**, **very low attenuation** (repeaters tens of kilometres apart rather than hundreds of metres) and
**security**. Its real disadvantages are **cost**, **fragility** and **difficult splicing** — options A and B
invert precisely those, which is how this question is always built. A further practical point: fibre provides
**galvanic isolation**, so it is the correct choice between buildings where lightning-induced surges would
destroy copper.

**Wrong traces:** A = fibre is more expensive to install and terminate · B = fibre needs fusion splicing or
precision connectors, not crimping · C = fibre carries no power — that is Power over Ethernet on copper.

---

**Q79.** Which is a genuine **disadvantage** of optical fibre? `[Trap]`

- **A)** it is highly susceptible to electromagnetic interference
- **B)** it offers lower bandwidth than coaxial cable
- **C)** it attenuates signals more rapidly than twisted pair
- **D)** installation and splicing require specialised equipment and skills, raising cost

**Answer: D) installation and splicing require specialised equipment and skills, raising cost**

**Trace / Why:** joining two fibres means aligning glass cores a few microns across, which requires a fusion
splicer and a trained technician. Fibre is also more fragile than copper and cannot tolerate tight bends. All
of fibre's genuine drawbacks are **practical and economic**, never electrical.

**📘 CONCEPT — C45 (see Concept Index)** › The pattern worth internalising: fibre's weaknesses are all about
**handling and money**, so any option claiming a *performance* deficiency is fabricated. Options A, B and C
each assert the exact reverse of one of fibre's strengths, which means this question can be answered by
elimination alone even without knowing about splicing. That symmetry between Q78 and Q79 is deliberate — the
same table read in both directions.

**Wrong traces:** A = fibre is completely **immune** to EMI · B = fibre has **far higher** bandwidth ·
C = fibre has **much lower** attenuation.

---

**Q80.** Which transmission medium is **completely** unaffected by electromagnetic interference? `[Core]`

- **A)** unshielded twisted pair
- **B)** shielded twisted pair
- **C)** coaxial cable
- **D)** optical fibre

**Answer: D) optical fibre**

**Trace / Why:** the other three carry **electrical** signals, so a changing magnetic field induces unwanted
currents in them. Shielding (STP, coax) reduces the effect but never eliminates it. Fibre carries photons,
which electromagnetic fields do not deflect — the immunity is complete rather than merely improved.

**📘 CONCEPT — C45 (see Concept Index)** › Note the ordering the distractors encode: **UTP < STP < coax <
fibre** in noise immunity, with the first three differing only in *degree* and fibre differing in *kind*.
Questions using the word "completely", "immune" or "entirely" want fibre; questions asking merely for
"better" immunity among copper options want the shielded one.

**Wrong traces:** A = the least immune of the four · B = shielding reduces but does not eliminate ·
C = the braid shields well, but the signal remains electrical.

---

**Q81.** Which of the following is a form of **unguided** transmission? `[Core]`

- **A)** microwave transmission
- **B)** shielded twisted pair
- **C)** multimode optical fibre
- **D)** coaxial cable

**Answer: A) microwave transmission**

**Trace / Why:** microwave signals travel through the **atmosphere** with no physical conduit, so they are
unguided (wireless). The other three all confine the signal within a cable.

**📘 CONCEPT — C47 · The three unguided bands and what each is used for**
> | Type | Frequency | Propagation | Typical use |
> |---|---|---|---|
> | **Radio** | 3 kHz – 1 GHz | **omnidirectional**, penetrates walls | AM/FM, TV, cordless, WiFi (lower end) |
> | **Microwave** | 1 – 300 GHz | **unidirectional, line of sight** | terrestrial links, satellite, WiFi, mobile |
> | **Infrared** | 300 GHz – 400 THz | line of sight, **blocked by walls** | remote controls, short-range links |
>
> **Applies when** the stem describes directionality, wall penetration, or line-of-sight requirements.
>
> **Boundary:** the decisive discriminator is **directionality and penetration**, which follow from
> frequency: low-frequency **radio is omnidirectional and passes through walls** (so no aiming is needed but
> interference and eavesdropping are easy), while **microwave and infrared need line of sight** (so they must
> be aimed, and infrared cannot leave a room — which is a security advantage as well as a limitation).

**Wrong traces:** B, C and D = all guided media with a physical conduit.

---

**Q82.** **Radio waves** are generally preferred over microwaves for broadcast applications because radio
waves `[Applied]`

- **A)** carry far more bandwidth per channel
- **B)** are unaffected by rain and atmospheric absorption
- **C)** can be focused into a narrow beam more easily
- **D)** propagate omnidirectionally and can penetrate walls, so receivers need no alignment

**Answer: D) propagate omnidirectionally and can penetrate walls, so receivers need no alignment**

**Trace / Why:** broadcasting means one transmitter reaching many uncoordinated receivers. Radio's lower
frequency makes it **omnidirectional** — the signal spreads in all directions — and able to pass through
buildings, so a receiver anywhere in range works without being aimed. That is exactly what AM/FM radio and
broadcast television require.

**📘 CONCEPT — C47 (see Concept Index)** › Omnidirectionality is simultaneously radio's **advantage** for
broadcast and its **disadvantage** for point-to-point links: the energy spreads everywhere, so most of it is
wasted, interference between users is likely, and anyone in range can listen. Microwave's narrow beam
reverses all three — efficient, less interference-prone, harder to intercept, but it must be aimed and needs
clear line of sight (Q83). Neither is better in the abstract; the application decides.

**Wrong traces:** A = microwaves carry **more** bandwidth, being higher frequency · B = radio is affected by
atmospheric conditions too, and rain fade specifically afflicts microwave · C = **microwaves** focus into
narrow beams; radio does not.

---

**Q83.** Terrestrial **microwave** links require repeater towers every few tens of kilometres because
`[Applied]`

- **A)** microwave signals attenuate to nothing within 10 metres
- **B)** microwave equipment must be recalibrated at short intervals
- **C)** microwaves cannot be modulated over longer distances
- **D)** microwaves travel in straight lines and need line of sight, which the Earth's curvature interrupts

**Answer: D) microwaves travel in straight lines and need line of sight, which the Earth's curvature interrupts**

**Trace / Why:** microwaves do not follow the Earth's curve or diffract appreciably around obstacles. Beyond
roughly 50 km the curvature of the Earth places the receiver below the horizon, so a tower is needed to
restore line of sight — which is why microwave relay chains are built on hilltops and tall masts.

**📘 CONCEPT — C47 (see Concept Index)** › Line-of-sight is the governing constraint for all microwave
deployment, and two consequences are examinable: tower **height** determines hop distance (raising the
antennas extends the horizon), and **satellite** links exist precisely to escape the problem — a
geostationary satellite at 36,000 km sees a third of the planet at once, trading the curvature limitation for
enormous propagation delay (Q85). Note also that microwave suffers **rain fade**, since water absorbs at
these frequencies.

**Wrong traces:** A = microwaves travel tens of kilometres easily · B and C = invented equipment and
modulation limitations.

---

**Q84.** **Infrared** transmission is unsuitable for communication between rooms because infrared `[Core]`

- **A)** requires a licence from the spectrum regulator
- **B)** interferes with radio and television reception
- **C)** offers too little bandwidth for any data application
- **D)** cannot penetrate solid walls

**Answer: D) cannot penetrate solid walls**

**Trace / Why:** at infrared frequencies, walls are opaque. That confines an infrared link to a single room —
a genuine limitation for general networking, but simultaneously an advantage: an infrared system in one room
cannot interfere with, or be eavesdropped from, the room next door, and it needs no licence because the signal
cannot escape.

**📘 CONCEPT — C47 (see Concept Index)** › The same physical property reads as a limitation or a feature
depending on the goal — **confinement** blocks room-to-room networking while providing free spatial reuse and
security. This is why infrared survives in remote controls and short-range device links rather than in LANs,
and why it needs no spectrum licence while radio and microwave bands are regulated.

**Wrong traces:** A = infrared is unlicensed, precisely because it does not escape the room · B = infrared
does not interfere with radio bands · C = infrared has ample bandwidth; range is the problem.

---

**Q85.** A geostationary satellite orbits at approximately 36,000 km. The one-way propagation delay to the
satellite and back down is therefore about `[Applied]`

- **A)** 24 ms
- **B)** 60 ms
- **C)** 120 ms
- **D)** 240 ms

**Answer: D) 240 ms**

**Trace / Why:** the signal must travel up and down — 72,000 km in total — at the speed of light in free
space.

```
distance = 2 × 36,000 km = 72,000 km = 7.2 × 10^7 m
Tp       = 7.2 × 10^7 / (3 × 10^8)
         = 0.24 s = 240 ms
```

A round trip (up-down-up-down) is therefore about **480 ms**, which is why satellite links feel
unresponsive and why interactive protocols perform badly over them.

**📘 CONCEPT — C48 · Satellite delay is the classic case of propagation dominating everything**
> The ~240 ms one-way figure is fixed by orbital geometry, so no amount of extra bandwidth improves it.
> Consequences worth carrying:
> - **stop-and-wait** collapses to near-zero efficiency (C60), since the sender waits ~480 ms per frame;
> - a very large **window** is needed to fill the pipe, which is a bandwidth-delay product argument (C7);
> - TCP's slow start takes many round trips to open up, so short transfers never reach full speed.
>
> **Applies when** the stem mentions a satellite, a geostationary orbit, or a very long delay.
>
> **Boundary:** the answer depends on whether the stem asks for **one way** (240 ms) or a **round trip**
> (480 ms) — a factor of two that papers exploit routinely. Low-earth-orbit constellations sit at ~500–1,200
> km and so have delays of a few milliseconds, which is the whole point of their design.

**Wrong traces:** A = uses 36,000 km only in one direction and mis-scales · B = a quarter of the correct
value · C = uses 36,000 km one way (120 ms), forgetting the return leg to Earth.

---

**Q86.** The connector normally used to terminate **UTP** cable in an Ethernet LAN is `[Core]`

- **A)** BNC
- **B)** RJ-45
- **C)** ST
- **D)** RJ-11

**Answer: B) RJ-45**

**Trace / Why:** RJ-45 is the eight-position modular plug that terminates the four pairs of a UTP Ethernet
cable. RJ-11 is its smaller six-position telephone cousin, BNC belongs to coaxial cable, and ST is a fibre
connector.

**📘 CONCEPT — C49 · Connectors, matched to their media**
> | Connector | Medium |
> |---|---|
> | **RJ-45** | UTP/STP Ethernet (8 positions) |
> | RJ-11 | telephone twisted pair (6 positions) |
> | **BNC** | coaxial (10Base2, video) |
> | F-type | coaxial (cable TV) |
> | **ST, SC, LC, MT-RJ** | optical fibre |
>
> **Applies when** the stem names a connector or asks which fits a medium.
>
> **Boundary:** RJ-45 and RJ-11 look alike and are the pair examiners exploit — the discriminator is the
> **position count** (8 versus 6) and the application (Ethernet versus telephone). An RJ-11 plug physically
> fits an RJ-45 socket, which is precisely why the confusion is a real-world fault as well as an exam trap.

**Wrong traces:** A = coaxial · C = optical fibre · D = telephone cable, the near-identical smaller plug.

---

**Q87.** The **BNC** connector is associated with which medium? `[Core]`

- **A)** coaxial cable
- **B)** unshielded twisted pair
- **C)** single-mode optical fibre
- **D)** infrared transceivers

**Answer: A) coaxial cable**

**Trace / Why:** BNC (Bayonet Neill–Concelman) is a bayonet-locking coaxial connector, used for 10Base2
"thin Ethernet" and for video. The family includes the **BNC T-connector**, which tapped a station onto the
bus, and the **BNC terminator**, which absorbed signals at each end of the backbone (C19).

**📘 CONCEPT — C49 (see Concept Index)** › The BNC family is worth remembering as a set because it maps onto
bus-topology Ethernet: **connector** to join cable to device, **T-connector** to tap a station onto the shared
backbone, **terminator** to prevent reflection. That trio only makes sense on a shared bus, which is why BNC
vanished along with bus topology when switched star cabling and RJ-45 took over.

**Wrong traces:** B = RJ-45 · C = ST, SC or LC · D = infrared needs no cable connector at all.

---

**Q88.** Which medium offers the **highest** bandwidth? `[Applied]`

- **A)** Cat 6 unshielded twisted pair
- **B)** coaxial cable
- **C)** optical fibre
- **D)** terrestrial microwave

**Answer: C) optical fibre**

**Trace / Why:** fibre's usable bandwidth is measured in terahertz, and with **wavelength-division
multiplexing** (C64) a single strand carries dozens of channels simultaneously — aggregate rates of terabits
per second are routine. No electrical or wireless medium comes close.

**📘 CONCEPT — C45 (see Concept Index)** › The bandwidth ordering is **UTP < coax < microwave < fibre**, and
fibre's margin is not incremental but orders of magnitude. The reason is physical: bandwidth available scales
with **carrier frequency**, and light at ~10¹⁴ Hz is roughly five orders of magnitude above microwave at
~10¹⁰ Hz. That is also why the historical progression of media has been towards ever higher frequencies.

**Wrong traces:** A = hundreds of megahertz · B = hundreds of megahertz to a few gigahertz ·
D = gigahertz, high for a wireless medium but far below fibre.

---

**Q89.** **Crosstalk** is `[Trap]`

- **A)** the reflection of a signal from an unterminated cable end
- **B)** the loss of signal strength with distance
- **C)** the variation in arrival time between bits sent in parallel
- **D)** unwanted coupling of a signal from one wire pair into an adjacent pair

**Answer: D) unwanted coupling of a signal from one wire pair into an adjacent pair**

**Trace / Why:** current in one pair creates a magnetic field that induces a voltage in a neighbouring pair
inside the same cable — so you hear another conversation, which is where the name comes from. It is a form of
**noise** (C11), reduced by giving each pair a different twist rate and by shielding.

**📘 CONCEPT — C48 (see Concept Index)** › Crosstalk is the impairment that **cable categories are specified
against** — Cat 5e and Cat 6 differ from Cat 5 chiefly in their crosstalk limits, not in any new physical
principle (C44). Distinguish it from the neighbouring terms this option set offers: **reflection** is caused
by an impedance mismatch, **attenuation** by distance, and **skew** by unequal propagation on parallel wires
(C14). All four degrade a signal; only crosstalk involves one conductor interfering with another.

**Wrong traces:** A = reflection, prevented by terminators (C19) · B = attenuation (C11) · C = skew, a
parallel-transmission problem (C14).

---

**Q90.** The maximum segment length for **1000BaseT** Ethernet over copper is `[Applied]`

- **A)** 25 m
- **B)** 50 m
- **C)** 100 m
- **D)** 500 m

**Answer: C) 100 m**

**Trace / Why:** every twisted-pair Ethernet variant — 10BaseT, 100BaseTX, 1000BaseT — is limited to
**100 m** per segment. The limit comes from attenuation and crosstalk accumulating over distance, and it has
stayed constant across generations because the cable categories improved in step with the data rates.

**📘 CONCEPT — C50 · Ethernet media designations decoded, and the distances that go with them**
> The name reads **⟨rate⟩ Base ⟨medium⟩**: rate in Mbps, **Base** for baseband signalling, then the medium.
>
> | Standard | Medium | Max segment |
> |---|---|---|
> | 10Base5 | thick coax | 500 m |
> | 10Base2 | thin coax | 185 m |
> | **10BaseT / 100BaseTX / 1000BaseT** | UTP | **100 m** |
> | 100BaseFX | multimode fibre | 2 km |
> | 1000BaseLX | single-mode fibre | 5 km+ |
>
> **Applies when** the stem gives an Ethernet standard name and asks the medium or distance.
>
> **Boundary:** the digit in the older coax names is the **distance in hundreds of metres** (10Base**5** =
> 500 m), while the letter in the newer names is the **medium** (**T** = twisted pair, **F** = fibre). Mixing
> the two conventions is the trap, and it is why 10Base2's limit is 185 m rather than the 200 m the name
> suggests.

**Wrong traces:** A and B = shorter than the standard permits · D = the 10Base5 thick-coax limit.

---

**Q91.** A **repeater** is used in a long cable run in order to `[Applied]`

- **A)** regenerate the signal before attenuation makes it unreadable
- **B)** filter frames by their destination MAC address
- **C)** convert between different network-layer protocols
- **D)** divide the cable into separate collision domains

**Answer: A) regenerate the signal before attenuation makes it unreadable**

**Trace / Why:** attenuation weakens the signal with distance (C11). A repeater receives the weakened
digital signal, **decides which bit each symbol was**, and transmits a clean new signal — so noise does not
accumulate. It works at layer 1 and reads no addresses at all.

**📘 CONCEPT — C51 · Repeater vs amplifier: regeneration beats amplification for digital signals**
> - An **amplifier** (analog) multiplies whatever it receives, **including the noise** — so noise
>   accumulates along a chain of amplifiers and the signal degrades irreversibly.
> - A **repeater** (digital) reconstructs the bit from the discrete levels and sends a **fresh** signal, so
>   noise does not accumulate at all.
>
> **Applies when** the stem mentions extending a cable run, or contrasts analog and digital amplification.
>
> **Boundary:** a repeater's inability to read addresses is what distinguishes it from a **bridge** — both
> extend a network, but only the bridge filters, and therefore only the bridge separates collision domains
> (C37). Option D is precisely a bridge's benefit offered under a repeater's name, and a **hub** is simply a
> multiport repeater with the same limitation.

**Wrong traces:** B = a bridge or switch, layer 2 · C = a gateway (C64 in Part 1) · D = a bridge or switch;
a repeater extends **one** collision domain rather than splitting it.

---

**Q92.** Which connectors are used with **optical fibre**? `[Core]`

- **A)** RJ-45 and RJ-11
- **B)** BNC and F-type
- **C)** DB-9 and DB-25
- **D)** SC, ST and LC

**Answer: D) SC, ST and LC**

**Trace / Why:** **SC** (subscriber/square, push-pull), **ST** (straight tip, bayonet) and **LC** (lucent,
small form factor) are the standard fibre connectors. All three must align cores a few microns across, which
is why they are precision-moulded and why fibre termination requires skill (Q79).

**📘 CONCEPT — C49 (see Concept Index)** › Group the connector families by medium and the questions become
recall: **RJ** for twisted pair, **BNC and F** for coax, **SC/ST/LC/MT-RJ** for fibre, **DB** for legacy
serial. Note that LC's small size is what allowed high port density in modern switches and transceivers,
which is why it displaced SC and ST in new installations — the same miniaturisation pressure that drove
serial links over parallel (C14).

**Wrong traces:** A = twisted pair · B = coaxial · C = legacy serial and parallel computer ports.

---
## Section 5 — Line Coding, Modulation & Multiplexing

**Q93.** **Line coding** is the process of converting `[Core]`

- **A)** analog data into an analog signal
- **B)** analog data into a digital signal
- **C)** digital data into a digital signal
- **D)** digital data into an analog signal

**Answer: C) digital data into a digital signal**

**Trace / Why:** the four conversions are distinct and each has its own name and techniques. Line coding
takes a sequence of **bits** and produces a sequence of **voltage levels** — the physical signal that
actually travels on the wire.

**📘 CONCEPT — C52 · The four conversions, and the technique family belonging to each**
> | Conversion | Name | Techniques |
> |---|---|---|
> | Digital data → digital signal | **line coding** | NRZ, Manchester, AMI, 4B/5B |
> | Digital data → analog signal | **modulation** | ASK, FSK, PSK, QAM |
> | Analog data → digital signal | **digitisation** | **PCM**, delta modulation |
> | Analog data → analog signal | analog modulation | AM, FM, PM |
>
> **Applies when** the stem names a technique and asks its category, or describes a conversion.
>
> **Boundary:** identify the **input** and the **output** separately — a modem performs digital-to-analog on
> transmit and analog-to-digital on receive, and PCM in a telephone network converts analog voice to digital
> while the line code then carries those bits. Confusing "digitisation" (PCM) with "modulation" (ASK/FSK) is
> the standard error, and the two run in opposite directions.

**Wrong traces:** A = analog modulation (AM/FM) · B = digitisation, i.e. PCM · D = modulation, i.e. what a
modem does.

---

**Q94.** The **Manchester** encoding scheme is described as *self-clocking* because `[Core]`

- **A)** it transmits a separate clock signal on a parallel wire
- **B)** it uses a fixed voltage level for the whole duration of each bit
- **C)** every bit period contains a mid-bit transition, from which the receiver recovers timing
- **D)** it requires the sender and receiver to be synchronised in advance by a shared oscillator

**Answer: C) every bit period contains a mid-bit transition, from which the receiver recovers timing**

**Trace / Why:** Manchester encodes a bit as a **transition** in the middle of the bit period — low-to-high
for one value, high-to-low for the other. Since a transition occurs in **every** bit period regardless of the
data, the receiver always has an edge to lock its clock onto, even during a long run of identical bits.

**📘 CONCEPT — C53 · Self-synchronisation: the receiver needs regular transitions to stay in step**
> A receiver samples the line at what it believes is the middle of each bit. If its clock drifts even
> slightly, over a long run of identical bits it will drift far enough to sample the wrong bit — so the code
> must guarantee frequent transitions.
>
> | Scheme | Transitions guaranteed? | Self-clocking |
> |---|---|---|
> | NRZ-L, NRZ-I | **no** — long runs are flat | no |
> | **Manchester** | **one per bit, always** | **yes** |
> | **Differential Manchester** | **one per bit, always** | **yes** |
> | AMI | only on 1s | partly (needs scrambling, C57) |
>
> **Applies when** the stem mentions synchronisation, clock recovery, or long runs of identical bits.
>
> **Boundary:** the guarantee costs **bandwidth** — Manchester needs twice the signal rate of NRZ for the same
> bit rate (Q96), which is why it was used for 10 Mbps Ethernet but abandoned at 100 Mbps in favour of the
> cheaper 4B/5B plus a scrambler (C56). Self-clocking is a trade, not a free improvement.

**Wrong traces:** A = a separate clock wire is exactly what self-clocking avoids · B = describes NRZ, which
is *not* self-clocking · D = a shared oscillator is the alternative to self-clocking, not its mechanism.

---

**Q95.** A significant problem with **NRZ** encoding is `[Trap]`

- **A)** it cannot represent a binary 0
- **B)** long runs of identical bits produce no transitions, causing loss of synchronisation and baseline wandering
- **C)** it requires twice the bandwidth of Manchester encoding
- **D)** it cannot be used on twisted-pair cable

**Answer: B) long runs of identical bits produce no transitions, causing loss of synchronisation and baseline wandering**

**Trace / Why:** in NRZ the level is held constant for the whole bit period, so a run of a hundred 1s is a
hundred bit-times of unchanging voltage. Two faults follow: the receiver's clock **drifts** with no edges to
correct it, and the running average of the signal **wanders** from the decision threshold, making the
receiver's level comparison unreliable. NRZ also carries a **DC component**, which transformers and
capacitively coupled links cannot pass.

**📘 CONCEPT — C54 · The three defects a line code is judged on**
> - **Synchronisation** — are there enough transitions for clock recovery? (C53)
> - **Baseline wandering** — does the running average drift, spoiling level decisions?
> - **DC component** — is there energy at zero frequency, which transformers and long lines block?
>
> Every scheme beyond NRZ exists to fix one or more of these.
>
> **Applies when** the stem asks what is wrong with a code, or why a more complex code is used.
>
> **Boundary:** the three defects share one cause — **insufficient transitions** — so a single fix addresses
> all three, which is why guaranteeing transitions (Manchester) or eliminating long runs (4B/5B, scrambling)
> is the universal remedy. Note that **NRZ-I** partly helps synchronisation by inverting on each 1, so a run
> of 1s produces transitions, but a run of **0s** still produces none.

**Wrong traces:** A = NRZ represents both values perfectly well · C = exactly reversed — **Manchester**
needs twice the bandwidth of NRZ (Q96) · D = NRZ is used on twisted pair routinely.

---

**Q96.** A **1 Mbps** data stream is encoded using **Manchester** encoding. The resulting signal rate is
`[Applied]`

- **A)** 0.5 Mbaud
- **B)** 1 Mbaud
- **C)** 2 Mbaud
- **D)** 4 Mbaud

**Answer: C) 2 Mbaud**

**Trace / Why:** Manchester places a transition in the middle of every bit period, and may need another at
the boundary between bits, so the signal changes up to **twice per bit**.

```
signal rate = 2 × bit rate = 2 × 1 Mbps = 2 Mbaud
```

The ratio is fixed: Manchester's **signal rate is always twice its bit rate**, so its bandwidth requirement
is double NRZ's for the same throughput.

**📘 CONCEPT — C55 · Signal rate per line code, expressed as a multiple of the bit rate**
> | Scheme | Signal rate (baud) | Bandwidth cost |
> |---|---|---|
> | NRZ-L, NRZ-I | ≈ **N/2** average | lowest |
> | AMI | ≈ N/2 average | low |
> | **Manchester, Differential Manchester** | **N** (transitions up to 2N) | **highest — 2× NRZ** |
> | 4B/5B + NRZ-I | N × 5/4 | 25% overhead only |
>
> **Applies when** the stem gives a bit rate and a line code and asks for the signal rate or bandwidth.
>
> **Boundary:** this is the **cost of self-clocking** (C53), and it is why the industry moved on: 100BaseTX
> gets guaranteed transitions from **4B/5B block coding** at only 25% overhead instead of Manchester's 100%.
> Note the direction of the relation — bit rate is *never* greater than the Manchester signal rate, so
> options below 1 Mbaud are impossible by construction.

**Wrong traces:** A = halves instead of doubling · B = assumes one transition per bit, i.e. NRZ ·
D = quadruples the rate.

---

**Q97.** In **AMI** (Alternate Mark Inversion) encoding `[Core]`

- **A)** binary 0 is a zero voltage level and binary 1 alternates between positive and negative
- **B)** binary 1 is always positive and binary 0 always negative
- **C)** each bit is represented by a transition at the middle of the bit period
- **D)** four bits are encoded as five signal elements

**Answer: A) binary 0 is a zero voltage level and binary 1 alternates between positive and negative**

**Trace / Why:** AMI is a **bipolar** scheme. A 0 is transmitted as no voltage; successive 1s alternate
between +V and −V. Because the 1s alternate, their contributions cancel, so the signal has **no DC
component** — the property that lets it pass through transformers on long-haul telephone links.

**📘 CONCEPT — C56 · AMI removes the DC component, but a run of zeros still kills synchronisation**
> Alternating the 1s makes the long-run average zero, solving the DC problem (C54). But a long run of **0s**
> is a long stretch of no voltage at all — no transitions, so the clock drifts.
>
> That residual defect is exactly what **scrambling** fixes (Q99): **B8ZS** and **HDB3** substitute a
> recognisable pattern containing deliberate violations of the alternation rule whenever a long zero run
> occurs, so the receiver both keeps its clock and knows the substitution was artificial.
>
> **Applies when** the stem mentions bipolar encoding, pseudoternary, DC components, or T1/E1 lines.
>
> **Boundary:** AMI is the reason scrambling exists — the two are a matched pair, and a question about B8ZS
> or HDB3 is implicitly a question about AMI's weakness. Note that AMI uses **three** levels (+, 0, −) while
> still carrying only one bit per symbol, so the extra level buys the DC balance rather than extra capacity.

**Wrong traces:** B = a simple polar scheme with no alternation, which would retain a DC component ·
C = Manchester · D = 4B/5B block coding (Q98).

---

**Q98.** In **4B/5B** block coding, every 4 bits of data are `[Core]`

- **A)** compressed into 3 signal elements to save bandwidth
- **B)** transmitted twice for error detection
- **C)** scrambled using a pseudorandom sequence
- **D)** replaced by a 5-bit code word chosen to guarantee frequent transitions

**Answer: D) replaced by a 5-bit code word chosen to guarantee frequent transitions**

**Trace / Why:** the 16 possible 4-bit groups are mapped to 16 carefully chosen 5-bit code words, selected so
that none has more than **three consecutive 0s**. Feeding those code words to NRZ-I therefore guarantees
regular transitions and solves synchronisation (C53) at a cost of only **25% overhead** — far cheaper than
Manchester's 100%.

**📘 CONCEPT — C57 · Block coding buys synchronisation with redundancy, cheaply**
> **mB/nB** coding maps m data bits to n signal bits with n > m. Since 2ⁿ > 2ᵐ, the surplus code words are
> discarded and only "good" ones (with enough transitions) are used. Bonus: the unused code words become
> detectable **invalid** patterns, giving a limited error-detection capability, and some are reserved as
> **control symbols** (100BaseTX uses them to mark idle and frame boundaries).
>
> | Scheme | Overhead | Used by |
> |---|---|---|
> | **4B/5B** | 25% | 100BaseTX (100 Mbps) |
> | **8B/10B** | 25% | Gigabit Ethernet, Fibre Channel, PCIe |
> | 64B/66B | 3% | 10 Gigabit Ethernet |
>
> **Applies when** the stem names an mB/nB scheme, or asks how synchronisation is achieved without doubling
> the bandwidth.
>
> **Boundary:** block coding fixes synchronisation but **not the DC component**, which is why it is normally
> paired with a **scrambler** (C58) — 100BaseTX uses 4B/5B *and* MLT-3. Note the trend in the table: as
> rates rose, overhead had to fall, driving the move from 4B/5B to 64B/66B.

**Wrong traces:** A = block coding **adds** bits rather than compressing · B = duplication is not the
mechanism · C = scrambling is a different, complementary technique.

---

**Q99.** **B8ZS** and **HDB3** are examples of `[Core]`

- **A)** block coding schemes that add redundant bits
- **B)** analog modulation techniques
- **C)** error-correcting codes
- **D)** scrambling techniques that replace long runs of zeros with recognisable violation patterns

**Answer: D) scrambling techniques that replace long runs of zeros with recognisable violation patterns**

**Trace / Why:** AMI loses synchronisation on long zero runs (C56). **B8ZS** (used in North America, T1)
replaces any string of eight 0s with a pattern containing two deliberate **bipolar violations** — pulses that
break AMI's alternation rule. **HDB3** (Europe, E1) does the same for four 0s. The receiver recognises the
violation as artificial, restores the zeros, and has kept its clock throughout.

**📘 CONCEPT — C58 · Scrambling: substitute a pattern the receiver can recognise as deliberate**
> The trick is that a **bipolar violation cannot arise from real data** under AMI's rules, so it is an
> unambiguous signal meaning "this was a substitution". No extra bits are added — the substitution is
> in-band — so unlike block coding there is **no bandwidth overhead**.
>
> | Scheme | Replaces | Region |
> |---|---|---|
> | **B8ZS** | 8 consecutive zeros | North America (T1) |
> | **HDB3** | 4 consecutive zeros | Europe, Japan (E1) |
>
> **Applies when** the stem names B8ZS or HDB3, mentions bipolar violations, or asks how AMI's zero-run
> problem is solved.
>
> **Boundary:** compare the two remedies for synchronisation directly — **block coding** (C57) adds
> redundant bits and costs bandwidth; **scrambling** substitutes patterns and costs none, but works only in
> a scheme that has spare, illegal signal patterns to exploit. AMI's three-level alternation provides exactly
> that spare capacity, which is why the pairing is natural.

**Wrong traces:** A = block coding, which adds bits · B = analog modulation of a carrier · C = these detect
nothing and correct nothing; they preserve timing.

---

**Q100.** In **ASK** (Amplitude Shift Keying), the bits are represented by varying the carrier's `[Core]`

- **A)** amplitude, while frequency and phase remain constant
- **B)** frequency, while amplitude and phase remain constant
- **C)** phase, while amplitude and frequency remain constant
- **D)** amplitude and phase together

**Answer: A) amplitude, while frequency and phase remain constant**

**Trace / Why:** a sine carrier has three properties that can be varied — amplitude, frequency and phase.
ASK varies **amplitude** only: for binary ASK, one amplitude (often zero) represents 0 and another
represents 1.

**📘 CONCEPT — C59 · The four digital-to-analog schemes, one per carrier property**
> | Scheme | Varies | Noise immunity | Notes |
> |---|---|---|---|
> | **ASK** | **amplitude** | **poor** — noise is amplitude | simplest, cheapest |
> | **FSK** | **frequency** | good | used by early modems |
> | **PSK** | **phase** | good | efficient; QPSK carries 2 bits/symbol |
> | **QAM** | **amplitude + phase** | good, highest rate | 16/64/256-QAM in modern links |
>
> **Applies when** the stem names a keying scheme and asks what varies, or asks which is most susceptible to
> noise.
>
> **Boundary:** **ASK is the most noise-vulnerable** precisely because noise itself adds amplitude — so a
> noise spike can imitate a level change, while it cannot easily imitate a frequency or phase change. That is
> why ASK alone is rare in practice and appears mainly inside **QAM**, where it is combined with phase to get
> the rate benefit without relying on amplitude alone.

**Wrong traces:** B = FSK · C = PSK · D = QAM.

---

**Q101.** **FSK** represents binary data by `[Core]`

- **A)** using two different carrier frequencies, one for each bit value
- **B)** using two different carrier amplitudes
- **C)** shifting the carrier phase by 180°
- **D)** varying both amplitude and phase simultaneously

**Answer: A) using two different carrier frequencies, one for each bit value**

**Trace / Why:** binary FSK transmits frequency f₁ for a 0 and f₂ for a 1, keeping amplitude and phase
constant. Because the information is in the frequency, amplitude noise does not corrupt it — which made FSK
the choice for early low-speed modems over noisy telephone lines.

**📘 CONCEPT — C59 (see Concept Index)** › FSK's robustness comes with a **bandwidth cost**: it must
accommodate two separate frequency bands plus the sidebands of each, so its bandwidth requirement exceeds
ASK's for the same bit rate. That is the general shape of the modulation trade — **robustness and rate are
bought with bandwidth or with receiver complexity**, and QAM (Q103) represents the modern resolution,
extracting many bits per symbol from a fixed bandwidth at the price of needing a good SNR.

**Wrong traces:** B = ASK · C = PSK, specifically BPSK · D = QAM.

---

**Q102.** **PSK** conveys information by varying the carrier's `[Applied]`

- **A)** amplitude
- **B)** frequency
- **C)** amplitude and frequency together
- **D)** phase

**Answer: D) phase**

**Trace / Why:** PSK shifts the **phase** of the carrier. BPSK uses two phases (0° and 180°) for one bit per
symbol; **QPSK** uses four phases 90° apart, so each symbol carries **2 bits** (log₂4 = 2) — doubling the bit
rate without increasing the baud rate or bandwidth (C8).

**📘 CONCEPT — C59 (see Concept Index)** › Phase modulation is where **bits per symbol** becomes the lever:
increasing the number of distinct phases raises the bit rate at a constant baud rate, which is Nyquist's
log₂L term in action (C9). The limit is noise — as phases crowd together, a small phase error moves a symbol
into its neighbour's decision region, and Shannon's bound (C10) is where the crowding stops paying. QAM
extends the idea into two dimensions by using amplitude as well (Q103), fitting more symbols in for the same
minimum separation.

**Wrong traces:** A = ASK · B = FSK · C = neither scheme; QAM combines amplitude with **phase**, not
frequency.

---

**Q103.** **QAM** achieves higher data rates than PSK alone by `[Applied]`

- **A)** varying both the amplitude and the phase of the carrier, giving more distinguishable symbols
- **B)** transmitting on two carrier frequencies simultaneously
- **C)** compressing the data before modulation
- **D)** doubling the transmitted power

**Answer: A) varying both the amplitude and the phase of the carrier, giving more distinguishable symbols**

**Trace / Why:** PSK varies one property, so symbols sit on a circle and crowd together as their number
grows. QAM varies **two** properties, placing symbols on a two-dimensional **constellation** — so for a given
minimum separation between symbols, far more of them fit. 16-QAM has 16 constellation points and carries 4
bits per symbol.

**📘 CONCEPT — C60 · QAM: two dimensions hold more symbols than one for the same noise margin**
> Symbols must stay far enough apart that noise cannot move one into another's region. On a **circle** (PSK)
> the points crowd quickly; on a **plane** (QAM) the area grows as the square, so many more points fit at the
> same spacing.
>
> | Scheme | Points | Bits/symbol |
> |---|---|---|
> | BPSK | 2 | 1 |
> | QPSK / 4-QAM | 4 | 2 |
> | **16-QAM** | 16 | **4** |
> | 64-QAM | 64 | 6 |
> | 256-QAM | 256 | 8 |
>
> **Applies when** the stem names a QAM order, a constellation, or asks how modern links reach high rates.
>
> **Boundary:** every step up the table demands a **better SNR**, because the points sit closer together — so
> the achievable QAM order is set by channel quality, which is Shannon's bound again (C10). This is exactly
> why WiFi and cable modems **adapt** their modulation to conditions, dropping from 256-QAM to QPSK as the
> signal degrades rather than failing outright.

**Wrong traces:** B = two carriers is a form of FDM (C63), not QAM · C = compression is a
presentation-layer function · D = more power does not create more symbols.

---

**Q104.** In **16-QAM**, how many bits does each symbol carry? `[Applied]`

- **A)** 2
- **B)** 4
- **C)** 6
- **D)** 8

**Answer: B) 4**

**Trace / Why:**

```
bits per symbol = log2(number of constellation points)
                = log2(16)
                = 4
```

So at 1,000 symbols per second, 16-QAM carries 4,000 bits per second.

**📘 CONCEPT — C60 (see Concept Index)** › The relation is **bits per symbol = log₂L**, identical to the
Nyquist level count (C9) and the baud-rate relation (C8) — one rule serving three topics. The numbers to have
ready: 4-QAM → 2, **16-QAM → 4**, 64-QAM → 6, 256-QAM → 8. Note the trap in the distractors: **6 and 8
belong to 64-QAM and 256-QAM**, so an option set for one QAM order is built from the neighbouring orders,
and reading "16" as the answer instead of log₂16 gives no listed option at all — a useful sanity signal.

**Wrong traces:** A = 4-QAM/QPSK · C = 64-QAM · D = 256-QAM.

---

**Q105.** **PCM** converts an analog signal to digital in which sequence of steps? `[Core]`

- **A)** quantisation, then sampling, then encoding
- **B)** sampling, then quantisation, then encoding
- **C)** encoding, then sampling, then quantisation
- **D)** sampling, then encoding, then quantisation

**Answer: B) sampling, then quantisation, then encoding**

**Trace / Why:** **sample** the continuous waveform at regular intervals (PAM) → **quantise** each sample to
the nearest of a finite set of levels → **encode** each level as a binary code word. The order is forced:
you cannot quantise a value you have not sampled, and you cannot encode a level you have not chosen.

**📘 CONCEPT — C61 · PCM's three steps, and where the irreversible loss happens**
> 1. **Sampling** — take amplitude readings at rate fs. Lossless **if** the sampling theorem is respected
>    (C62).
> 2. **Quantisation** — round each sample to one of L levels. **This is where information is lost**, and the
>    error introduced is **quantisation noise**.
> 3. **Encoding** — write each level as log₂L bits. Lossless.
>
> **Applies when** the stem asks for the steps, their order, or where PCM loses accuracy.
>
> **Boundary:** only **quantisation** is irreversible, so PCM's accuracy is set by the number of levels —
> more bits per sample means finer levels and less quantisation noise, at a proportional cost in bit rate
> (Q107). Sampling, by contrast, is *perfectly* reversible provided the rate is high enough, which is the
> content of the sampling theorem and a genuinely surprising result.

**Wrong traces:** A, C and D = each permutes the three steps into an order that is physically impossible.

---

**Q106.** According to the **sampling theorem**, an analog signal must be sampled at a rate of at least
`[Core]`

- **A)** the same as the highest frequency present in the signal
- **B)** half the highest frequency present in the signal
- **C)** four times the highest frequency present in the signal
- **D)** twice the highest frequency present in the signal

**Answer: D) twice the highest frequency present in the signal**

**Trace / Why:** the Nyquist–Shannon sampling theorem states that a signal band-limited to f_max can be
reconstructed **exactly** from samples taken at f_s ≥ 2·f_max. Sampling more slowly causes **aliasing** —
high-frequency components masquerade as lower ones and cannot be separated afterwards.

**📘 CONCEPT — C62 · The sampling theorem, and the standard telephony numbers it produces**
> ```
> Nyquist rate = 2 × f_max
> ```
> Telephone voice is band-limited to about 4 kHz, so the sampling rate is **8,000 samples/s** — the figure
> behind every digital telephony calculation. CD audio band-limits to 20 kHz and samples at 44.1 kHz, safely
> above the 40 kHz minimum.
>
> **Applies when** the stem gives a maximum frequency, or a sampling rate and asks whether it suffices.
>
> **Boundary:** the theorem needs the **highest** frequency, not the bandwidth — for a signal occupying
> 300–3,400 Hz the required rate is 2 × 3,400 = 6,800, not 2 × 3,100. And "at least" matters: sampling
> exactly at 2·f_max is the theoretical minimum, so practical systems sample above it and use an anti-alias
> filter to guarantee the band limit.

**Wrong traces:** A = one times, which aliases · B = half, which aliases badly · C = four times is safe but
wasteful, and is not the theorem's minimum.

---

**Q107.** A voice channel band-limited to **4 kHz** is digitised using PCM with **8 bits** per sample. The
resulting bit rate is `[Applied]` `[Asked: GATE-style]`

- **A)** 8 kbps
- **B)** 32 kbps
- **C)** 64 kbps
- **D)** 128 kbps

**Answer: C) 64 kbps**

**Trace / Why:**

```
sampling rate = 2 × f_max = 2 × 4,000 = 8,000 samples/s
bit rate      = sampling rate × bits per sample
              = 8,000 × 8
              = 64,000 bps = 64 kbps
```

This is the **DS-0** channel, the fundamental unit of digital telephony — and the reason a T1 carrying 24
such channels runs at 24 × 64 kbps + 8 kbps framing = **1.544 Mbps**.

**📘 CONCEPT — C61 (see Concept Index)** › The composite formula is
**bit rate = 2 · f_max · log₂L**, combining the sampling theorem (C62) with the encoding step. Note how the
two factors trade: doubling the **bits per sample** doubles the rate and halves the quantisation noise, while
doubling the **sampling rate** doubles the rate and extends the frequency range. Both cost bandwidth, and
64 kbps for telephone-quality voice is the number to have memorised, since so many carrier-system questions
are built on it.

**Wrong traces:** A = the sampling rate alone, in thousands · B = uses 4 bits per sample · D = uses a
16 kHz sampling rate.

---

**Q108.** **Delta modulation** differs from PCM in that delta modulation `[Core]`

- **A)** samples at half the Nyquist rate to halve the bit rate
- **B)** transmits a single bit per sample, indicating only whether the signal rose or fell
- **C)** encodes the absolute amplitude of each sample using 8 bits
- **D)** converts digital data into an analog signal

**Answer: B) transmits a single bit per sample, indicating only whether the signal rose or fell**

**Trace / Why:** rather than encoding each sample's absolute value, delta modulation encodes the **change**:
a 1 means "the signal increased by one step", a 0 means "it decreased by one step". One bit per sample makes
the encoder far simpler and the bit rate far lower than PCM's.

**📘 CONCEPT — C63 · Delta modulation: cheap and simple, at the cost of two characteristic distortions**
> | | PCM | Delta modulation |
> |---|---|---|
> | Bits per sample | log₂L (typically 8) | **1** |
> | Encodes | absolute amplitude | **the change** |
> | Complexity | higher | very low |
> | Quality | good | poorer |
>
> Two named defects: **slope overload**, when the signal changes faster than one step per sample so the
> staircase cannot keep up; and **granular noise**, when the signal is nearly flat and the output oscillates
> around it. **Adaptive delta modulation** varies the step size to reduce both.
>
> **Applies when** the stem mentions one-bit encoding, staircase approximation, slope overload or granular
> noise.
>
> **Boundary:** the two defects pull the **step size** in opposite directions — a large step avoids slope
> overload but worsens granular noise, and a small step does the reverse. That irreconcilable tension is
> precisely why the adaptive variant exists, and it is the examinable insight rather than the definitions
> themselves.

**Wrong traces:** A = the sampling rate is not reduced; the bits **per sample** are · C = describes PCM ·
D = that is modulation (C52), a different conversion entirely.

---

**Q109.** **FDM** (Frequency Division Multiplexing) shares a link by `[Core]`

- **A)** giving each signal the whole bandwidth for a brief time slot in rotation
- **B)** assigning each signal a different code sequence over the whole band
- **C)** allocating each signal a different frequency band, all transmitted simultaneously
- **D)** sending each signal on a separate physical wire

**Answer: C) allocating each signal a different frequency band, all transmitted simultaneously**

**Trace / Why:** FDM divides the link's bandwidth into non-overlapping frequency channels and modulates each
input onto its own carrier. All channels are present **at the same time**, separated in frequency, with
**guard bands** between them to prevent interference. Radio and television broadcasting and traditional cable
TV all work this way.

**📘 CONCEPT — C64 · The multiplexing families: divide the frequency, the time, or the wavelength**
> | Technique | Divides | Channels coexist |
> |---|---|---|
> | **FDM** | **frequency** | simultaneously, different bands |
> | **TDM** | **time** | in rotation, whole bandwidth each |
> | **WDM** | **wavelength** (optical FDM) | simultaneously, different colours |
> | **CDM** | **code** | simultaneously, same band, different codes |
>
> **Applies when** the stem describes how a shared link is divided.
>
> **Boundary:** FDM is inherently **analog** in conception and needs **guard bands**, which waste some
> spectrum; TDM is digital and needs **synchronisation** instead. So the two waste different resources, and
> the choice follows the signal type — which is why analog broadcast is FDM and digital carrier systems are
> TDM.

**Wrong traces:** A = TDM · B = CDM / spread spectrum · D = space division, i.e. simply using more cables.

---

**Q110.** In **synchronous TDM**, if one input channel has no data to send during its turn `[Core]`

- **A)** the multiplexer skips it and gives the slot to another channel
- **B)** the frame is shortened by one slot
- **C)** the multiplexer waits until that channel has data
- **D)** the slot is transmitted empty, and the capacity is wasted

**Answer: D) the slot is transmitted empty, and the capacity is wasted**

**Trace / Why:** synchronous TDM pre-allocates slots by position — slot 3 always belongs to input 3, so the
receiver identifies the source purely from **where** the data sits in the frame. That positional guarantee is
what removes the need for addressing, but it means an idle input's slot goes out empty and its share of
capacity is lost.

**📘 CONCEPT — C65 · Synchronous vs statistical TDM: guaranteed slots against efficient sharing**
> | | Synchronous TDM | Statistical TDM |
> |---|---|---|
> | Slot allocation | **fixed** by position | **on demand**, only to active inputs |
> | Idle input | slot sent **empty** — wasted | slot **skipped** |
> | Addressing needed | no — position identifies the source | **yes** — each slot carries an address |
> | Frame size | constant | varies with activity |
> | Suits | constant-rate traffic (voice) | **bursty** traffic (data) |
>
> **Applies when** the stem describes what happens to an idle channel, or asks which variant suits bursty
> data.
>
> **Boundary:** statistical TDM's efficiency is bought with **per-slot addressing overhead** and variable
> delay — the same trade as packet switching over circuit switching (C24), and for the same reason. Note the
> consequence: statistical TDM can **oversubscribe**, admitting inputs whose combined peak exceeds the link,
> which works only because they are rarely all active at once.

**Wrong traces:** A = **statistical** TDM's behaviour, the direct contrast · B = the frame length is fixed in
synchronous TDM · C = the multiplexer never waits; slots are clocked out on schedule.

---

**Q111.** **Statistical TDM** requires each slot to carry an address because `[Core]`

- **A)** the receiver must acknowledge each slot individually
- **B)** slots are allocated only to channels with data, so position no longer identifies the source
- **C)** addresses are needed to correct transmission errors
- **D)** each channel uses a different frequency band

**Answer: B) slots are allocated only to channels with data, so position no longer identifies the source**

**Trace / Why:** in synchronous TDM the receiver knows the source from the slot's **position** (C65). Once
idle channels are skipped, positions shift from frame to frame, so the receiver can no longer infer the
source — each slot must therefore say which input it came from.

**📘 CONCEPT — C65 (see Concept Index)** › This is a clean instance of a general principle: **removing an
implicit guarantee forces an explicit label.** Synchronous TDM's fixed positions are an implicit addressing
scheme; abandoning them for efficiency requires explicit addressing, which costs overhead in every slot. The
same exchange appears throughout networking — virtual circuits carry a short VCI because the path is fixed,
while datagrams must carry a full address because it is not (C26).

**Wrong traces:** A = no per-slot acknowledgement is involved · C = error control is separate from
addressing · D = that describes FDM (C64), not TDM at all.

---

**Q112.** **WDM** (Wavelength Division Multiplexing) is essentially `[Applied]`

- **A)** FDM applied to optical fibre, with different wavelengths of light acting as the separate channels
- **B)** TDM applied to optical fibre, with each channel taking a turn
- **C)** a technique for converting analog signals to digital form
- **D)** a method of encoding four data bits as five signal elements

**Answer: A) FDM applied to optical fibre, with different wavelengths of light acting as the separate channels**

**Trace / Why:** WDM combines several optical carriers of different **wavelengths** (colours) onto one fibre
using a prism or diffraction grating, and separates them again at the far end. Since wavelength and frequency
are inversely related, this is FDM in optical clothing — and it is what allows a single strand to carry
dozens of independent channels simultaneously.

**Worked cross-check on TDM, for contrast:** four inputs of 1,000 bps each, synchronously
time-multiplexed, produce an output of

```
4 × 1,000 = 4,000 bps
```

— the whole capacity delivered to one input at a time, in rotation. WDM instead runs all four
**concurrently** on separate wavelengths.

**📘 CONCEPT — C64 (see Concept Index)** › WDM is the reason fibre's bandwidth advantage is so overwhelming
in practice (Q88): **DWDM** (dense WDM) packs 40, 80 or more channels of 10–100 Gbps onto one strand,
multiplying an already large capacity. Note the arithmetic difference between the two families — in **TDM**
the output rate is the **sum** of the inputs (so the link must be n times faster), whereas in **FDM/WDM** the
channels occupy separate bands at their original rates and the link must be n times **wider**. Rate versus
width is the distinction worth carrying.

**Wrong traces:** B = WDM channels are simultaneous, not time-shared · C = that is PCM (C61) · D = that is
4B/5B block coding (C57).

---
## Section 6 — Data Link Layer: Framing, Error Control & Flow Control

*Highest GATE yield in the chapter, together with the transport layer. Every numerical is worked.*

**Q113.** The purpose of **framing** at the data link layer is to `[Core]`

- **A)** convert digital data into an analog signal
- **B)** choose the best route to the destination network
- **C)** mark the beginning and end of each unit so the receiver can extract discrete blocks from the bit stream
- **D)** divide the available bandwidth among several senders

**Answer: C) mark the beginning and end of each unit so the receiver can extract discrete blocks from the bit stream**

**Trace / Why:** the physical layer hands up an undifferentiated stream of bits. Without boundaries the
receiver cannot tell where one frame's header stops and the next begins — so it could not locate addresses,
compute a checksum over the right bits, or acknowledge anything.

**📘 CONCEPT — C66 · Framing needs a delimiter, and the delimiter creates a transparency problem**
> A frame is delimited by a special pattern — HDLC uses the **flag** `01111110`. But then the flag pattern
> must never appear inside the payload, or the receiver would end the frame early. Solving that is the
> **data transparency** problem, and there are two standard solutions:
> - **Byte (character) stuffing** — insert an escape byte before any accidental flag or escape in the data.
> - **Bit stuffing** — insert a 0 after every five consecutive 1s, so the six-1 flag can never occur in data.
>
> **Applies when** the stem mentions flags, delimiters, stuffing, escape characters, or transparency.
>
> **Boundary:** framing also enables **error control**, because a checksum must cover a known extent of bits
> — so framing is a precondition for the rest of the data link layer's work, not merely a convenience.
> Fixed-size frames need no delimiter at all (ATM cells are 53 bytes always), which is exactly why cell
> switching avoids the whole transparency problem.

**Wrong traces:** A = modulation, a physical-layer function (C52) · B = routing, a network-layer function
(C29) · D = multiplexing (C64) or medium access (C77).

---

**Q114.** In **byte stuffing**, when the flag pattern appears inside the data field, the sender `[Applied]`

- **A)** discards the frame and reports an error
- **B)** splits the frame into two smaller frames
- **C)** inserts an escape byte (ESC) before the offending byte
- **D)** inverts every bit of the offending byte

**Answer: C) inserts an escape byte (ESC) before the offending byte**

**Trace / Why:** the receiver, seeing ESC, knows the **next** byte is data rather than a delimiter, removes
the ESC and passes the byte through. Because ESC itself might occur in the data, an ESC in the payload must
also be escaped — so ESC becomes ESC ESC.

**📘 CONCEPT — C67 · Byte stuffing: escape the delimiter, then escape the escape**
> ```
> data contains FLAG  →  send  ESC FLAG   →  receiver strips ESC, keeps FLAG as data
> data contains ESC   →  send  ESC ESC    →  receiver strips one ESC, keeps ESC as data
> ```
> The second rule is the one students omit, and without it the scheme is ambiguous.
>
> **Applies when** the stem uses byte-oriented framing (PPP, BISYNC) or mentions escape characters.
>
> **Boundary:** byte stuffing works on **whole bytes**, so it suits byte-oriented protocols; bit-oriented
> protocols such as HDLC use **bit stuffing** instead (Q115), which is finer-grained and adds less overhead.
> Note that stuffing makes the frame **variable in length** even for fixed-length payloads, which is why the
> receiver must destuff before interpreting any length field.

**Wrong traces:** A = the frame is legitimate; the encoding merely needs adjusting · B = splitting does not
solve the ambiguity · D = inversion is not recoverable by the receiver.

---

**Q115.** The **bit stuffing** rule used in HDLC is: after every `[Applied]`

- **A)** six consecutive 0s, insert a 1
- **B)** five consecutive 0s, insert a 1
- **C)** five consecutive 1s, insert a 0
- **D)** six consecutive 1s, insert a 0

**Answer: C) five consecutive 1s, insert a 0**

**Trace / Why:** the HDLC flag is `01111110` — six 1s between two 0s. If the sender inserts a 0 after every
run of **five** 1s in the data, six consecutive 1s can never occur in the payload, so any occurrence of the
flag pattern must be a genuine delimiter. The receiver simply deletes a 0 that follows five 1s.

**📘 CONCEPT — C68 · Bit stuffing: five ones, then a stuffed zero**
> ```
> FLAG = 01111110               (six 1s — the delimiter)
> sender:   after 11111 in data, insert 0
> receiver: after 11111, delete the following 0
> ```
> The count is **five, not six**: stuffing after five prevents the sixth from ever appearing.
>
> **Applies when** the stem shows a bit string to be stuffed or destuffed, or names HDLC or SDLC.
>
> **Boundary:** the off-by-one between the flag's **six** 1s and the stuffing rule's **five** is the whole
> trap, and it is worth restating: you stuff *one earlier* than the pattern you are avoiding. Note that
> stuffing is transparent to the layers above — they never see the inserted bits — but it does make the
> transmitted frame slightly longer than the logical one, which matters when computing exact transmission
> times.

**Wrong traces:** A and B = stuff on runs of 0s, which HDLC's flag does not contain · D = stuffing after six
would be too late, since six 1s is already the flag.

---

**Q116.** A data field consists of a 0, followed by **twenty consecutive 1s**, followed by a 0. Using HDLC
bit stuffing, how many 0 bits are inserted? `[Applied]`

- **A)** 1
- **B)** 2
- **C)** 3
- **D)** 4

**Answer: D) 4**

**Trace / Why:** the counter resets to zero after each stuffed bit, so each *group of five* 1s triggers one
insertion.

```
0 | 11111 → stuff | 11111 → stuff | 11111 → stuff | 11111 → stuff | 0
     (5)              (10)             (15)            (20)

twenty 1s ÷ 5 per stuff = 4 stuffed bits
```

Transmitted field length = 22 original bits + 4 stuffed = 26 bits.

**📘 CONCEPT — C68 (see Concept Index)** › The mechanical rule is **⌊(number of consecutive 1s) ÷ 5⌋**
insertions — but only because the counter **resets after each stuffed 0**, which is the step that decides the
arithmetic. A common error is to reason "twenty 1s contains sixteen overlapping runs of five" and stuff far
too often; the reset makes the runs disjoint. Note the edge case: exactly five 1s stuffs once, and exactly
four stuffs not at all.

**Wrong traces:** A = one insertion for the whole run, ignoring the repetition · B = stuffs every ten bits ·
C = stuffs after the first, second and third groups but misses the fourth.

---

**Q117.** A **burst error** is one in which `[Core]`

- **A)** two or more bits in the data unit are corrupted, not necessarily all consecutive
- **B)** exactly one bit is inverted
- **C)** the entire frame is lost in transit
- **D)** bits arrive in the wrong order

**Answer: A) two or more bits in the data unit are corrupted, not necessarily all consecutive**

**Trace / Why:** a burst error's **length** is measured from the first corrupted bit to the last, and bits in
between may or may not be affected. Bursts are far more common than single-bit errors in practice, because
noise events (an impulse, a lightning strike, a motor starting) last much longer than one bit period at
modern data rates.

**📘 CONCEPT — C69 · Two error types, and why burst errors dominate**
> - **Single-bit error** — exactly one bit inverted. Rare in serial transmission, because a noise event
>   lasting even a microsecond spans many bits at megabit rates.
> - **Burst error** — corruption spanning two or more bit positions; the length is first-to-last inclusive.
>
> **Applies when** the stem describes a pattern of corruption, or asks which error type a code detects.
>
> **Boundary:** the practical dominance of bursts is why **CRC** is the standard detection method — it
> detects **all** bursts up to the length of its check field (C72), whereas simple parity fails as soon as an
> even number of bits is corrupted (Q120). Note that a **lost** frame is not an error in this taxonomy; it is
> handled by timers and retransmission (C79) rather than by an error-detecting code.

**Wrong traces:** B = a single-bit error · C = frame loss, handled by ARQ timers rather than error codes ·
D = misordering, a network-layer consequence (C26).

---

**Q118.** The essential difference between error **detection** and error **correction** is that correction
`[Trap]`

- **A)** requires the receiver to acknowledge every frame
- **B)** requires enough redundancy to identify *which* bits are wrong, not merely that something is wrong
- **C)** is performed at the physical layer while detection is performed at the data link layer
- **D)** is always cheaper to implement than detection

**Answer: B) requires enough redundancy to identify *which* bits are wrong, not merely that something is wrong**

**Trace / Why:** detecting an error needs only enough redundancy to know the received word is invalid.
Correcting it needs enough to determine which **valid** word was intended — a much stronger requirement,
because the code must distinguish between all the ways the word could have been corrupted. That is why
correcting codes need substantially more redundant bits than detecting codes.

**📘 CONCEPT — C70 · Detection versus correction, and the two strategies built on them**
> | | Detection | Correction (FEC) |
> |---|---|---|
> | Redundancy needed | low | **much higher** |
> | On finding an error | request retransmission (**ARQ**) | **repair locally** |
> | Suits | links where retransmission is cheap | links where it is not — satellite, one-way, real-time |
>
> **Applies when** the stem contrasts the two, or asks why a particular link uses forward error correction.
>
> **Boundary:** the choice is decided by the **cost of retransmission**. On a LAN, detect-and-retransmit is
> far cheaper than carrying correction overhead on every frame. On a deep-space link with a 40-minute round
> trip, retransmission is unthinkable and FEC is the only option. Note that the two are combined in
> practice — WiFi uses FEC *and* retransmits what the FEC cannot repair.

**Wrong traces:** A = acknowledgement belongs to ARQ, which is the **detection**-based strategy · C = both
happen at the data link layer (and above) · D = correction is more expensive, being the whole reason
detection is preferred where possible.

---

**Q119.** A **simple parity check** appended to a data unit can detect `[Core]`

- **A)** any odd number of bit errors
- **B)** any even number of bit errors
- **C)** all single and double bit errors
- **D)** all burst errors of any length

**Answer: A) any odd number of bit errors**

**Trace / Why:** the parity bit is chosen so the total number of 1s is even (even parity). Any **odd** number
of inversions flips the parity and is detected. Any **even** number leaves the parity unchanged and passes
undetected — so one error is caught, two are missed, three are caught, and so on.

**📘 CONCEPT — C71 · Simple parity: cheapest possible, and blind to half of all error patterns**
> One redundant bit, no correction capability, and detection of **odd** error counts only. Since burst errors
> commonly corrupt an even number of bits, simple parity misses roughly half of real-world error patterns —
> which is why it survives only where errors are rare and single (memory, asynchronous serial links) and
> never on a network link.
>
> **Applies when** the stem mentions a single parity bit, even or odd parity, or asks what parity misses.
>
> **Boundary:** **two-dimensional parity** (a parity bit per row *and* per column) is a real improvement — it
> detects all 1, 2 and 3-bit errors and can **correct** any single-bit error, since the failing row and
> column intersect at the culprit (Q121). But even 2-D parity misses some 4-bit patterns: four errors forming
> a rectangle preserve every row and column parity.

**Wrong traces:** B = exactly the patterns parity **cannot** detect · C = double errors are missed ·
D = burst errors of even weight pass undetected.

---

**Q120.** Why does a simple parity check fail to detect **two** bit errors in the same data unit? `[Trap]`

- **A)** because the parity bit itself is likely to be one of the corrupted bits
- **B)** because two errors always occur in different bytes
- **C)** because the receiver only checks parity on frames longer than 8 bits
- **D)** because two inversions cancel out, leaving the total number of 1s with the same parity

**Answer: D) because two inversions cancel out, leaving the total number of 1s with the same parity**

**Trace / Why:** each inversion changes the 1-count by ±1. Two inversions change it by −2, 0 or +2 — always
an **even** change, so the parity is preserved and the receiver sees a valid word.

```
sent:     1 0 1 1 0 0 1 | P=0   (four 1s, even ✔)
errors on bits 2 and 5:
received: 1 1 1 1 1 0 1 | P=0   (six 1s, even ✔ — accepted, though wrong)
```

**📘 CONCEPT — C71 (see Concept Index)** › The mechanism generalises: **parity detects an error pattern
exactly when the pattern has odd weight.** That single statement covers every case — one error detected, two
missed, three detected, four missed. It also explains why parity's failure rate is so poor against **burst**
errors: a burst of length ≥ 2 has an even weight roughly half the time, so parity misses about half of them,
which is unacceptable on a real link (C69).

**Wrong traces:** A = the parity bit's own corruption is one error, not the explanation for two ·
B = byte boundaries are irrelevant · C = parity is checked regardless of length.

---

**Q121.** **Two-dimensional parity** (parity bits on both rows and columns) can `[Applied]`

- **A)** detect and correct any number of errors
- **B)** detect all 1, 2 and 3-bit errors, and correct any single-bit error
- **C)** correct all double-bit errors
- **D)** detect only single-bit errors, like simple parity

**Answer: B) detect all 1, 2 and 3-bit errors, and correct any single-bit error**

**Trace / Why:** arrange the data as a grid and add a parity bit to each row and each column. A single
corrupted bit fails **exactly one row parity and exactly one column parity**, and those two intersect at a
unique cell — so the error is located and can be corrected by inverting it. Two or three errors disturb the
parities in ways that are detectable but not uniquely locatable.

**📘 CONCEPT — C71 (see Concept Index)** › Location is what turns detection into correction, and 2-D parity
achieves it cheaply by giving each bit **two independent parity memberships** (its row and its column) — the
same idea Hamming codes generalise with overlapping parity groups (C74). Its limit is the **rectangle
pattern**: four errors at the corners of a rectangle flip two row parities and two column parities *twice
each*, restoring all of them, so that specific 4-bit pattern is completely invisible.

**Wrong traces:** A = no code corrects an unlimited number of errors · C = double errors are detected but
cannot be located uniquely · D = understates it; 2-D parity is a genuine improvement.

---

**Q122.** The **Internet checksum** used by IP, TCP and UDP is computed by `[Core]`

- **A)** dividing the data by a generator polynomial and keeping the remainder
- **B)** summing the data in 16-bit words using one's complement arithmetic and transmitting the complement of the sum
- **C)** counting the number of 1 bits and appending the count
- **D)** XOR-ing all bytes of the data together

**Answer: B) summing the data in 16-bit words using one's complement arithmetic and transmitting the complement of the sum**

**Trace / Why:** the sender divides the data into 16-bit words, adds them with end-around carry (one's
complement addition), complements the result and puts it in the checksum field. The receiver adds **all**
words *including* the checksum; if nothing was corrupted the result is all 1s, whose complement is zero — so
the receiver simply tests for zero.

**📘 CONCEPT — C72 · Checksum vs CRC: cheap arithmetic against strong polynomial detection**
> | | Internet checksum | CRC |
> |---|---|---|
> | Operation | one's complement **addition** | polynomial **division** (XOR) |
> | Strength | weak — misses reordered words and some multi-bit patterns | **strong** — all bursts ≤ r bits |
> | Cost in software | **cheap** | more expensive |
> | Used by | **IP, TCP, UDP** headers | **Ethernet, HDLC, PPP** frames (the FCS) |
>
> **Applies when** the stem names a checksum or an FCS, or asks which layer uses which.
>
> **Boundary:** the division of labour is deliberate — the **data link layer** uses a strong CRC in hardware
> because it guards the raw, error-prone medium, while the **transport and network layers** use a cheap
> checksum in software as an end-to-end backstop (C35). A notable weakness of the checksum: because addition
> is commutative, **swapping two words is undetectable**.

**Wrong traces:** A = CRC · C = no protocol uses a 1-count as a checksum · D = a simple XOR (a longitudinal
parity), weaker still.

---

**Q123.** **CRC** error detection is based on `[Core]`

- **A)** treating the data as a polynomial and taking the remainder of division by a generator polynomial
- **B)** adding all the bytes and appending the total
- **C)** transmitting each bit twice and comparing
- **D)** counting parity across rows and columns

**Answer: A) treating the data as a polynomial and taking the remainder of division by a generator polynomial**

**Trace / Why:** the data bits are the coefficients of a binary polynomial. The sender appends r zeros
(r = degree of the generator), divides by the generator using **modulo-2 arithmetic** (XOR, no carries), and
replaces the zeros with the r-bit remainder. The receiver divides the whole received frame by the same
generator: a remainder of zero means no detected error.

**📘 CONCEPT — C73 · CRC procedure, and why the arithmetic is XOR**
> ```
> 1. r = degree of generator = (generator length − 1)
> 2. append r zeros to the data
> 3. divide by the generator using XOR (modulo-2, no borrow/carry)
> 4. the r-bit remainder is the CRC / FCS; transmit data + CRC
> 5. receiver divides the whole thing; remainder 0 ⇒ accept
> ```
> Standard generators: **CRC-8, CRC-10, CRC-16, CRC-32** (CRC-32 is Ethernet's FCS).
>
> **Applies when** the stem gives a data word and a generator, or names an FCS.
>
> **Boundary:** modulo-2 division has **no carries or borrows**, so each step is a single XOR — which is what
> makes CRC cheap enough to implement in hardware at line rate with a shift register. Note that the
> **remainder length equals the generator's degree**, i.e. one less than the generator's bit length, and that
> off-by-one is the commonest setup error (Q124).

**Wrong traces:** B = the checksum · C = simple repetition, wasteful and weak · D = two-dimensional parity.

---

**Q124.** The data word **1101011011** is to be protected by CRC using the generator **10011**. The
transmitted CRC (remainder) is `[Applied]` `[Asked: GATE-style]`

- **A)** 0000
- **B)** 1001
- **C)** 1110
- **D)** 1111

**Answer: C) 1110**

**Trace / Why:** the generator has 5 bits, so its degree is 4 — append **4** zeros and divide by XOR.

```
      1101011011 0000    ← data with 4 appended zeros
      10011
      -----
       10001 1011 0000
       10011
       -----
          10 1011 0000
             10011
             -----
              1000 0000
              10011  … continuing the XOR division …
      remainder = 1110
```

Transmitted frame = `1101011011` + `1110` = `11010110111110`. The receiver divides that by 10011 and gets
remainder 0.

**📘 CONCEPT — C73 (see Concept Index)** › Two checks catch almost every CRC slip: the remainder must have
**exactly r bits** (here 4, matching the appended zeros), and dividing the **transmitted** frame by the
generator must give **zero**. Note that a leading zero in the remainder is significant and must be written —
a remainder of `0110` is four bits, not three. Distractor A is the value the *receiver* computes on a clean
frame, which is a different quantity from the transmitted CRC.

**Wrong traces:** A = the receiver's remainder on an error-free frame, not the CRC itself · B = arises from
appending only 3 zeros · D = an XOR step performed as an arithmetic subtraction.

---

**Q125.** A CRC with an **r-bit** check field is guaranteed to detect `[Applied]`

- **A)** all errors, of any pattern and length
- **B)** only single-bit errors
- **C)** all burst errors of length **r or less**
- **D)** all burst errors of length **2r or less**

**Answer: C) all burst errors of length **r or less****

**Trace / Why:** a burst shorter than or equal to the check field's length cannot be a multiple of the
generator polynomial, so it always leaves a non-zero remainder. Longer bursts are detected with very high
probability — for CRC-32 the escape probability is about 2⁻³², roughly one in four billion — but not with
certainty.

**📘 CONCEPT — C73 (see Concept Index)** › CRC's guarantees, in order of strength: **all single-bit errors**;
**all double-bit errors** (with a suitable generator); **all odd numbers of errors** if the generator has the
factor (x + 1); and **all bursts of length ≤ r**. Everything longer is probabilistic. Note the design
consequence: Ethernet uses CRC-**32** because its frames are long and a 32-bit field makes the escape
probability negligible, while a short control frame can be safely protected by CRC-8.

**Wrong traces:** A = no finite check field detects every possible pattern · B = drastically understates
CRC · D = overstates the **guarantee**; bursts longer than r are only detected probabilistically.

---

**Q126.** The **Hamming distance** between two code words is `[Core]`

- **A)** the number of bit positions in which they differ
- **B)** the number of 1 bits in the longer of the two
- **C)** the arithmetic difference between their decimal values
- **D)** the number of redundant bits used by the code

**Answer: A) the number of bit positions in which they differ**

**Trace / Why:** compute it by XOR-ing the two words and counting the 1s in the result.

```
10101  ⊕  11110  =  01011   →  three 1s  →  Hamming distance = 3
```

**📘 CONCEPT — C74 · Minimum Hamming distance determines exactly what a code can do**
> The **minimum** distance d_min is the smallest distance between any two valid code words in the code. It
> alone fixes the code's power:
> ```
> to DETECT  up to s errors:   d_min ≥ s + 1
> to CORRECT up to t errors:   d_min ≥ 2t + 1
> ```
> | d_min | Detects | Corrects |
> |---|---|---|
> | 2 | 1 | 0 |
> | **3** | **2** | **1** |
> | 4 | 3 | 1 |
> | **5** | **4** | **2** |
>
> **Applies when** the stem gives a minimum distance, or asks how many errors a code handles.
>
> **Boundary:** correction is roughly **twice as expensive** as detection in distance terms, which is the
> formal statement of C70 — you need distance to *see* that a word is invalid, and twice as much to know
> which valid word it came from. Note that simple parity has d_min = 2, which is exactly why it detects one
> error and corrects none (C71).

**Wrong traces:** B = a Hamming **weight**, not a distance · C = decimal subtraction is unrelated · D = the
redundancy count, a different property of the code.

---

**Q127.** To **correct** up to **2** bit errors, a code must have a minimum Hamming distance of at least
`[Applied]`

- **A)** 3
- **B)** 5
- **C)** 7
- **D)** 9

**Answer: B) 5**

**Trace / Why:**

```
d_min ≥ 2t + 1
      = 2(2) + 1
      = 5
```

With d_min = 5, a word corrupted in 2 positions is still strictly closer to the intended code word (distance
2) than to any other (distance ≥ 3), so nearest-neighbour decoding recovers it uniquely.

**📘 CONCEPT — C74 (see Concept Index)** › The two formulas are worth keeping side by side, because papers
ask both and the options overlap: **detect s ⇒ d_min ≥ s + 1** and **correct t ⇒ d_min ≥ 2t + 1**. So d_min = 5
corrects 2 **and** detects 4 — a code always detects more errors than it corrects, and quoting the detection
figure where correction was asked (or the reverse) is the standard error. Distractor A is exactly the answer
for correcting **one** error.

**Wrong traces:** A = corrects only 1 error (2×1 + 1) · C = corrects 3 · D = corrects 4 — each the formula
evaluated at the wrong t.

---

**Q128.** A Hamming code protects **7** data bits. How many redundant (parity) bits are required?
`[Applied]` `[Asked: GATE-style]`

- **A)** 3
- **B)** 4
- **C)** 5
- **D)** 7

**Answer: B) 4**

**Trace / Why:** the r parity bits must be able to name every possible single-error position **plus** the
no-error case.

```
condition:  2^r ≥ m + r + 1
try r = 3:  2^3 = 8  ≥ 7 + 3 + 1 = 11 ?   8 ≥ 11  ✘
try r = 4:  2^4 = 16 ≥ 7 + 4 + 1 = 12 ?  16 ≥ 12  ✔
```

So **r = 4**, giving an 11-bit code word carrying 7 data bits.

**📘 CONCEPT — C75 · The Hamming bound: 2^r ≥ m + r + 1**
> The r parity bits form a syndrome that must identify one of **m + r** error positions or signal "no error"
> — hence m + r + 1 outcomes and 2^r available syndromes.
>
> | m (data bits) | r (parity bits) | Code word |
> |---|---|---|
> | 4 | **3** | 7 (the classic Hamming(7,4)) |
> | 7 | **4** | 11 |
> | 8 | 4 | 12 |
> | 11 | 4 | 15 |
> | 16 | 5 | 21 |
>
> Parity bits occupy the **power-of-two** positions 1, 2, 4, 8 …, each checking the positions whose index
> includes its bit.
>
> **Applies when** the stem gives a data-bit count and asks for parity bits, or the reverse.
>
> **Boundary:** solve by **trial**, not by rearranging — r appears on both sides, so the inequality has no
> closed form. And note that r must satisfy the condition *including its own contribution* to the length:
> testing 2^r ≥ m + 1 instead gives r = 3 here, which is distractor A and the commonest mistake.

**Wrong traces:** A = from omitting r from the right-hand side · C = an over-estimate, satisfying the bound
but not minimal · D = equal to the data bits, no derivation.

---

**Q129.** In the **stop-and-wait** protocol, the sender `[Core]`

- **A)** sends a window of N frames before waiting for any acknowledgement
- **B)** sends one frame and waits for its acknowledgement before sending the next
- **C)** sends frames continuously and ignores acknowledgements
- **D)** sends each frame twice to guarantee delivery

**Answer: B) sends one frame and waits for its acknowledgement before sending the next**

**Trace / Why:** exactly one frame is outstanding at any time. That makes flow control trivial — the receiver
controls the pace simply by withholding the acknowledgement — but it leaves the link idle for a full
round-trip after every frame, which is disastrous when the round trip is long relative to the frame's
transmission time.

**📘 CONCEPT — C76 · Stop-and-wait: correct, simple, and catastrophically inefficient on long links**
> One frame outstanding, so throughput is one frame per round trip regardless of the link's speed. The key
> parameter is
> ```
> a = Tp / Tt      (propagation time ÷ transmission time)
> efficiency = 1 / (1 + 2a)
> ```
> **Applies when** the stem names stop-and-wait, or gives Tp and Tt and asks for utilisation.
>
> **Boundary:** **stop-and-wait ARQ** adds the reliability machinery on top — sequence numbers (just 1 bit
> suffices), a timer, and retransmission on timeout. The 1-bit sequence number is needed to distinguish a
> **retransmitted frame** from the next new one after a lost acknowledgement (Q140), which is the subtle
> point: without it a lost ACK causes a duplicate to be accepted as new data.

**Wrong traces:** A = sliding window / Go-Back-N · C = no flow control at all · D = simple repetition, not
stop-and-wait.

---

**Q130.** On a link where the propagation time is **4 times** the frame transmission time (a = 4), the
efficiency of stop-and-wait is `[Applied]` `[Asked: GATE-style]`

- **A)** 11.1%
- **B)** 20.0%
- **C)** 33.3%
- **D)** 50.0%

**Answer: A) 11.1%**

**Trace / Why:**

```
efficiency = 1 / (1 + 2a)
           = 1 / (1 + 8)
           = 1 / 9
           = 0.111 = 11.1%
```

Intuitively: the sender spends 1 time unit transmitting and then 8 waiting (4 out, 4 back), so it is busy
one-ninth of the time. Nearly 89% of the link's capacity is wasted.

**📘 CONCEPT — C77 · The 1/(1 + 2a) formula, and the values worth recognising**
> ```
> a = Tp / Tt ,      efficiency = 1 / (1 + 2a) ,      useful throughput = efficiency × bandwidth
> ```
> | a | Efficiency |
> |---|---|
> | 0 | 100% |
> | 0.5 | 50% |
> | 1 | 33.3% |
> | **4** | **11.1%** |
> | 9 | 5.3% |
>
> **Applies when** the stem gives a, or gives distance/rate/size from which a can be computed (C5, C6).
>
> **Boundary:** the **2a** is the round trip — the sender waits Tp for the frame to arrive *and* Tp for the
> acknowledgement to come back. Using a single a instead of 2a is the standard error and roughly doubles the
> answer. Note that a rises with **bandwidth** as well as distance (a faster link has smaller Tt), so
> upgrading a link's speed makes stop-and-wait *worse*, not better — the paradox that motivates windowing
> (C78).

**Wrong traces:** B = 1/(1 + a), the round trip counted once · C = the a = 1 value · D = 1/2a, omitting the
transmission time itself.

---

**Q131.** The **sliding-window** protocols exist in order to `[Core]`

- **A)** reduce the number of bits needed in the frame header
- **B)** keep the link busy by allowing several frames to be outstanding at once
- **C)** guarantee that frames are never corrupted in transit
- **D)** eliminate the need for acknowledgements

**Answer: B) keep the link busy by allowing several frames to be outstanding at once**

**Trace / Why:** stop-and-wait wastes 2a time units per frame (C77). If the sender may have **N** frames
outstanding, it can keep transmitting while earlier acknowledgements are still in flight, and the idle time
is filled. Choose N large enough and the link never idles at all.

**📘 CONCEPT — C78 · Window efficiency: N frames per round trip instead of one**
> ```
> efficiency = min( 1 ,  N / (1 + 2a) )
> ```
> The link is **fully utilised** when **N ≥ 1 + 2a** — which is the bandwidth-delay product expressed in
> frames (C7). Below that the window is the bottleneck; above it, extra window size buys nothing.
>
> **Applies when** the stem gives a window size and an a value, or asks what window fills a link.
>
> **Boundary:** the formula caps at 1 — efficiency cannot exceed 100%, so an N larger than 1 + 2a does not
> improve throughput and merely consumes buffers. The **optimal** window is therefore exactly ⌈1 + 2a⌉,
> which is the number most design questions are really asking for (Q137).

**Wrong traces:** A = windowing **adds** header bits for sequence numbers · C = corruption is handled by
error-detecting codes, not by windowing · D = sliding-window protocols depend on acknowledgements.

---

**Q132.** In **Go-Back-N** ARQ with an **m-bit** sequence number field, the maximum sender window size is
`[Applied]` `[Asked: GATE-style]`

- **A)** 2^m − 1
- **B)** 2^m
- **C)** 2^(m−1)
- **D)** 2^(m−1) − 1

**Answer: A) 2^m − 1**

**Trace / Why:** m bits give 2^m distinct sequence numbers. If the window were the full 2^m, a receiver could
not distinguish a **retransmission** of the whole previous window from an entirely new window — the sequence
numbers would repeat exactly. Leaving one number unused breaks the ambiguity, so the window is capped at
**2^m − 1**.

**📘 CONCEPT — C79 · Window limits: Go-Back-N leaves one spare, Selective Repeat needs half**
> | Protocol | Sequence numbers | **Max sender window** | Receiver window |
> |---|---|---|---|
> | Stop-and-wait | 2 (1 bit) | 1 | 1 |
> | **Go-Back-N** | 2^m | **2^m − 1** | **1** |
> | **Selective Repeat** | 2^m | **2^(m−1)** | **2^(m−1)** |
>
> | m | GBN window | SR window |
> |---|---|---|
> | 2 | 3 | 2 |
> | **3** | **7** | 4 |
> | 4 | 15 | **8** |
> | 5 | 31 | 16 |
>
> **Applies when** the stem gives a sequence-number field width and names a protocol.
>
> **Boundary:** the two limits differ because the **receiver windows** differ. GBN's receiver accepts only
> the next in-order frame (window 1), so only the sender's window can overlap ambiguously — one spare number
> suffices. SR's receiver buffers out-of-order frames (window > 1), so sender and receiver windows must not
> overlap at all, forcing the split into halves (Q133). Applying GBN's formula to SR is the commonest error
> in this topic.

**Wrong traces:** B = the full sequence space, which is ambiguous · C = the **Selective Repeat** limit ·
D = neither protocol's limit.

---

**Q133.** In **Selective Repeat** ARQ with a **4-bit** sequence number field, the maximum window size is
`[Applied]` `[Asked: GATE-style]`

- **A)** 8
- **B)** 15
- **C)** 16
- **D)** 32

**Answer: A) 8**

**Trace / Why:**

```
sequence numbers = 2^4 = 16
SR window        = 2^(m−1) = 2^3 = 8
```

Half the sequence space. The reason is that the SR receiver keeps its own window of buffered
out-of-order frames; if the sender's and receiver's windows could overlap, a retransmitted old frame could be
mistaken for a new frame with the same number.

**📘 CONCEPT — C79 (see Concept Index)** › The rule generalises as **W_sender + W_receiver ≤ 2^m**, and since
SR uses equal windows each is at most 2^(m−1). GBN is the same rule with W_receiver = 1, giving
W_sender ≤ 2^m − 1 — so **one formula produces both limits**, which is the tidiest way to remember them.
Distractor B is precisely the GBN answer for m = 4, planted for anyone who reaches for the more familiar
formula.

**Wrong traces:** B = the **Go-Back-N** limit for m = 4 · C = the full sequence space · D = 2^(m+1), no
derivation.

---

**Q134.** The essential difference between **Go-Back-N** and **Selective Repeat** is that on detecting a
lost frame, Go-Back-N `[Trap]`

- **A)** discards the connection and re-establishes it
- **B)** retransmits the lost frame **and all frames sent after it**, whereas Selective Repeat retransmits only the lost frame
- **C)** retransmits only the lost frame, whereas Selective Repeat retransmits the whole window
- **D)** waits for a negative acknowledgement before retransmitting anything

**Answer: B) retransmits the lost frame **and all frames sent after it**, whereas Selective Repeat retransmits only the lost frame**

**Trace / Why:** the GBN receiver accepts frames **only in order** and discards anything out of order, so
every frame after the lost one must be sent again — hence the name. The SR receiver **buffers** out-of-order
frames, so only the missing one needs retransmission.

**📘 CONCEPT — C79 (see Concept Index)** › The retransmission behaviour follows directly from the **receiver
window size**: a receiver window of 1 (GBN) cannot store out-of-order frames, so they must be re-sent; a
receiver window > 1 (SR) can. The trade is therefore **simplicity against bandwidth**: GBN needs only one
buffer and no sorting logic, while SR needs a buffer per window slot, per-frame timers and reordering — but
wastes far less capacity on a lossy link. That is why GBN suits low-error links and SR suits noisy ones.

**Wrong traces:** A = neither protocol tears down the connection · C = the two protocols **swapped** — the
intended trap · D = both use timeouts; NAKs are an optimisation, not a requirement.

---

**Q135.** A Go-Back-N protocol uses a window of **N = 5** on a link where **a = 4**. Its efficiency is
`[Applied]` `[Asked: GATE-style]`

- **A)** 11.1%
- **B)** 33.3%
- **C)** 55.6%
- **D)** 100%

**Answer: C) 55.6%**

**Trace / Why:**

```
efficiency = min( 1 , N / (1 + 2a) )
           = min( 1 , 5 / (1 + 8) )
           = 5 / 9
           = 0.556 = 55.6%
```

Compare Q130: the same link under stop-and-wait gives 11.1%. Widening the window from 1 to 5 multiplied
throughput by five. Full utilisation would need N ≥ 1 + 2a = **9**.

**📘 CONCEPT — C78 (see Concept Index)** › Three quantities travel together in these questions and it is
worth computing all three: **a** (from the link), **1 + 2a** (the window needed for 100%), and **N/(1 + 2a)**
(the efficiency actually achieved). Here they are 4, 9 and 5/9. Note that efficiency is **linear in N** until
it saturates, so doubling a small window doubles throughput — which is exactly why the window, not the
bandwidth, is the thing to increase on a high-delay link.

**Wrong traces:** A = the stop-and-wait value, N = 1 · B = N/(1 + 2a) with a = 1 · D = would require N ≥ 9.

---

**Q136.** **Piggybacking** in a bidirectional data link protocol means `[Core]`

- **A)** carrying an acknowledgement for received data inside an outgoing data frame
- **B)** sending two copies of each frame for reliability
- **C)** transmitting on two channels simultaneously
- **D)** appending a CRC to every frame

**Answer: A) carrying an acknowledgement for received data inside an outgoing data frame**

**Trace / Why:** when both ends send data, a separate acknowledgement frame is wasteful — it carries a full
header for a few bits of control information. Piggybacking places the acknowledgement in a field of a data
frame that was going that way anyway, so one frame does two jobs.

**📘 CONCEPT — C80 · Piggybacking saves frames, at the cost of a timing decision**
> Instead of `[DATA →] [← ACK] [← DATA]`, send `[DATA →] [← DATA+ACK]`. Fewer frames means less header
> overhead and less medium contention.
>
> **Applies when** the stem describes acknowledgements combined with data, or asks how bidirectional
> efficiency is improved.
>
> **Boundary:** the complication is **how long to wait** for outgoing data to piggyback on. Wait too long and
> the sender times out and retransmits unnecessarily; send a standalone ACK immediately and the saving is
> lost. Real protocols use a short **delayed-ACK timer** to resolve it — TCP does exactly this, and it is why
> TCP acknowledgements are sometimes deferred by up to 500 ms.

**Wrong traces:** B = duplication, unrelated · C = full-duplex transmission (C2) · D = error detection
(C73), which every frame does anyway.

---

**Q137.** On a link with **a = 4**, the minimum number of bits needed in the sequence number field for a
Go-Back-N protocol to achieve **100%** utilisation is `[Applied]`

- **A)** 4
- **B)** 5
- **C)** 8
- **D)** 10

**Answer: A) 4**

**Trace / Why:** work in three steps.

```
1. window needed for full utilisation:  N ≥ 1 + 2a = 1 + 8 = 9
2. Go-Back-N requires  N ≤ 2^m − 1,  so  2^m − 1 ≥ 9  ⇒  2^m ≥ 10
3. smallest m with 2^m ≥ 10:  m = 4  (2^4 = 16 ≥ 10)  ✔   m = 3 gives 8 < 10  ✘
```

So 4 bits, providing 16 sequence numbers and a maximum window of 15 — comfortably above the 9 required.

**📘 CONCEPT — C81 · Sizing the sequence-number field: window first, then the protocol's cap, then log₂**
> ```
> 1. N_required = 1 + 2a                    (from C78)
> 2. apply the protocol cap:  GBN → N ≤ 2^m − 1 ;  SR → N ≤ 2^(m−1)
> 3. solve for the smallest integer m
> ```
> **Applies when** the stem asks for the number of sequence-number **bits**, rather than the window size.
>
> **Boundary:** the answer differs by protocol for the same link — with N = 9, GBN needs 2^m ≥ 10 so m = 4,
> while **Selective Repeat** needs 2^(m−1) ≥ 9 so 2^m ≥ 18 and **m = 5**. Reading GBN where the paper wrote
> SR therefore changes the answer, and distractor B is exactly the SR value.

**Wrong traces:** B = the **Selective Repeat** requirement for the same link · C = the window size 9 rounded
up to 8 bits · D = the required window itself, mistaken for a bit count.

---

**Q138.** In protocol efficiency analysis, the parameter **a** is defined as `[Applied]`

- **A)** propagation time divided by transmission time
- **B)** transmission time divided by propagation time
- **C)** bandwidth divided by delay
- **D)** window size divided by round-trip time

**Answer: A) propagation time divided by transmission time**

**Trace / Why:**

```
a = Tp / Tt = (distance / speed) / (frame size / bandwidth)
```

It measures how many frame-times fit in the propagation delay. Small a means a short, slow link where the
frame occupies the wire; large a means a long, fast link where many frames could be in flight at once.

**📘 CONCEPT — C77 (see Concept Index)** › Because a = Tp/Tt, it **rises with distance and with bandwidth**
and **falls with frame size**. Two consequences that questions test: a fast long-haul link has a huge a and
therefore desperately needs a large window (C78), and **using larger frames reduces a**, which is a second
way to improve efficiency besides widening the window. Inverting the ratio (option B) makes every efficiency
answer wrong in a systematic way, so it is worth writing the definition down before substituting.

**Wrong traces:** B = the reciprocal, which inverts every subsequent result · C = the bandwidth-delay product
(C7), a related but different quantity · D = not a standard parameter.

---

**Q139.** In **Selective Repeat**, the receiver window `[Trap]`

- **A)** is always 1, as in Go-Back-N
- **B)** is greater than 1, so out-of-order frames can be buffered until the missing one arrives
- **C)** must equal the full sequence-number space
- **D)** is irrelevant, since the receiver accepts every frame it receives

**Answer: B) is greater than 1, so out-of-order frames can be buffered until the missing one arrives**

**Trace / Why:** the SR receiver keeps a window of buffers, typically the same size as the sender's window.
It accepts and stores any frame falling inside that window, even out of order, acknowledges it individually,
and delivers to the network layer only when the gap is filled. That buffering is precisely what allows the
sender to retransmit **only** the missing frame (Q134).

**📘 CONCEPT — C79 (see Concept Index)** › The receiver window is the **root cause** of every difference
between the two protocols, and tracing the chain is the best way to remember them: receiver window > 1 ⇒
out-of-order frames can be kept ⇒ only the lost frame need be re-sent ⇒ individual acknowledgements are
required ⇒ sender and receiver windows must not overlap ⇒ the window cap is 2^(m−1) rather than 2^m − 1
(Q133). Every SR fact follows from that one design decision.

**Wrong traces:** A = **Go-Back-N's** receiver window, which is what forces its wasteful retransmission ·
C = would make sequence numbers ambiguous · D = the window defines exactly which frames are acceptable.

---

**Q140.** In stop-and-wait ARQ, if an **acknowledgement is lost** but the frame arrived correctly, the
sender's timeout causes a retransmission. The receiver detects the duplicate by `[Applied]`

- **A)** comparing the CRC of the new frame with the previous one
- **B)** asking the sender to confirm whether the frame is new
- **C)** checking the frame's sequence number, which repeats the one already accepted
- **D)** noting that the frame arrived sooner than expected

**Answer: C) checking the frame's sequence number, which repeats the one already accepted**

**Trace / Why:** the receiver expects the next sequence number. A frame carrying the sequence number it has
**already accepted** must be a duplicate caused by a lost acknowledgement, so the receiver discards the data
and re-sends the acknowledgement. One bit of sequence number is sufficient, since only two states must be
distinguished.

**📘 CONCEPT — C82 · Sequence numbers exist to detect duplicates, not only to order frames**
> Four things can go wrong, and the recovery for each:
> | Event | Detected by | Recovery |
> |---|---|---|
> | Frame corrupted | CRC fails | discard; sender times out and resends |
> | Frame lost | no ACK arrives | sender times out and resends |
> | **ACK lost** | sender times out | resend; **receiver spots the duplicate sequence number** |
> | ACK delayed | sender times out early | as above — duplicate discarded |
>
> **Applies when** the stem describes a lost or delayed acknowledgement, or asks why a 1-bit sequence number
> is needed at all.
>
> **Boundary:** this is the reason stop-and-wait **cannot** work with no sequence number: without it the
> retransmitted frame would be accepted as new data and the receiver's byte stream would contain a duplicate.
> So the sequence number's duplicate-detection role is more fundamental than its ordering role here — with
> only one frame outstanding there is no ordering to do.

**Wrong traces:** A = a duplicate has a **valid** CRC, so the CRC cannot distinguish it · B = no such
confirmation exchange exists · D = arrival timing is not used for duplicate detection.

---
