Step 1 & 2 — Exercise 6.1: Examining Port Numbers

Five connections pulled from `netstat -nab` output, matched to protocol/port/transport:

| # | Local Address:Port | Foreign Address:Port | Protocol/Transport | Matches Reference Table? |
|---|---|---|---|---|
| 1 | `0.0.0.0:445` (LISTENING) | — | TCP | **SMB** — TCP 445 |
| 2 | `0.0.0.0:123` (W32Time) | `*:*` | UDP | **NTP** — UDP 123 |
| 3 | `192.168.51.238:53061` (brave.exe) | `17.188.180.29:443` (ESTABLISHED) | TCP | Destination matches **HTTPS** (TCP 443); source port 53061 is ephemeral |
| 4 | `192.168.51.238:58147` (SearchHost.exe) | `23.11.41.157:80` (ESTABLISHED) | TCP | Destination matches **HTTP** (TCP 80); source port 58147 is ephemeral |
| 5 | `192.168.51.238:139` (LISTENING) | — | TCP | Not in the Reference table — legacy NetBIOS Session Service, predates SMB's move to port 445 |

Reflection: Nice mix in there — #1 and #2 are well-known ports on both ends with nothing ephemeral involved, #3 and #4 show the far more typical pattern of a random high-numbered ephemeral source port hitting a fixed well-known destination port, and #5 is a good reminder that not everything you'll see maps cleanly onto the book's table — some ports, like 139, are older protocols worth recognizing even outside the standard list.

---

Step 3 — Capturing a Real ARP Exchange

Attempt 1: Pinged `192.168.51.1` (the router). No change in the ARP table before/after — expected, since the router's already constantly cached from routine connectivity.

Attempt 2: Pinged `192.168.51.205` instead, aiming for a device not recently contacted:

```
Pinging 192.168.51.205 with 32 bytes of data:
Request timed out. (x4)
Packets: Sent = 4, Received = 0, Lost = 4 (100% loss)
```

ARP table before and after — identical either way:

```
192.168.51.1     f0-2f-74-e2-f6-94     dynamic
192.168.51.205   58-11-22-d1-f0-44     dynamic   <- already present, unchanged
192.168.51.255   ff-ff-ff-ff-ff-ff     static
224.0.0.22       01-00-5e-00-00-16     static
224.0.0.251      01-00-5e-00-00-fb     static
224.0.0.252      01-00-5e-00-00-fc     static
239.192.152.143  01-00-5e-40-98-8f     static
239.255.255.250  01-00-5e-7f-ff-fa     static
255.255.255.255  ff-ff-ff-ff-ff-ff     static
```

No new or changed entry — `192.168.51.205` was already ARP-resolved before this exercise, likely just from background network chatter rather than any direct contact from this PC. The ping itself timed out (100% loss), meaning the device isn't responding to ICMP, which is a separate matter from ARP entirely.

Reflection: Turned into a good real demonstration of something easy to gloss over — ARP entries can show up from general background network activity, not just your own direct pings or connections. It also cleanly separates two ideas that are easy to lump together: ARP resolves *addresses*, ICMP (ping) tests *reachability* — a device can be fully ARP-resolved and still ignore a ping entirely.

---

Step 4 — 5-Layer Encapsulation Stack (HTTP GET Request)

<img width="1380" height="796" alt="image" src="https://github.com/user-attachments/assets/52743ea7-2aa1-4dc2-b3a5-337be79c8602" />



| Step | PDU | What gets added |
|---|---|---|
| 1 | Data | HTTP GET request, plain application data |
| 2 | Segment | TCP header — source/destination ports (e.g. 443) |
| 3 | Packet | IP header — source/destination IP addresses |
| 4 | Frame | MAC header — source/destination MAC addresses |
| 5 | Bits | Converted to binary, sent as electrical/light/radio signal |

---

Step 5 — Written Lab: Service → Transport Protocol & Port

| Service | Transport Protocol | Port |
|---|---|---|
| SFTP | TCP | 22 |
| Telnet | TCP | 23 |
| SMTP | TCP | 25 |
| NTP | UDP | 123 |
| LDAP | TCP | 389 |
| TFTP | UDP | 69 |
| RDP | TCP | 3389 |
| Syslog | UDP | 514 |
| SMB | TCP | 445 |
| IMAP | TCP | 143 |
| HTTP | TCP | 80 |
| HTTPS | TCP | 443 |
| LDAPS | TCP | 636 |
| SMTPS | TCP | 465 |
| SSH | TCP | 22 |
| SQL | TCP | 1433 (MS SQL Server default) |
