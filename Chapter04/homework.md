Step 1 — Your answers to the book's Exercise 4.1 (your own IP and MAC address converted between binary/decimal/hex)

Exercise 4.1: Converting Binary, Decimal, and Hexadecimal

**IP Address: 192.168.51.238**

| Octet | Decimal | Binary | Hex |
|---|---|---|---|
| 1 | 192 | 11000000 | C0 |
| 2 | 168 | 10101000 | A8 |
| 3 | 51 | 00110011 | 33 |
| 4 | 238 | 11101110 | EE |

**MAC Address: 80-AF-CA-35-03-AB**

| Byte | Hex | Decimal | Binary |
|---|---|---|---|
| Byte 1 (OUI) | 80 | 128 | 10000000 |
| Byte 6 (device-specific) | AB | 171 | 10101011 |

> **Reflection:** Ran a quick sanity check on both — `80 = 8×16 + 0 = 128`, and `128` in binary is `10000000`, so that lines up. Same deal with `AB = 10×16 + 11 = 171`. Nice to see the conversions actually check out instead of just trusting the math blindly.

---

## Step 2 — MAC Address Anatomy Sketch

A sketch of a 48-bit MAC address split into its OUI (vendor) and device-specific halves, with the I/G and L/G bits labeled in the first byte.

`![MAC address anatomy sketch, OUI/device-specific split with I/G and L/G bits labeled](mac-address-anatomy-sketch.png)`

*(Drop the exported/screenshotted diagram in here, saved as `mac-address-anatomy-sketch.png` next to this file.)*

**Reference for what should be on it:**

- **First 3 bytes (OUI):** assigned by IEEE to the vendor
- **Last 3 bytes:** assigned by the vendor to identify the specific card
- **I/G bit** (first byte, second-lowest bit): `0` = unicast, `1` = multicast/broadcast
- **L/G bit** (first byte, lowest bit): `0` = factory burned-in address, `1` = locally administered (this is the bit MAC-spoofing tools flip)

---

## Step 3 — Exercise 4.2: Exploring Ethernet Standards

| Link | Connection | Ethernet Standard | Max Speed | Max Distance | Media |
|---|---|---|---|---|---|
| Laptop → Router | Computer to router | **802.11 (Wi-Fi)** — likely 802.11ac or 802.11ax, worth confirming on the router's spec sheet | 400 Mbps–1+ Gbps depending on standard/router | ~30–50 m indoors, less through walls | Radio waves (2.4 GHz / 5 GHz) |
| PC → Router | Computer to router | **100BaseTX** (Cat 5 UTP's guaranteed spec — could be 1000BaseT if the cable's actually Cat 5e) | 100 Mbps | 100 m (328 ft) | UTP copper |
| Router → Modem | Router to modem | **100BaseTX** (same Cat 5 UTP) | 100 Mbps | 100 m (328 ft) | UTP copper |
| Modem → ISP | Modem to internet provider | Fiber, typically **GPON** on the ISP side (not a standard 802.3 spec, since it's fiber-to-the-home tech) | Varies by plan, often several hundred Mbps to multi-Gbps | Kilometers | Single-mode fiber |

> **Reflection:** This one was a fun excuse to actually go check my own setup instead of guessing at generic examples. A couple of things worth double-checking later: whether that PC-to-router cable is really Cat 5 or secretly Cat 5e (which would bump it up to 1000BaseT), and what exact 802.11 standard my router actually supports instead of just assuming.

---

## Step 5 — Decoding Three Standard Names Cold

| Standard | Speed | Medium | Max Distance |
|---|---|---|---|
| `10GBaseLR` | 10 Gbps | Baseband, long-range single-mode fiber | ~10 km |
| `1000BaseSX` | 1000 Mbps (1 Gbps) | Baseband, short-range multimode fiber | ~220–550 m depending on cable grade |
| `100BaseTX` | 100 Mbps | Baseband, twisted-pair copper (Cat 5 UTP) | 100 m (328 ft) |

> **Reflection:** Once the `[Speed][Base][Medium/Distance]` pattern clicks, these basically decode themselves — no lookup table needed.

---

## Step 6 — Written Lab: Binary, Decimal, and Hexadecimal Conversions

1. The decimal value of **15** is equal to 0xF in hexadecimal.
2. The decimal value of 5 equals **0101** in binary.
3. The decimal value of 14 equals **0xE** in hexadecimal.
4. The binary value of 0111 equals **7** in decimal.
5. The binary value of **11100** equals 28 in decimal.
6. The hexadecimal value of **0x10** equals 16 in decimal.
7. The decimal value of **11** equals 0xb in hexadecimal.
8. The hexadecimal value of 0xE equals **1110** in binary.
9. The decimal value of 177 equals **10110001** in binary.
10. The decimal value of 208 equals **0xD0** in hexadecimal.

