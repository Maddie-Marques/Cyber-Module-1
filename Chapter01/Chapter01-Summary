**<u>CHAPTER 1 — Introduction to Networks</u>**

1.  **<u>Workstations, Servers & Hosts</u>**

A network is simply two or more connected computers sharing resources — files, printers, an internet connection — by communicating in binary. These three terms (workstation, server, host) overlap on purpose.

- **Workstation:** a beefg, multi-CPU machine end users work from daily. Don't confuse this with a **<u>client</u>** — a client just *requests* resources and might not even be a full workstation.

- **Server:** also powerful but “at the service" of the network, running a network operating system. Works best when dedicated to a single job — file, mail, print, web (via HTTPS), fax, applications, telephony, or proxy server. Needs far more CPU/storage/memory than a client since it's serving a crowd, and should be kept somewhere physically secure.

- **Host:** the loosest term of them all — TCP/IP-speak for *any* device with an IP address. Workstations and servers both qualify. It's a holdover from the main frame era, when only mainframes ("hosts") got addresses and everything else was just a dumb terminal.

2.  **<u>LANs, WANs, and the Scoped Variants</u>**

> A **LAN** spans one location (office, department, home office). It used to be capped at ~30 workstations, but advancing tech skill killed that limit — large LANs still get split into two **workgroups** (departmental groupings sharing a segment) for easier admin. Two separate workgroups can't reach each other's resources unless connected via a router, which lets both sides access each other's stuff while keeping performance and administration separate — one giant merged workgroup would just be slow and messy.
>
> A **WAN** spans large geographical areas using routers and public links — the Internet is a classic example. WANs differ from LANs: they usually need a router ports, span larger areas, run slower, and connection time/duration can be chosen (a LAN is just always on). The word “Internet” comes from **internetwork** — a LAN/WAN combo connecting many networks, where hosts use hardware addresses locally but IP addresses to reach hosts across a router. The Internet itself is a **distributed WAN** (many interconnected computers everywhere); a **centralized WAN** has one main site that remote offices connect into.
>
> Everything else — MAN, PAN, CAN, SAN, SDWAN, MPLS — isn't a fourth category of its own; it's a LAN or WAN scoped to a specific size or purpose.

| **TYPE**  | **Scope & Typical Use**                                                                                                                                                                                                                        |
|-----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **MAN**   | Metropolitan area over a leased carrier line — essentially a concentrated WAN, often fastin-ground fiber.                                                                                                                                      |
| **PAN**   | Close-range device-to-device (Bluetooth, infared, ZigBee, or wired) — just a few meters think conference room.                                                                                                                                 |
| **CAN**   | Sits between LAN and WAN in scope — connects buildings on a campus, usually with Wi-Fi and internet/data-center access baked in.                                                                                                               |
| **SAN**   | Exclusively storage traffic between servers and storage arrays, lives in data centers, uses Fibre Channel or iSCSI — never mixed with regular LAN traffic (FCoE is a transitional tech running Fibre Channel over existing Ethernet gear).     |
| **SDWAN** | Virtual WAN architecture — software manages connectivity, can dynamically add/remove bandwidth or reroute around a dead circuit in real time, decoupling hardware from its control logic.                                                      |
| **MPLS**  | Tags data with labels at the network's edge, forwards purely based on those labels through virtual (not physical) links — enables traffic prioritization ( e.g., voice over plain data) and redundancy via multiple paths through the "cloud". |
| **mGRE**  | Dynamically creates/tears down VPN tunnels to one or many nodes without pre-configured static end points (used in Dynamic Multipoint VPN).                                                                                                     |

3.  **<u>Peer-to-Peer VS. Client-Server</u>**

You can't tell these apart just by looking at the wiring — the difference is logical, not physical.

**Peer-to-Peer:** No central authority — every machine is a peer and whichever one holds a resource decides who gets access to it. A machine can be both a client and a server at once. Works fine for small networks with light security needs, but backups get sketchy, and every machine has to maintain its own user list and passwords, which fall out of sync fast.

**Client-server:** Flips this model — one server runs the network OS, centralizes security storage, and user account and directs clients to resources easier to find security of clients going straight to each other. This makes resources easier to find, security much tighter (all accounts live on one server that's never used as a workstation), and the network more scalable — despite doing more work, performance is actually better organised. Peer-to-Peer is simpler; client-server scales and secures better. Most real networks today are a **hybrid** of both.

4.  **<u>Physical Topologies</u>**

A network's physical topology is the literal wiring layout; its logical topology is how data actually moves through that layout — and the two can differ.

Bus, star, and mesh sit at three different points on the same trade-off: cost / simplicity VS. fault tolerant. Star wins in practice because it's the best compromise.

- **Bus:** One unbroken, terminated cable that every computer taps into (origionally via wire taps, later drop cables or "T" connecters on 10Base2). Every computer sees all traffic, but only the addressed one keeps it cheap, easy to install, one shared cable — but hard to troubleshoot/move/change, and has **zero fault tolerance:** one break kills the whole network.

- **Star (hub-and-spoke):** Every computer run its own cable (or wireless link) to a central hub / switch / acess point. A cable failure only kills that one machine — faults isolate per device — making easy to scale (just ran a new cable) and easy to troubleshoot, which it's why it's **today's default**/the most wildly used topology. Downside: more cabling costs and the central device (switch) is a **single point of Failure:** for the whole network. Two fancier variants: Point-to point star links (extending scalability further), and wireless star topologies using access points.

- **Ring:** Each computer connects to the next, forming a loop, the data travels around back to source. Adding a device breaks the ring (killing the network); needs more cabling, hard to reconfigure, not fault-tolerant — basically extinct in LANs today, though it still shows up in WAN tech like SONET.

- **Mesh:** Every device connects to every other device — maximum possible connections using the formula **n(n+1)/2** (4 nodes = 6 links, 10 nodes = 45 links). Great fault tolerance (If one link dies, traffic just reroutes) and least likely to have collisions, but cost and management overhead explodes fast — doesn't scale, so it's basically extinct in LANs. You'll mainly see a restrained **hybrid mesh** on WANs (including the Internet), or a **partial mesh** where only select devices are interconnected to save cost/complexity at the expense of some fault tolerance.

- **Point-to-Point:** A direct single-path link between two routers/switch (or even two computers) — Physical if via serial cable, logical if it's a circuit over Frame Relay/MPLS. Not very scalable — same limitation as peer-to-peer networking.

- **Point-to-multipoint:** One router interface connects out to multiple destination routers — e.g., one corporate router to several branches, or a campus network.

- **Hybrid:** A mix of two or more topology types working together in the same network (e.g., star-connected LANs joined in a fall mesh, connected to a WAN ring).

**Why it matters:** Bus and mesh are the two extremes — cheapest/least fault-tolerant vs. most fault-tolerant / least affordable. **Star** is the practical middle ground almost every real LAN uses today.

5.  **<u>Backbone, Segments & Related Infrastructure</u>**

The **backbone** is the network's spine — the fast (usually Gigabit Ethernet or faster), robust, common connection point that all segments and servers plug in to. It needs top-quality media since it's typically stay in service for 5-10 years. A **segment** is any smaller section connected to, but not physically part of, that backbone.

**Related concepts:**

- **Demarcation point (demarc):** where a carrier's responsibility ends and yours begins — " via a smart jack".

- **Service provider links:**

- **DSL :** copper, last hop digital service.

- **Cable:** hybrid fiber/coax via a cable modem.

- **Leased lines:** dedicated, unshared, secure.

- **Virtual infrastructure:**

- **vSwitch:** switching/routing entirely in software on a hypervisor — no physical hardware needed.

- **vNIC:** connects a virtual machine to the hypervisor and out to the LAN.

- **NFV (Network Function Virtualisation):** lets you run functions like routers, firewalls, and load balancers all virtualised on one device.

**Choosing a topology** comes down to asking the right question: budget, required falt-tolerance/security, and how much the network needs to scale. For example: a wireless AP for a small, cheap setup; a wired star for a growing company; hybrid/partial mesh for an ISP needing near-100% uptime.

***<u>TL;DR Summary:</u>***

- This chapter created a solid foundation for you to build your networking knowledge on as you go through this book.

- You also learned that the components required to build a network aren't all you need. Understanding the various type of network connection methods, such as peer-to-peer and client-server, is also vital.

- Furthermore, you learned about the various types of logical and physical network topologies and the features and drawbacks of each. I wrapped up the chapter with a discussion about network virtalisation and equipped you with the right questions to ask yourself to ensure that you come up with the right network topology for your networking needs.

***<u>Exam Essentials:</u>***

- **<u>Know your network topologies.</u>**

Know the name and descriptions of the topologies.

Be aware of the difference between physical networks (what humans see) and logical networks (what the equipment “sees”).

- **<u>Know the advantages and disadvantage of the topologies.</u>**

It is essential to know what each topology brings to the table.

Knowing one various characteristics of each topology comes in handy during troubleshooting.

- **<u>Understanding the terms LAN and WAN</u>**

You need to understand when you would use a LAN and when you would use a WAN.

A LAN is used to connect a group of hosts, and a WAN is used to connect various LANs.

- **<u>Know the three-tiered model and collapsed core.</u>**

Know what the core, distribution and access layer provide in the three-tiered model.

The collapsed core is used to reduce costs by collapsing the core and distribution tiers onto one tier.

- **<u>Understand the differences between north-south traffic and east-west traffic.</u>**

North-south traffic typically leaves the network through a routed connection.

East-west traffic typically resides in the immediate network.
