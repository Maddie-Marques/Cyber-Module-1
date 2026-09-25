Step 1 — Your answers to the book's Exercise 3.1 (the ports on your own computer and router)

Exercise 3.1: Investigating Computer Connections

Q1 – Which ports can you identify (DB-9, USB, RJ-45, just to name a few)?

ANSWER: My laptop (Dell) has a mix of modern and legacy ports:

| Port | Type | Purpose |
|---|---|---|
| Thunderbolt 3 Type-C | Combo | USB-C data, video output, charging |
| Thunderbolt 4 Type-C | Combo | Same as above, newer/faster standard |
| Smart Card reader | Security | Smart-card-based authentication/login |
| USB-A / Mini DisplayPort | Combo | USB-A data or DisplayPort video |
| **RJ-45** | Networking | Wired Ethernet connection |
| HDMI Type-A | Video | External monitor/TV output |
| USB ports (×2) | Data | Peripheral devices |
| UAJ (Universal Audio Jack) | Audio | Combo headset/microphone jack |
| SD card slot | Storage | Memory card reader |

Q2 – How is your computer connected to the network?

My laptop's on Wi-Fi, using its built-in wireless card — nothing extra plugged in. My desktop's a different story though, since it needs a USB Wi-Fi dongle to get online at all.

Q3 – What type of connections does your router have?

My router's got 3 LAN ports, 1 WAN port, a power connector, and a reset button. The WAN port runs an Ethernet cable to a separate modem, which hooks into fiber internet from my ISP. The fiber cable itself ends in a square (SC) connector.



Step 2 — Your T568A / T568B pinout sketch with the differing pins circled — a phone photo or exported diagram

<img width="1380" height="920" alt="image" src="https://github.com/user-attachments/assets/e46cfe13-bc4e-4ab8-8cda-f80a73300bdc" />

---

Step 3 — Your answers to the book's Exercise 3.2 (the real cable you inspected, its standard, and its type)

Exercise 3.2: Investigating Ethernet Cables

Q1 – What type of cable is it, and what EIA/TIA 568 wiring code does it use?

ANSWER: It's a Cat 5 UTP (Unshielded Twisted Pair) cable, wired to the T568B standard.

Q2 – Is the cable a straight-through or crossover cable?

ANSWER: It's straight-through. It's connecting **unlike devices** — PC-to-router and modem-to-router — so both ends are wired the same way (T568B), which is what makes it straight-through instead of crossover.

Q3 – What was the cable connecting?
A PC to a router, and separately, a modem to a router.

---

Step 4 — Your cable-choice justification for each link in the Step 4 topology (laptop → switch → switch → router → modem)


| Link | Device Types | Cable | Standard/Wiring | Justification |
|---|---|---|---|---|
| Laptop → Switch | Unlike | Cat 5e/6 UTP | Straight-through | Textbook host-to-switch setup, nothing unusual |
| Switch → Switch | Like | Cat 5e/6 UTP | Traditionally crossover | Used to need crossover since both switches use the same TX/RX pins, but these days a straight-through works fine because Auto-MDIX handles it |
| Switch → Router | Unlike | Cat 5e/6 UTP | Straight-through | Different device types, so straight-through as usual |
| Router → Modem | Unlike | Cat 5e/6 UTP | Straight-through | Router's WAN port to the modem, unlike devices, straight-through again |

---

Step 5 — Your answers to the book's own Written Lab (fill in the EIA/TIA 568A and 568B pin tables)

Written Lab

You can find the answers to the written labs in Appendix A. Fill out the respective EIA/TIA 568A and B wiring standard.

EIA/TIA 568A

| Pin | Colour |
|---|---|
| 1 | White/Orange |
| 2 | Orange |
| 3 | White/Green |
| 4 | Blue |
| 5 | White/Blue |
| 6 | Green |
| 7 | White/Brown |
| 8 | Brown |

EIA/TIA 568B

| Pin | Colour |
|---|---|
| 1 | White/Green |
| 2 | Green |
| 3 | White/Orange |
| 4 | Blue |
| 5 | White/Blue |
| 6 | Orange |
| 7 | White/Brown |
| 8 | Brown |
