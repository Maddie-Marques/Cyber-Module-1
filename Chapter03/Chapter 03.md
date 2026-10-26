# *<u>Chapter 3 — Networking Connectors & Wiring Standards:</u>*

## *<u>Coaxial Cable:</u>*

Coax is a single copper center conductor, wrapped in a plastic insolation layer, with a braided shield over that, then an outer jacket of PVC or a Teflon-type plenum-rated coating. It's big advantage: the braided shielding resists EMI/RFI (electromagnatic/radio frequency interference) and other stray signals.

Plenum vs. non-plenum: the plenum is the space between floors in multistory buildings used to circulate air, and often carries cabling too. Fire code (NFPA, National Fire Protection Association) requires plenum-rated cable there, because regular (non-plenum) insulation gives off poisonous smoke and can help fire spread like a wick. Non-plenum is cheaper, so it's fine anywhere that isn't a plenum space.

<table>
<colgroup>
<col style="width: 22%" />
<col style="width: 15%" />
<col style="width: 12%" />
<col style="width: 14%" />
<col style="width: 18%" />
<col style="width: 17%" />
</colgroup>
<thead>
<tr class="header">
<th>Type</th>
<th>Standard</th>
<th>Cable</th>
<th>Size</th>
<th>Connector</th>
<th>Status</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Thinnet</td>
<td>10Base2</td>
<td>RG-58</td>
<td>~5mm</td>
<td>BNC with 50-ohm terminating resisters on both ends</td>
<td>Essentially obsolete for LANs</td>
</tr>
<tr class="even">
<td>Thicknet</td>
<td>10Base5</td>
<td>RG-8</td>
<td>~1/2 inch</td>
<td>Worked the same way</td>
<td>Basically extinct</td>
</tr>
<tr class="odd">
<td>Cable TV cable/modem</td>
<td></td>
<td><p>RG-59</p>
<p>Or</p>
<p>RG-6</p></td>
<td></td>
<td>F-type screw-on connector</td>
<td>Where coax survives today.</td>
</tr>
</tbody>
</table>

## *<u>Twisted-Pair: UTP vs. STP, and Categories:</u>*

Twisted-pair is multiple individually insulated wires twisted together in pairs. The twists are there specifically to reduce cross-talk (interference from nearby signals) and protect against outside interference.

- UTP (Unshielded Twisted Pair): What you'll touch every day. Used in Ethernet standards like 10BaseT through 40GBaseT. It wins today because it's cheap, easy to work with, and support speeds that weren't possible a decade ago.

- STP (Shielded Twisted Pair): adds a metal foil or braid around each pair for better noise immunity in electrically noisy environments.

### Categories

Each higher category is rated for a higher frequency and has more twists per inch.

| Category | Frequency | Speed/use                                                                                                          | Status                                                                         |
|----------|-----------|--------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------|
| Cat 1    | 1 MHz     | Voice only (POTS, Plain Old Telephone Service). Not rated for dat                                                  | Voice only                                                                     |
| Cat 2    | 10 MHz    | 4 Mbps                                                                                                             | Obsolete                                                                       |
| Cat 3    | 16 MHz    | 10 Mbps Ethernet                                                                                                   | Obsolete for networks                                                          |
| Cat 4    | 20 MHz    |                                                                                                                    | Obsolete                                                                       |
| Cat 5    | 100 MHz   | 100BaseTX (two-pair)                                                                                               | Largely replaced by Cat 5e                                                     |
| Cat 5e   | 100 MHz   | Gigabit. Handles crosstalk from all four pairs transmitting at once.                                               | Practical minimum or any modern install                                        |
| Cat 6    | 250 MHz   | Gigabit comfortably. Standard since June 2002                                                                      | Go-to for new buildings. Common as riser cable between floors, alongside fiber |
| Cat 6A   | 500 MHz   | Better crosstalk handling lets 10GBaseT run the full 100m (regular Cat 6 has a shorter max distance at that speed) | Current                                                                        |
| Cat 7    |           | 10 Gigabit Ethernet over 100m of copper                                                                            | Current                                                                        |
| Cat 8    |           | 25G/40G at 30m                                                                                                     | Built for data centers                                                         |

### Practical Tips

- Yanking Cat 5e stretches its internal twists and can invalidate its rating. Cat 6 has a plastic spine to resist that, and Cat 7/8 add metal shielding.

- Always test and connect all four pairs, even though older setups used only two. Gigabit needs all eight wires in good shape.

- A true Cat 5e/6/7/8 install means every component end-to-end (patch cables, wall panel, patch panel) meets spec. One mismatched piece breaks the whole certification, and verifying it takes pricey test gear.

## *<u>RJ Connectors:</u>*

BNC connectors don't fit UTP, so RJ (registered jack) connectors are used instead. They're attached with a crimper, just with a different die shape than BNC.

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 34%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Connector</th>
<th>Positions / Wires</th>
<th>Used For</th>
<th>Notes</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>RJ-11</td>
<td><p>Small 4/6-position jack.</p>
<p>Four wires (two pairs)</p></td>
<td>Phone lines and home DSL (Digital Subscriber Line)</td>
<td>Never used for LANs</td>
</tr>
<tr class="even">
<td>RJ-45</td>
<td>8-position jack. Eight wires (four pairs)</td>
<td>Ethernet data</td>
<td>Unshielded wiring</td>
</tr>
<tr class="odd">
<td>RJ-48c</td>
<td>Looks identical to the RJ-45</td>
<td>T1 circuits (long-distance WAN), not short LAN runs</td>
<td>Wired differently, and the wires are typically shielded. Don't mix them up by sight alone.</td>
</tr>
</tbody>
</table>

## *<u>Straight-Through, Crossover, Rolled Cable & T568A/B:</u>*

### T568A vs. 1568B:

Over 60 vendors agreed on two wiring standards, T568A and T568B, though not universally, so you'll find both in the wild. The entire difference: pair 2 (orange) and pair 3 (green) swap positions.

- T568A: green on pins 1—2,orange split accross pins 3 and 6.

<img src="./media/image1.png" style="width:5.31874in;height:3.35414in" />

- T568B: those two pairs swapped (orange on 1—2, green one on 3 and 6).

<img src="./media/image2.png" style="width:5.35872in;height:3.78163in" />

- Pins 4,5,7,8 (blue and brown pairs) never move. They're needed for 1000 Base T, PoE (Power over Ethernet), and some 100 Mbps setups.

- Since VoIP (Voice over IP) commonly shares the same wire as data today, new installs should always collect and use all eight pins.

### Three Cable Types:

<table style="width:100%;">
<colgroup>
<col style="width: 19%" />
<col style="width: 30%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Cable</th>
<th>Connects</th>
<th>Wiring</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Straight-through</td>
<td>Unlike devices: host to switch/hub, or router to switch/hub</td>
<td><p>Same standard on both ends</p>
<p>(T568A + T568A, or T568B + T568B).</p>
<p>For basic 10/100 Ethernet only pins 1, 2, 3, 6 are used, wired 1-to-1, 2-to-2, and so on.</p>
<p>A Gigabit straight-through is still 1-to-1 through 8-to-8</p>
<p><img src="./media/image3.png" style="width:3.86669in;height:0.96497in" /></p></td>
</tr>
<tr class="even">
<td>Crossover</td>
<td><p>Like devices: switch-switch,</p>
<p>hub-hub,</p>
<p>host-host,</p>
<p>hub-switch, or</p>
<p>router direct to host.</p></td>
<td><p>T568A on the one end, T568B on the other.</p>
<p>Pin 1 goes to pin 3 and pin 2 to pin 6, on one end only</p>
<p><img src="./media/image4.png" style="width:3.86555in;height:0.96775in" /></p></td>
</tr>
<tr class="odd">
<td>Rolled / rollover</td>
<td>A host's serial interface to a Cisco router or switch console port, for management access</td>
<td><p>Carries no Ethernet at all.</p>
<p>Easiest to make: flip the connector on one end of a straight-through cable and reattach it</p></td>
</tr>
</tbody>
</table>

For Gigabit (1000BaseT), all four pairs (all 8 pins) are used simultaneously, not just two pairs.

### Crossover :

- Always label a crossover cable clearly so no one mistakes it for a regular patch cable. Plugged in wrong, the workstation can't talk to the network.

- A cable tester can confirm which type you're holding, or flag a bad cable.

- Handy trick: carry a crossover cable to test a server's NIC (Network Interface Card) directly from your laptop without going through a switch.

T1 crossover cable: niche, used only to connect to CSU/DSU (Channel Service Unit/Data Service Unit) devices back-to-back. It swaps pins 1/2 with pins 4/5 rather than using the Ethernet crossover point.

<img src="./media/image5.png" style="width:4.95112in;height:4.00798in" />

## *<u>Fiber-Optic Cable & Connectors:</u>*

Fiber transmits digital signals as light rather than electricity, which makes it immunebu EMI/RFI.

Construction, inside out: a glass or plastic core (glass carries father, plastic is cheaper), then cladding that reflects light back into the core, then a flexible buffer, sometime Kevlar armour, then a PVC or plenum jacket.

- Pros: totally immune to EMI/RFI, and can run up to ~40 km C

- Cons: harder to install, more expensive (cable and test equipment), and harder to troubleshoot.

### <u>Single-Mode vs. Multimode:</u>

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 42%" />
<col style="width: 43%" />
</colgroup>
<thead>
<tr class="header">
<th></th>
<th>SMF (Single-Mode Fiber)</th>
<th>MMF (Multimode Fiber)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Light</td>
<td>One laser-driven ray (one or two glass strands, with LED or laser sources)</td>
<td>Multiple LED-driven rays, dispersed across multiple paths and reflected back by the cladding</td>
</tr>
<tr class="even">
<td>Distance</td>
<td><p>Long runs.</p>
<p>Up to 90x farther than multimode, 40-80km depending on the transceiver</p></td>
<td><p>Medium runs, up to ~3000ft.</p>
<p>Gets unreliable beyond that</p></td>
</tr>
<tr class="odd">
<td>Speed</td>
<td>Faster</td>
<td>Great with bandwidth over medium distances</td>
</tr>
<tr class="even">
<td>Cost / install</td>
<td><p>Trickier.</p>
<p>Shouldn't be crimped or bent sharply</p></td>
<td><p>Cheaper.</p>
<p>Comes in glass or an easier-to-install plastic version.</p></td>
</tr>
<tr class="odd">
<td>Typical use</td>
<td>Between buildings</td>
<td>Within one building</td>
</tr>
</tbody>
</table>

### Connectors

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 41%" />
<col style="width: 42%" />
</colgroup>
<thead>
<tr class="header">
<th>Connector</th>
<th>What it is</th>
<th>Notes</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>ST (straight tip)</td>
<td>AT&amp;T designed, bayonet twist-lock (like BNC)</td>
<td><p>Popular for SMFWAN links.</p>
<p>won't loosen over time.</p></td>
</tr>
<tr class="even">
<td>FC</td>
<td>Similar to ST, but with a screw mechanism</td>
<td>less common</td>
</tr>
<tr class="odd">
<td>SC</td>
<td><p>Square, push-on/off.</p>
<p>Nicknamed “Square Charlie”</p></td>
<td>Popular in MMF installs, but larger and being phased out for smaller connectors</td>
</tr>
<tr class="even">
<td>MT-RJ</td>
<td><p>SSF (small form factor).</p>
<p>One-third the size of SC/ST, combines TX/RX in one connector</p></td>
<td><p>keyed for single polarity.</p>
<p>No polishing or epoxy needed</p></td>
</tr>
<tr class="odd">
<td>LC</td>
<td><p>SFF.</p>
<p>Resembles an RJ connector with a spring-loaded detent.</p></td>
<td><p>Smaller for higher port density.</p>
<p>Popular for Fibre Channel and Gigabit Ethernet.</p>
<p>Can't be disassembled to swap TX/RX like SC can</p></td>
</tr>
<tr class="even">
<td>MPO</td>
<td>High-density SFF connector, fitting 2,8,12, or 24 fibers in one plug</td>
<td>Typicallyfans out to multiple LC connectors for 10-100 Gbps per connection</td>
</tr>
</tbody>
</table>

SFF connectors pack more terminations into less space.

### APC vs. UPC:

These aren't connectors but polish/finish types. They affect return loss (decibel loss) from reflectance, not the core light path.

- UPC (Ultra Physical Contact): straight cut. Reflects some light into the cladding instead of the core, causing loss. Colour-coded blue.

- APC (Angled Physical Contact): angled cut that significantly reduces that loss. Colour-coded green.

### Related Bits:

- FDP (fiber distribution panel): handles fiber termination and distribution, with a cable tray and splice drawer.

- Fibre Channel (FC): a seperate high-speed (2-32 Gbps) storage transfer protocol used for SANs (Storage Area Networks). It delivers raw data losslessly, doesn't follow the OSI model, and uses different transceivers than Ethernet optical modules.

### Copper vs. Fiber:

- Distance: fiber wins for multi-mile runs. Copper tops up around 1500 ft before needing signal regeneration, and the UTP standard maxes at .

- Security: fiber wins for high-security needs, since it doesn't create a readable magnetic field.

##  *<u>Twinaxial Cable & DAC</u>*

(Textbook only, not mentioned in study guide.)

- Twinaxial (twinax) cable is used for short-distance, high-speed connections like 10G/40G Ethernet in data centers. It's copper-based, so it's significally cheaper than fiber for runs of 10 meters or less.

- DAC (Direct Attach Copper) is part of the twinax family: fixed-length (~26-28 AWG, American Wire Gauge) cables with connectors on both ends for direct device-to-device communication. It uses electromagnetic shielding (like STP) to boost speed and reliability.

## *<u>Transceivers & Media Converters:</u>*

(Textbook only, not mentioned in Study Guide)

A transceiver combines a transmitter and receiver in one housing. They're used in everything from cell phones to fiber-optic systems.

| Module                            | What it is                                                               | Speed           |
|-----------------------------------|--------------------------------------------------------------------------|-----------------|
| SFP (Small Form-factor Pluggable) | Compact optical transceiver module                                       |                 |
| SFP+                              | Enhanced SFP                                                             | Up to 16 Gbit/s |
| QSFP (Quad SFP)                   | Hot-pluggable transceiver connecting network hardware to fiber or copper | 4 x 1 Gbps      |
| QSFP+                             | Enhanced QSFP                                                            | 4 x 10 Gbps     |
| QSFP28                            | Used for 100 Gbps links                                                  | 4 x 28 Gbps     |

Fiber transceivers come in two flavours:

- : data flows one way only.

- Bidirectional: data flows both way simultaneously over a single strand by seperating the two devices' transmission wavelengths (per IEEE 802.3ah).

Media converters handle switching between media types:

- Single-mode fiber to Ethernet

- Multimode fiber to the Ethernet

- Fiber to coaxial

- Single-mode to multimode fiber

## *<u>Serial Cables (RS-232, DB-25, USB):</u>*

(Textbook only, not mentioned in Study Guide)

Serial communication sends one bit after another. Parallel sends bits in groups like the old Centronics printer cable, now replaced by USB.

- RS-232 (Recommended standard 232): a common serial standard connecting DTE and DCE (Data Terminal Equipment and Data Communication Equipment, like a computer to an external modem), typically via a DB-9 connector. Mostly obsolete now, replaced by USB/USB-C.

- DB-25: from the D-series connectors invented in 1952. It has 25 pins in a “B” size shell and was RS-232's usual connector. Rarely seen today. Even DB-9, once commonly used for Cisco console cables, has mostly been replaced with USB.

- USB (Universal Serial Bus): now the standard serial bus on motherboards. Typically 4 external ports, expandable via hubs, technically supporting up to 127 devices (rarely practical). Used for printers, scanners, keyboards, mice, and more.

## *<u>Cable Properties (Speed, Distance, Duplex, Noise Immunity, Frequency):</u>*

Every cable choice trades off these five things:

<table>
<colgroup>
<col style="width: 49%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Property</th>
<th>What to know</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Speed</td>
<td>Varies by network area. Core areas often run 10Gbps +. Distribution/access area (where users connect) typically run 100/1000 Mbps, through demand keeps pushing that up</td>
</tr>
<tr class="even">
<td>Distance</td>
<td>All cables suffers attenuation (signal degradation over distance). Twisted-pair maxes out at 328 ft (100 m) per segment</td>
</tr>
<tr class="odd">
<td>Duplex</td>
<td><p>Half-duplex: can't send and receive at once, like a walkie-talkies.</p>
<p>Full-duplex: send and receive simultaneously, doubling effective throughput.</p>
<p>Full-duplex is standard in modern switched networks.</p></td>
</tr>
<tr class="even">
<td>Noise immunity</td>
<td>Two magnetic-flux issues (below)</td>
</tr>
<tr class="odd">
<td>Frequency</td>
<td>Sets a cable's max bandwidth. Cat 5e tops out at 100 MHz (fine for 1Gbps over short runs). Cat 6 handles 250 MHz and 1 Gbps comfortably, so it's the better choice for longer runs like between floors.</td>
</tr>
</tbody>
</table>

### Noise immunity, the two issues:

1.  Tapping: Copper's current can be “tapped” and read without even cutting the cable. STP makes this harder but not impossible. Fiber carries light, not electricity, so it's far more tap-resistant: tapping it means physically cutting and repairing the cable, which tends to get noticed.

2.  External EMI: Motors, speakers, and ballasts can introduce unwanted current in nearby cupper, so keep cabling away from them.

## *<u>Wiring Distribution: MDF/IDF, Blocks, Demarc:</u>*

On the building side signals pass through an MDF/IDF, terminate on a 66 or 110 block, and hand off to the provider at the demarc, the exact line between “your problem” and “their problem” when troubleshooting.

### Distribution Frames:

- MDF (Main Distribution Frame): the building's central wiring reference point and WAN termination point, where internal lines connect to external (telco) lines.

- IDF (Intermediate Distribution Frame): connects to the MDF from an equipment/telecom room, giving more flexible distribution throughout the building.

- 25-pair cable (aka feeder cable): bundles 25 wire pairs under one jacket. Mainly for telephone backbone/cross-connecting cabling, cutting down on clutter.

### Punch-Down Blocks

<table>
<colgroup>
<col style="width: 11%" />
<col style="width: 43%" />
<col style="width: 44%" />
</colgroup>
<thead>
<tr class="header">
<th>Block</th>
<th>What it is</th>
<th>Notes</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>66 block</td>
<td><p>Old analog telephone termination standard (from 1962).</p>
<p>Uses 25-pair cable across 50 rows.</p></td>
<td>Mostly in older buildings</td>
</tr>
<tr class="even">
<td>110 block</td>
<td><p>The modern replacement</p>
<p>Used for both phone (RJ-11) and network (RJ-45) connections.</p></td>
<td><p>Sizes from 25 to 500+ pairs.</p>
<p>Supports 1 Gbps with Cat 6+ (though Cat 6's size makes a tight fit)</p></td>
</tr>
<tr class="odd">
<td>Krone</td>
<td>European equivalent</td>
<td>110-block-compatible</td>
</tr>
<tr class="even">
<td>BIX block</td>
<td>Another punch-down type, terminating up to 25 pairs</td>
<td>lip-in fitting, no pre-stripping of wires</td>
</tr>
</tbody>
</table>

### The Demarc:

- Demarc (demarcation point): the exact point where the service provider's responsibility ends. Often at the MDF, usually just an RJ-45 jack where your CSU/DSU connects to the WAN.

- Admins test both sides of the demarc to figure out if a problem is internal or the provider's fault.

- Demarc extension: the cabling between the demore and your actual office space.

- Smart jack (aka NID/NIU, Network Interface Device/Unit): a provider-owned interface, often installed because you can't directly test the bare demarc jack. It can be powered and looped for testing, and may also handle code/protocol conversion so the provider's signal works with your internal gear.

## *<u>Core Insights:</u>*

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 74%" />
</colgroup>
<thead>
<tr class="header">
<th>Concept</th>
<th>The thing to remember</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Coax</td>
<td><p>Braided shield resists EMI/RFI.</p>
<p>Thinnet/thicknet (BNC) are obsolete.</p>
<p>Survives as RG-6 /RG-59 with F-type for cable modems.</p></td>
</tr>
<tr class="even">
<td>Plenum</td>
<td><p>Air space between floors.</p>
<p>Fire code requires plenum-rated cable there.</p>
<p>(non-plenum gives off poisonous smoke)</p></td>
</tr>
<tr class="odd">
<td>UTP vs. STP</td>
<td><p>Twists cut crosstalk.</p>
<p>STP adds foil/braid for noisy environments.</p></td>
</tr>
<tr class="even">
<td>Categories</td>
<td><p>Cat 5e = practical minimum (100 MHz, Gigabit).</p>
<p>Cat 6 = 250 MHz.</p>
<p>Cat 6A = 500 MHz, 10GBaseT at 100m.</p>
<p>Cat 8 = 25G/40G at 30m.</p></td>
</tr>
<tr class="odd">
<td>Certification</td>
<td>Every component end-to-end must meet the category specs.</td>
</tr>
<tr class="even">
<td>RJ connectors</td>
<td><p>RJ-11 = phone/DSL.</p>
<p>RJ-45 = Ethernet.</p>
<p>RJ-48c looks like an RJ-45, but it is for T1.</p></td>
</tr>
<tr class="odd">
<td>T568A vs. T568B</td>
<td><p>Only the orange and green pair swap.</p>
<p>Same on both ends = straight-through.</p>
<p>A + B = crossover.</p></td>
</tr>
<tr class="even">
<td>Cable types</td>
<td><p>Straight-through = unlike devices.</p>
<p>Crossover = like devices.</p>
<p>Rollover = Cisco console, no Ethernet.</p></td>
</tr>
<tr class="odd">
<td>SMF vs. MMF</td>
<td><p>SMF = laser, long distance, between buildings.</p>
<p>MMF = LED, cheaper, within a building.</p></td>
</tr>
<tr class="even">
<td>Fiber connectors</td>
<td><p>ST(twist),</p>
<p>SC (square, push-pull),</p>
<p>LC (small, RJ-like),</p>
<p>MT-RJ (TX/RX in one),</p>
<p>MPO (up to 24 fibers).</p></td>
</tr>
<tr class="odd">
<td>APC vs. UPC</td>
<td><p>Polish types.</p>
<p>APC = angled, green, less loss.</p>
<p>UPC = straight blue.</p></td>
</tr>
<tr class="even">
<td>Twinax / DAC</td>
<td>Cheap copper for ≤ 10m high-speed data center links.</td>
</tr>
<tr class="odd">
<td>Transceivers</td>
<td><p>SFP -&gt; SFP + (16 Gbit/s).</p>
<p>QSFP (4x1) -&gt; QSFP+ (4x10) -&gt; QSFP28 (4x28, 100G).</p>
<p>Bidirectional = one strand, two wavelengths.</p></td>
</tr>
<tr class="even">
<td>Serial</td>
<td><p>RS-232 with DB-9/DB-25, now replaced by USB</p>
<p>(up to 127 devices)</p></td>
</tr>
<tr class="odd">
<td>Cable proporties</td>
<td><p>Speed,</p>
<p>distance (UTP = 328 ft/100m),</p>
<p>duplex,</p>
<p>noise immunity (fiber resists tapping),</p>
<p>frequency.</p></td>
</tr>
<tr class="even">
<td>Wiring distribution</td>
<td><p>MDF (main, WAN termination) -&gt; IDF.</p>
<p>66 block = old phone.</p>
<p>110 block = modern.</p>
<p>Demarc = where the provider's responsibility ends.</p>
<p>Smart jack = loophole test point.</p></td>
</tr>
</tbody>
</table>

## Abbreviations: 

| **Abbreviation** | **Stands for**                                    |
|------------------|---------------------------------------------------|
| **APC**          | Angled Physical Contact                           |
| **AWG**          | American Wire Gauge                               |
| **BIX**          | Building Industry Cross-connect                   |
| **BNC**          | Bayonet Neill–Concelman                           |
| **Cat**          | Category                                          |
| **CSU/DSU**      | Channel Service Unit/Data Service Unit            |
| **DAC**          | Direct Attach Copper                              |
| **DB**           | D-subminiature (D-series) connector               |
| **DCE**          | Data Communication Equipment                      |
| **DSL**          | Digital Subscriber Line                           |
| **DTE**          | Data Terminal Equipment                           |
| **EMI**          | Electromagnetic Interference                      |
| **FC**           | Fibre Channel (also the FC fiber connector)       |
| **FDP**          | Fiber Distribution Panel                          |
| **IDF**          | Intermediate Distribution Frame                   |
| **IEEE**         | Institute of Electrical and Electronics Engineers |
| **LC**           | Lucent Connector (local connector)                |
| **MDF**          | Main Distribution Frame                           |
| **MMF**          | Multimode Fiber                                   |
| **MPO**          | Multi-fiber Push-On                               |
