# ***<u>CHAPTER 2 — The OSI Model</u>***

## **<u>Why Layers At All?</u>**

Back in the day, networks only balked to networks from the **same manufacturer:** all-DECnet or all-IBM, never mixed. The **OSI (Open Systems interconnection) model** was created by **ISO (International Organisation for Standardisation)** in the late '70s to break that barrier. It gave vendor a shared blueprint so different systems could interoperate. Think of it as the “world peace” of networking: nice good, imperfect in reality.

**Reference model:** A *conceptual* blueprint that splits communication into logical groupings called **layers**. It's not physical. It's a set of guidelines developers use to build networked apps.

Layers work like departments in a company. Each one does its own job and trusts the others to handle theirs. This **binding** means a developer working on one layer's protocol only has to worry about that layer's functions. Change how one layer works and the others don't need to know or care. That isolation is what makes multi-vendor gear play nicely together.

### **<u>Benefits of layering:</u>**

- Breaks communication into smaller, simpler pieces, so design and troubleshooting are easier.

- Enables multi-vendor development.

- Pushes industry standardisation.

- Let's different hardware and softwhere talk to each other.

- **Changes in one layer don't ripple into the others** (the big one).

**The seven layers, top to bottom**. Mnemonic: *Please Do Not Throw Sausage Pizza Away* (read it bottom-up).

| **\#** | **Layer**    | **Group**                                                                                            |
|--------|--------------|------------------------------------------------------------------------------------------------------|
| **7**  | Application  | **Upper 3:** how apps talk to users and each other. They know nothing about networking or addresses. |
| **6**  | Presentation | Upper 3                                                                                              |
| **5**  | Session      | Upper 3                                                                                              |
| **4**  | Transport    | Lower 4: actually getting data from end to end.                                                      |
| **3**  | Network      | Lower 4                                                                                              |
| **2**  | Data Link    | Lower 4                                                                                              |
| **1**  | Physical     | Lower 4                                                                                              |

## **<u>The Upper Three: Application, Presentation, Session</u>**

### **Application Layer (Layer 7)**

This is where users interact with the network, but technically through **application processes/API (Application Programming interfaces)**, not the app itself. It checks whether a communication partner is available and whether there are enough resources to connect.

Key distinction: apps like Chrome or Word **don't live at Layer 7**. They only use Application-layer protocols when they need a remote resources. Strip all networking off a machine and Chrome still opens a local HTML file fine. Only things life HTTPS (Hypertext Transfer Protocol Secure) or FTP (File Transfer Protocol) requests need the Application layer.

### Presentation Layer (Layer 6)

This layer is the **translator**. It presents data to the Application layer in a standard format, so data from one system's format makes sense on the other's. Examples: **EBCDIC Extended Binary Coded Decimal Interchange Code)** **—\> ASCII (American Standard Code for Information Interchange)**, or Unicode —\> ASCII.

It also handles:

- **Compression and decompression**.

- **Encryption and decryption**.

- Some multimedia standards.

### Session Layer (Layer 5)

This layer **sets up, manage, and tears down sessions** between Presentation-layer entities. It keeps one app's data separate from another's, which is how you can run multiple browser sessions without them crossing wires. It also controls the dialogue between devices through three modes:

| **Mode**        | **Direction**                  |
|-----------------|--------------------------------|
| **Simplex**     | One direction only             |
| **Half-duplex** | Both directions, one at a time |
| **full-duplex** | Both directions simultaneously |

## Transport Layer (Layer 4): TCP vs. UDP, Flow Control & Windowing

### The Transport layer:

- **segments and reassembles** data into a stream

- provides **end-to-end transport**

- can set up a **logical connection** between sender and receiver

- **multiplexes** upper-layer apps and hides network details from them.

> Developers pick the protocol that fits their app:

|                  | **TCP (Transmission Control Protocol)** | **UDP (User Datagram Protocol)** |
|------------------|-----------------------------------------|----------------------------------|
| Type             | Connection-orientated                   | Connectionless                   |
| Virtual circuit? | Yes                                     | No, just sends                   |
| Reliable?        | Yes: sequencing, ACKs, flow control     | No                               |
| Overhead         | Higher (setup costs)                    | Lower                            |

### Connection-Orientated Communication & the Three-Way Handshake

Before sending anything, the sender's TCP process contacts the receivers process to build a **virtual circuit**. That setup is the *overhead* of connection-orientated communication. Both sides also agree up front on how much data can be sent before an acknowledgement is needed.

### Three-way handshake (SYN, SYN/AC, ACK)

1.  **SYN**: sender asks to synchronize.

2.  **SYN/AC**: receiver acknowledges and asks for it's own sync, making it bidirectional.

3.  **ACK**: sender confirms. Data transfer begins.

A connection-orientated service = **virtual circuit setup + sequencing + acknowledgements + flow control**.

### Congestion & Flow Control

**Congestion** happens when a faster senderfloods a slower network, or when lots of senders pile onto one destination or gateway. Think too many care on one road.

**Flow control** lets the *receiver* govern how much the sender pushes which prevents buffer overflow:

- Incoming data site in a **buffer**.

- If the flood exceeds capacity, excess datagrams get **dropped**.

- To avoid that, the receiver sends a **"not ready"/stop signal** to pause the sender, then a **"ready"** signal once the buffer clears. It works like a spotlight.

### Windowing

Waiting for an ACK after every little segment is slow. So the sender can send **multiple segments before needing an ACK**. The amount it can send unacknowledged is the **window**, measured in **bytes, not packet**, for TCP/IP.

- Window size 1 means an ACK after every segment.

- Window size 3 means three segments, then an ACK.

- If segments keep going missing, the receiver can **shrink the window** to improve reliability.

### Acknowledgements

Reliable delivery uses **positive acknowledgements with retransmission**. The sender tracks every segment, starts a timer, and **retransmits if no ACK arrives before the timer expires**.

Example:

1.  Sender sends segments 1-3.

2.  Receiver ACks by requesting 4.

3.  Sender sends 4-6, but 5 goes missing.

4.  Receiver requests 5 again.

5.  Sender must resend 5 and get its ACK before mowing on to 7.

**Bottom line:** virtual circuit = TCP; no virtual circuit = UDP.

## Network Layer (Layer 3) & Routing

The Network layer handles **logical (IP) addressing**, tracks where devices are, and finds the **best path** for data. It moves traffic between devices that aren't locally attached, meaning between networks, not individual hosts.

**Routers** are the Layer 3 devices that do this. You'll sometimes hear them called “layer 3 switches.” When a packet hits a router interface:

1.  The router checks the **destination IP**.

2.  If the packet isn't addressed to that router itself, it looks up the **destination network** in its **routing table**.

3.  It picks an **exit interface** and forwards the packet.

4.  If there's no matching entry, the packet is **dropped**.

### Two Packet Types

| **buffer**               | **buffer**                                                       | **buffer**                                                                                                                                                  |
|--------------------------|------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Data packets**         | User traffic                                                     | *Routed* protocols: IP, IPv6                                                                                                                                |
| **Route-update packets** | outing-protocol chatter that builds and maintains routing tables | *Routing* protocols: **RIP (Routing Information Protocol)**, RIPv2, **EIGRP (Enhanced Interior Gateway Routing Protocol), OSPF (Open Shortest Path First)** |

### What's in a Routing Table

- **Network addresses**: protocol-specific, since each routing protocol tracks its own addressing scheme

- **Interface**: the exit used to reach a given network

- **Metric**: the distance to the remote network. RIP uses hop count. Others use bandwidth, delay, or tick count.

### Broadcast vs. Collision Domains

- **Routers break up broadcast domains**. Broadcasts don't cross routers by default, which cuts traffic.

- **Switches breakup collision domains**.

- Each router interface is a **separate network** and needs its own unique **network ID**. Every host on that network shares the same network number.

###  Router Facts to Memorise

- Don't forward broadcast or multicast packets by default

- Use the **logical address** to pick the next-hop router

- Can run **access lists** for security filtering

- can bridge at Layer 2 while also routing

- Connect **VLANs (Virtual LANs)** together

- can prove **QoS (Quality of Service)**

## Data Link Layer (Layer 2): MAC & LLC Sublayers

The Data Link layer handles physical transmission, error notification, topology, and flow control. It delivers messages to the correct device on a **LAN (Local Area Network)** using **MAC (hardware) addresses**, and translates Network-layer messages into bits for the physical layer.

Routers only care about where *networks* are The Data Link layer **uniquely identifies each device within a local network.**

**Framing**: the message gets wrapped in a **frame** with a header containing source and destination hardware addresses. It's like the disposable stages of an Apollo rocket: useful for part of the journey, then stripped off.

- Every time a packet hops between routers it gets **reframed** with control info for that hop, and that info is stripped at the receiving end.

- The **packet itself is never altered**. It's only wrapped and unwrapped for each media type.

### The Two sublayers (Ethernet/IEEE)

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 66%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Sublayer</strong></th>
<th><strong>What it does</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>MAC (Media Access control)</strong></td>
<td><p>Defines where packets get placed on the media using <strong>“first come, first served”</strong> contention-based access, where everyone shares bandwidth.</p>
<p>Handles <strong>physical addressing, logical topologies</strong> (the signal path through a physical topology), line discipline <strong>error notification (not correction)</strong> ordered frame delivery, and optional flow control.</p></td>
</tr>
<tr class="even">
<td><strong>LLC (Logical Link Control)</strong></td>
<td><p>Reads its own header to identify <strong>which Network-layer protocols (e.g., IP)</strong> a received frames payload belongs to.</p>
<p>Can also provide flow control and sequencing.</p></td>
</tr>
</tbody>
</table>

### IEEE Project 802

**IEEE (Institute of Electrical and Electronics Engineers) Project 802** standardised **LAN/MAN (Metropolitan Area Network)** tech. It's named for its first meeting, in the 2nd month of 1980.

- The ones to really remember: **802.3 = Ethernet, 802.11 = wireless**.

- Only **802.1, 802.3, 802.11, and 802.15** are still active.

## Physical Layer (Layer 1) & Modulation

The Physical layer just **sends and receives bits (1s and 0s)**. They're represented differently depending on the medium: voltage state transitions, light, radio signals, or audio tones.

It defines the **electrical, mechanical, procedural, and functional specs** for activating, maintaining, and deactivating a physical link including the interface between:

- **DTE (Data Terminal Equipment):** the attached device, usually at the customer

- **DCE (Data Communication Equipment):** a modem or **CSU/DSU (Channel Service Unit/Data Service Unit)**

It also defines the **physical topology:** the literal cable layout (bus, star, ring, mesh from Chapter 1). Don't confuse that with the **logical topology** (the signal path), which belongs to the Data Link layer.

### Modulation

**Modulation** varies a carrier waveform's properties to encode info onto it. It turns a **baseband** signal into a **passband** signal. A **passband** is the frequency range that passes through a filter without loss.

- **Modulator** does it. **Demodulator** reverses it. A device that does both is a **modem**.

- **Digital modulation:** moves a digital bit stream over an analog bandpass channel (e.g., old phone lines).

- **Analog modulation:** shifts an analog signal to a different frequency for transmission (e.g., audio,TV).

## Multiplexing & Line Coding

<table>
<colgroup>
<col style="width: 49%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Method</strong></th>
<th><strong>How it works</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>FDM (Frequency-Division Multiplexing)</strong></td>
<td>Multiple signals at the <strong>same time</strong> over the same physical network, each on its <strong>own frequency channel</strong></td>
</tr>
<tr class="even">
<td><strong>TDM (Time-Division Multiplexing)</strong></td>
<td><p>One signal path shared by multiple independent signals, each getting a <strong>syncronised time slice</strong> in an altering pattern.</p>
<p>The receiver demultiplexes them back to their original form.</p></td>
</tr>
<tr class="odd">
<td><strong>Ethernet baseband (line coding)</strong></td>
<td>Digital baseband modulation that uses the <strong>entire bandwidth for one signal at a time</strong>.</td>
</tr>
</tbody>
</table>

## Encapsulation (The Five Steps)

**Encapsulation** is data getting wrapped with protocol info at each OSI layer on the way out. Each layer only ever talks to its **peer layer** on the receiving end. It does that using **PDUs (Protocol Data Units)**, which carry control info, usually in a **header** and sometimes a **trailer**. The PDU name changes at every layer because each layer adds its own wrapper.

**The five sleeps at the transmitting host:**

| **Step** | **PDU**               | **Layer**    | **What happens**                                           |
|----------|-----------------------|--------------|------------------------------------------------------------|
| 1        | **Data**              | Upper layers | User info converted for transmission                       |
| 2        | **Segments**          | Transport    | Data segmented; a reliable connection is set up            |
| 3        | **Packets/Datagrams** | Network      | Logical (IP) address added so it can be routed             |
| 4        | **Frames**            | Data Link    | Hardware (Ethernet/MAC) addresses added for local delivery |
| 5        | **Bits**              | Physical     | Converted using a digital encoding and clocking scheme     |

Step 5 (encoding clocking) is the modulation process from Topic 6. Full encapsulation gets revisited in Chapter 6.

## Core Insights

<table>
<colgroup>
<col style="width: 35%" />
<col style="width: 64%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Concept</strong></th>
<th><strong>The thing to remember</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>OSI Model</td>
<td><p>Conceptual 7-layer blueprint from ISO.</p>
<p>Layer changes don't ripple.</p></td>
</tr>
<tr class="even">
<td>Mnemonic</td>
<td>Please Do Not Throw Sausage Pizza Away (Physical -&gt; Application)</td>
</tr>
<tr class="odd">
<td>Upper 3 vs. Lower 4</td>
<td><p>Upper = app communication no addressing.</p>
<p>Lower = end-to-end delivery.</p></td>
</tr>
<tr class="even">
<td>Application</td>
<td><p>Users interact via APIs.</p>
<p>Chrome itself isn't at layer 7.</p></td>
</tr>
<tr class="odd">
<td>Presentation</td>
<td>Translation (EBCDIC/ASCII), compression, encryption.</td>
</tr>
<tr class="even">
<td>Session</td>
<td><p>sets up, manages, and tears down sessions.</p>
<p>Simplex/half/full duplex</p></td>
</tr>
<tr class="odd">
<td>TCP vs. UDP</td>
<td><p>TCP = connection-orientated virtual circuit, reliable.</p>
<p>UDP = connectionless.</p></td>
</tr>
<tr class="even">
<td>Three-way handshake</td>
<td>SYN -&gt; SYN/ACK -&gt; ACK</td>
</tr>
<tr class="odd">
<td>Flow control</td>
<td>Receiver controls the pace with stop/ready signals.</td>
</tr>
<tr class="even">
<td>Windowing</td>
<td><p>Sends multiple segments per Ack.</p>
<p>Measured in bytes.</p></td>
</tr>
<tr class="odd">
<td>Network layer</td>
<td><p>Logical addressing, routing between networks.</p>
<p>Routers live here</p></td>
</tr>
<tr class="even">
<td>Routed vs. rooting protocols</td>
<td><p>Routed (IP) carries data.</p>
<p>Routing (RIP, OSPF, EIGRP) builds tables.</p></td>
</tr>
<tr class="odd">
<td>Domains</td>
<td><p>Routers split broadcast domains.</p>
<p>Switches spit collision domains.</p></td>
</tr>
<tr class="even">
<td>Data Link</td>
<td><p>MAC addresses and frames.</p>
<p>MAC sublayer = access/addressing.</p>
<p>LLC = which L3 proticol.</p></td>
</tr>
<tr class="odd">
<td>IEEE 802</td>
<td>802.3 = Ethernet, 802.11 = wireless</td>
</tr>
<tr class="even">
<td>Physical</td>
<td>Bits, DTE/DCE physical topology, modulation (FDM/TDM)</td>
</tr>
<tr class="odd">
<td>Encapsulation</td>
<td>Data -&gt; Segments -&gt; Packets -&gt; Frames -&gt; Bits</td>
</tr>
</tbody>
</table>

## Abbreviations List

- ACK — Acknowledgement

- API — Application Programming Interface

- ASCII — American Standard Code for Information Interchange

- CSU/DSU — Channel Service Unit/Data Service Unit

- DCE — Data Communication Equipment

- DTE — Data Terminal Equipment

- EBCDIC — Extended Binary Coded Decimal Interchange Code

- EIGRP — Enhanced Interior Gateway Routing Protocol

- FDM — Frequency Division Multiplexing

- HTTPS — Hypertext Transfer Protocol Secure

- IEEE — Institute of Electrical and Electronics Engineers

- IP — Internet Protocol

- IPv6 — Internet Protocol version 6

- ISO — International Organisation

- LAN — Local Area Network

- LLC — Logical Link Control

- MAC — Media Access Control

- OSI — Open Systems Interconnection

- OSPF — Open Shortest Path First

- PDU — Protocol Data Unit

- QoS — Quality of Service

- RIP — Routing Information Protocol

- SYN — Synchronize

- TCP — Transmission Control Protocol

- TDM — Time-Division Multiplexing

- UDP — User Datagram Protocol

- VLAN — Virtual Local Area Network

## ***<u>TL;DR Summary:</u>***

You're now armed with a ton of fundamental information. You're set to build on it and are well on your way to certification.

Let's take a minute to go over what you've learned in this chapter. We started by discussing;

- Internetworking models and the advantages of having them.

- I then discussed the OSI model—the seven-layer model used to help application developers design applications that can run on any type of system or network.

- Each layer has its special jobs and select responsibilities within the model to ensure that solid, effective communications do, in fact, occur.

- I provided you with complete details of each layer and discussed how you need to view the specifications of the OSI model.

- I also discussed the encapsulation method used in networking. Encapsulation is a highly important concept to understand, and I'll continue to discuss it throughout this book.

- This chapter finished with a brief introduction to modulation of digital and analog signals.

***<u>Exam Essentials:</u>***

- <u>**Memorize and understand the OSI layers.**    </u>

You must remember and understand the seven layers of the OSI model and what function each layer provides.

The Application, Presentation, and Session layers are the upper layers responsible for communicating from a user interface to an application.

The Transport layer provides segmentation, sequencing, and virtual circuits.

The Network layer provides logical network addressing and routing through the internetwork.

The Data Link layer frames and places data on the network medium.

The Physical layer is responsible for taking 1s and 0s and encoding them into a digital signal for transmission on the network segment.

- <u>**Know the sublayers of the Data Link layer.**    </u>

In addition to the OSI layers, knowing that the Data Link layer is the only layer that has sublayers and the functions of those sublayers is extremely important.

The Data Link layer has two sublayers: LLC and MAC.

- The LLC sublayer is responsible primarily for the multiplexing of Network layer protocols.

- The MAC sublayer is responsible for physical addressing and determining the appropriate time to place data on the network.
