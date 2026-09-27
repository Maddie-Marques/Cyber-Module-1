Step 1 — Exercise 7.1: Identifying IP Addresses

Only one adapter in the `ipconfig /all` output actually had an active IPv4 address — everything else showed "Media disconnected" or no IPv4 line at all (Ethernet adapter and both Wi-Fi Direct virtual adapters were unused).

| Adapter | IPv4 Address | Version | Class | Public/Private |
|---|---|---|---|---|
| Wireless LAN adapter WiFi | 192.168.51.238 | IPv4 | **Class C** (192–223 first octet) | **Private** — 192.168.0.0/16 range |

**Version/Class:** IPv4, Class C
**Public or private:** Private
All other adapters (Ethernet, both Wi-Fi Direct virtuals) were disconnected/unused, so no active address to classify there.

---

Step 2 — Public IP Lookup & NAT

| | Value |
|---|---|
| **Public IP address** | 102.182.193.231 |
| **Version/Class** | IPv4, **Class A** (first octet 102 falls in the 1–126 range) |
| **Public/Private** | **Public** |

**One-sentence NAT explanation:** My public IP (102.182.193.231) differs from my local IP (192.168.51.238) because my router uses NAT to translate every private address on my home network into a single shared public address whenever traffic heads out to the internet.

---

Step 3 — EUI-64 Conversion (Hand-Worked)

**MAC address:** `80-AF-CA-35-03-AB`

<img width="1380" height="920" alt="image" src="https://github.com/user-attachments/assets/03050606-f62e-4fc1-a576-a76a8e0dfe27" />


```
1. Split the MAC into two halves:
   80-AF-CA   |   35-03-AB

2. Insert FFFE in the middle:
   80-AF-CA-FF-FE-35-03-AB

3. Flip the 7th bit (the U/L bit) of the first byte:
   First byte: 80 (hex) = 10000000 (binary)
   Bit position (left to right): 1  2  3  4  5  6  7  8
   Bit 7 flips: 0 -> 1
   New binary: 10000010 = 82 (hex)

4. Reassemble:
   82-AF-CA-FF-FE-35-03-AB

5. Written as an IPv6 interface ID:
   82AF:CAFF:FE35:03AB
```

Result: MAC `80-AF-CA-35-03-AB` → EUI-64 interface ID `82AF:CAFF:FE35:03AB`

---

Step 4 — Network / Broadcast / Host Range Calculation

Address used: **172.16.0.0** (Class B, default mask /16 = 255.255.0.0)

| | Value |
|---|---|
| Network address (all host bits off) | 172.16.0.0 |
| Broadcast address (all host bits on) | 172.16.255.255 |
| Valid host range | 172.16.0.1 – 172.16.255.254 |

The first two octets (172.16) stay fixed as the network portion; the last two octets are entirely host bits, giving 65,534 usable addresses.

---

Step 5 — Written Lab 7.1

1. Class C private range: **192.168.0.0 – 192.168.255.255**
2. IPv6 benefits over IPv4: vastly larger address space (128-bit vs. 32-bit), no need for NAT, simplified header for faster routing, built-in autoconfiguration (SLAAC), native multicast (no broadcasts at all), and IPsec support built into the protocol.
3. **APIPA** (Automatic Private IP Addressing)
4. Unicast address: an address assigned to a single interface — a packet sent to it goes to that one specific device.
5. Multicast address: an address that identifies a group of interfaces — a packet sent to it is delivered to every member of that group.
6. **MAC address** (Media Access Control address)
7. IPv6 (128 bits) has **96 more bits** than IPv4 (32 bits).
8. Class B private range: **172.16.0.0 – 172.31.255.255**
9. Class C first octet: **192–223 decimal**, or `110xxxxx` in binary
10. **127.0.0.1** is the loopback address, used to test a device's own local TCP/IP stack without sending traffic onto the network.

Written Lab 7.2

1. Unicast
2. Global unicast address
3. Link-local address
4. Unique local address
5. Multicast
6. Anycast
7. Anycast (one-to-nearest)
8. ::1
9. FE80
10. FC00::/7 

