Step 1 — 

Exercise 5.1: Experimenting with DHCP

| | IPv4 Address | Lease Obtained |
|---|---|---|
| Before (`ipconfig /all`) | 192.168.51.238 | 25 September 2026, 02:41:46 |
| After (`ipconfig /release` → `/renew` → `ipconfig /all`) | 192.168.51.238 | 25 September 2026, 06:32:06 |

(

---

## Step 2 — Exercise 5.2: Examining DNS Entries

**Domain 1: www.wiley.com**

| Query | Result |
|---|---|
| A/AAAA record (default) | Resolves through Cloudflare's CDN (`www.wiley.com.cdn.cloudflare.net`) — both IPv6 and IPv4 addresses returned (`104.18.42.79`, `172.64.145.177`, plus two IPv6 addresses) |
| CNAME | `www.wiley.com` → canonical name `www.wiley.com.cdn.cloudflare.net` |
| MX | Two mail exchangers, both preference 10: `mxa-0053b401.gslb.pphosted.com` and `mxb-0053b401.gslb.pphosted.com` |

**Domain 2: www.unitec.ie**

| Query | Result |
|---|---|
| A record (default) | `unitec.ie` → `185.43.233.181` |
| CNAME | `www.unitec.ie` → canonical name `unitec.ie` |
| MX | `mx1-eu.emailsecurity.app` (preference 10) and `mx2-eu.emailsecurity.app` (preference 20) |

> **Reflection:** Good side-by-side comparison — Wiley routes everything through Cloudflare's CDN (hence the CNAME pointing to a cloudflare.net domain and multiple IPv4/IPv6 addresses for load distribution), while Unitec just points its www subdomain straight at the root domain with a single IP. Wiley's two MX records share equal priority (both 10, so either can be tried first), while Unitec's have distinct priorities (10 vs. 20) — a nice real-world example of MX preference numbers actually doing something, since mail servers try `mx1` first and only fall back to `mx2` if it's down.

---

## Step 3 — Hub/Switch/Router Collision & Broadcast Domain Sketch

Recreated from memory: three rows comparing a hub, switch, and router, each connecting four devices, and how many collision/broadcast domains each one creates.

`![Hub, switch, and router collision/broadcast domain comparison](hub-switch-router-domains.png)`

*(Drop the exported/screenshotted diagram in here, saved as `hub-switch-router-domains.png` next to this file.)*

| Device | Collision Domains | Broadcast Domains |
|---|---|---|
| Hub (4 devices, no intelligence) | 1 | 1 |
| Switch (4 devices, one per port) | 4 | 1 |
| Router (4 devices, one per interface) | 4 | 4 |

> A hub breaks up nothing. A switch breaks up collision domains only. A router breaks up both.

---

## Step 4 — One-Sentence Defense Map

| Device | Sentence |
|---|---|
| **Firewall** | Protects the network perimeter by filtering traffic based on IP/port rules, and sits **inline** — it can actively block traffic. |
| **IDS** | Monitors traffic for suspicious patterns and alerts on them, but sits **out-of-band** — it can only observe and report, never block. |
| **IPS** | Monitors traffic for suspicious patterns like an IDS, but sits **inline**, meaning it can actively block malicious traffic in real time. |
| **HIDS** | Protects a single host by monitoring that machine's own logs and file activity, and is **out-of-band** relative to network traffic — it observes locally rather than blocking network flow. |
| **Proxy Server** | Protects internal clients by handling requests on their behalf (hiding their identity, filtering content), and sits **inline**, since traffic must pass through it. |

---

## Step 5 — Written Lab: Device / OSI Layer Matching

| Description | Device or OSI Layer |
|---|---|
| This device sends and receives information about the Network layer. | **Router** |
| This layer creates a virtual circuit before transmitting between two end stations. | **Transport layer** |
| A layer 3 switch or multilayer switch. | **Router (functionally)** |
| This device uses hardware addresses to filter a network. | **Switch/Bridge** |
| Ethernet is defined at these layers. | **Physical and Data Link layers** |
| This layer supports flow control and sequencing. | **Transport layer** |
| This device can measure the distance to a remote network. | **Router** |
| Logical addressing is used at this layer. | **Network layer** |
| Hardware addresses are defined at this layer. | **Data Link layer** |
| This device creates one big collision domain and one large broadcast domain. | **Hub** |
| This device creates many smaller collision domains, but the network is still one large broadcast domain. | **Switch** |
| This device can never run full-duplex. | **Hub** |
| This device breaks up collision domains and broadcast domains. | **Router** |
