Step 1 — Subnetting 192.168.50.0/27 by Hand

`/27` = 255.255.255.224 — 3 bits borrowed from the host portion of this Class C network.

Block size: 256 - 224 = 32

1. Number of subnets: 2³ = 8
2. Hosts per subnet: 2⁵ - 2 = 30 usable hosts

3–5. Subnet list, broadcast address, and valid host range:**

| Subnet | Network Address | Broadcast Address | Valid Host Range |
|---|---|---|---|
| 1 | 192.168.50.0 | 192.168.50.31 | .1 – .30 |
| 2 | 192.168.50.32 | 192.168.50.63 | .33 – .62 |
| 3 | 192.168.50.64 | 192.168.50.95 | .65 – .94 |
| 4 | 192.168.50.96 | 192.168.50.127 | .97 – .126 |
| 5 | 192.168.50.128 | 192.168.50.159 | .129 – .158 |
| 6 | 192.168.50.160 | 192.168.50.191 | .161 – .190 |
| 7 | 192.168.50.192 | 192.168.50.223 | .193 – .222 |
| 8 | 192.168.50.224 | 192.168.50.255 | .225 – .254 |

---

Step 2 — Four-Step Troubleshooting Sequence

| Step | Command | Result | What it proved (and what a failure would have meant) |
|---|---|---|---|
| 1 | `ping 127.0.0.1` | 4/4 received, 0% loss | Local TCP/IP stack is properly initialized. A failure here would mean a broken IP stack, requiring TCP/IP to be reinstalled — nothing to do with the network yet. |
| 2 | `ping 192.168.51.238` (own IP) | 4/4 received, 0% loss | NIC can communicate with the IP protocol stack via the LAN driver. A failure here would point to a NIC-level problem, not cabling or network. |
| 3 | `ping 192.168.51.1` (default gateway) | 4/4 received, 0% loss, ~3ms avg | NIC is physically connected to the local network and can reach the router. A failure here would mean a local physical network problem — anywhere between the NIC and the router. |
| 4 | `ping 8.8.8.8` (external host) | 4/4 received, 0% loss, ~5ms avg | Full end-to-end IP communication works, all the way out to a remote network. A failure here — with steps 1–3 successful — would isolate the problem to somewhere beyond the local network (ISP, routing, or the remote host). |


---

Step 3 — NAT Type Identification

| | Value |
|---|---|
| Private IP | 192.168.51.238 |
| Public IP | 102.182.193.231 |

Why they differ: The private IP only works on my home network — my router uses NAT to translate it into the single public IP that represents my whole household to the internet.

NAT type: Almost certainly Overloading (PAT — Port Address Translation), since every device on my home network shares that one public IP simultaneously. Overloading is the only NAT type that maps many private IPs to a single public IP at once, telling each device's traffic apart by port number rather than IP.

---

Step 4 — Reverse the Problem: Find the Mask

Requirement: 1 Class C network, 5 usable subnets, largest subnet needs 15 hosts.

Working from host bits first, since that's the tighter constraint:

- 4 host bits → 2⁴ - 2 = 14 usable hosts — **fails** (need at least 15)
- 5 host bits → 2⁵ - 2 = 30 usable hosts — **works**

5 host bits leaves 8 - 5 = 3 bits for subnetting → 2³ = 8 subnets, comfortably covering the 5 required.

Result: Subnet mask = **255.255.255.224** (`/27`)

Why a smaller block size wouldn't work: A `/28` mask (4 host bits, block size 16) only gives 14 usable hosts per subnet — one short of the 15 needed. Even though `/28` would yield more subnets (16 vs. 8), it fails the host-count requirement, which takes priority since a subnet that can't fit its own hosts is useless no matter how many subnets exist.

---

Step 5 — Written Lab: CIDR / Subnet Mask Table

<img width="1092" height="1534" alt="image" src="https://github.com/user-attachments/assets/18bd445d-b9e8-4acd-9c4f-6c0dbcd52b3f" />


| CIDR | Subnet Mask |
|---|---|
| /13 | 255.248.0.0 |
| /8 | 255.0.0.0 |
| /15 | 255.254.0.0 |
| /30 | 255.255.255.252 |
| /22 | 255.255.252.0 |
| /26 | 255.255.255.192 |
| /28 | 255.255.255.240 |
| /4 | 240.0.0.0 |
| /18 | 255.255.192.0 |
| /27 | 255.255.255.224 |
| /29 | 255.255.255.248 |
| /21 | 255.255.248.0 |
| /11 | 255.224.0.0 |
| /25 | 255.255.255.128 |
| /10 | 255.192.0.0 |

