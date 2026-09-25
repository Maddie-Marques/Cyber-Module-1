Step 1 — Your before/after results from the book's Exercise 5.1 (DHCP release/renew)

Exercise 5.1: Experimenting with DHCP

| | IPv4 Address | Lease Obtained |
|---|---|---|
| Before (`ipconfig /all`) | 192.168.51.238 | 25 September 2026, 02:41:46 |
| After (`ipconfig /release` → `/renew` → `ipconfig /all`) | 192.168.51.238 | 25 September 2026, 06:32:06 |

(Process recorded on video)

---

Step 2 — Your nslookup results from Exercise 5.2 for two domains (A, CNAME, and MX records)

Exercise 5.2: Examining DNS Entries

ANSWER:

Domain 1: www.wiley.com

| Query | Result |
|---|---|
| A/AAAA record (default) | Resolves through Cloudflare's CDN (`www.wiley.com.cdn.cloudflare.net`) — both IPv6 and IPv4 addresses returned (`104.18.42.79`, `172.64.145.177`, plus two IPv6 addresses) |
| CNAME | `www.wiley.com` → canonical name `www.wiley.com.cdn.cloudflare.net` |
| MX | Two mail exchangers, both preference 10: `mxa-0053b401.gslb.pphosted.com` and `mxb-0053b401.gslb.pphosted.com` |

Domain 2: www.unitec.ie

| Query | Result |
|---|---|
| A record (default) | `unitec.ie` → `185.43.233.181` |
| CNAME | `www.unitec.ie` → canonical name `unitec.ie` |
| MX | `mx1-eu.emailsecurity.app` (preference 10) and `mx2-eu.emailsecurity.app` (preference 20) |

(Process recorded on video)

---

Step 3 — Your hub/switch/router collision-and-broadcast-domain sketch, recreated from memory — a phone photo or exported diagram

<img width="1380" height="644" alt="image" src="https://github.com/user-attachments/assets/6d9ab661-0ae7-48e4-9ed4-921d6ecce856" />

---

Step 4 — Your one-sentence defense-map notes on Firewall, IDS, IPS, HIDS, and Proxy Server

ANSWER: 

| Device | Sentence |
|---|---|
| Firewall | Protects the network perimeter by filtering traffic based on IP/port rules, and sits inline — it can actively block traffic. |
| IDS | Monitors traffic for suspicious patterns and alerts on them, but sits out-of-band — it can only observe and report, never block. |
| IPS | Monitors traffic for suspicious patterns like an IDS, but sits inline, meaning it can actively block malicious traffic in real time. |
| HIDS | Protects a single host by monitoring that machine's own logs and file activity, and is out-of-band relative to network traffic — it observes locally rather than blocking network flow. |
| Proxy Server | Protects internal clients by handling requests on their behalf (hiding their identity, filtering content), and sits inline, since traffic must pass through it. |

---

Step 5 — Your answers to the book's own Written Lab (matching each description to a device or OSI layer)

Written Lab

Complete the table by filling in the appropriate layer of the OSI or hub, switch, or router device. You can find the answers in Appendix A.

| Description | Device or OSI Layer |
|---|---|
| This device sends and receives information about the Network layer. | Router |
| This layer creates a virtual circuit before transmitting between two end stations. | Transport layer |
| A layer 3 switch or multilayer switch. | Router (functionally) |
| This device uses hardware addresses to filter a network. | Switch/Bridge |
| Ethernet is defined at these layers. | Physical and Data Link layers |
| This layer supports flow control and sequencing. | Transport layer |
| This device can measure the distance to a remote network. | Router |
| Logical addressing is used at this layer. | Network layer |
| Hardware addresses are defined at this layer. | Data Link layer |
| This device creates one big collision domain and one large broadcast domain. | Hub |
| This device creates many smaller collision domains, but the network is still one large broadcast domain. | Switch |
| This device can never run full-duplex. | Hub |
| This device breaks up collision domains and broadcast domains. | Router |
