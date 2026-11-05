# ***<u>Why Subnet, and How to Plan Subnets:</u>***

Chapter 7 gave you one flat network: one set for host bits, one broadcast domain. Once an organisation needs more than one segment (a floor, a branch office, a VLAN), you carve that one address block into several smaller ones without wasting addresses. That's **subnetting**. the same block-size math that finds your subnets also finds your broadcast addresses and valid host ranges.

## <u>Benefits:</u>

- **Reduced network traffic**: routers create broadcast domains, so more (smaller) broadcast domains means less traffic per segment.

- **Optimised performance**: the reward for reducing traffic.

- **Simplified management**: problems are easier to isolate in several small networks than in one giant one.

- **Spanning large geographic distances**: WAN links are slower and pricier than LAN links, so connecting multiple smaller networks is more efficient.

To create subnets, you **borrow bits from the host portion** and reserve them for the subnet address. More subnets means fewer bits.

## <u>Design Recipe:</u>

1.  **Number of network IDs needed**: one per subnet, plus one per WAN connection.

2.  **Number of host IDs per subnet**: one per TCP/IP, plus one per router interface.

3.  **Create one subnet mask** for the network, a **unique subnet ID** per physical segment, and a **host-ID range** per subnet.

### *Powers of 2 to memorise:*

| **2¹** | **2²** | **2³** | **2⁴** | **2⁵** | **2⁶** | **2⁷** | **2⁸** | **2⁹** | **2¹⁰** | **2¹¹** | **2¹²** | **2¹³** | **2¹⁴** |
|--------|--------|--------|--------|--------|--------|--------|--------|--------|---------|---------|---------|---------|---------|
| 2      | 4      | 8      | 16     | 32     | 64     | 128    | 256    | 512    | 1,024   | 2,084   | 4,096   | 8,192   | 16,384  |

Trick: each power is double the previous one, so you can half or double from 256 to reach any of them.

# ***<u>Subnet Mask & CIDR:</u>***

A **subnet mask** is a 32-bit value that lets a recipient tell the network/subnet portion of an address from the host portion of an address from the host portion. The 1s mark **network/subnet positions**.

- Default mask can't shrink: Class A can't go below 255.0.0.0, B below 255.255.0.0, C below 255.255.255.0.

- **255.255.255.255** can't be assigned, because all 1s is a broadcast.

| **Class** | **Format**                     | **Default mask**    |
|-----------|--------------------------------|---------------------|
| A         | network.host.host.host         | 255.0.0.0 (/8)      |
| B         | network.network.host.host      | 255.255.0.0 (/16)   |
| C         | network. network. network.host | 255.255.255.0 (/24) |

## <u>CIDR (Classless Inter-Domain Routing):</u>

- **CIDR notation (/26) is simply the number of network bits** (1s) counted across the whole mask. / 26 is the same mask as 255.255.255.192, just shorter.

- It's how ISPs allocate blocks of addresses (e.g., 192.168.10.32/28).

- CIDR replaced the fixed Class A/B/C boundaries, which is why a mask can end anywhere.) including in the third octet of a Class B address.

- Using different mask lengths within one network is **VLSM** (Variable Length Subnet Masking).

- The largest usable mask is **/30**, because you must keep at least 2 host bits.

asks run /8 through /30 in single-bit steps (255.0.0.0, .128.0.0, .192.0.0, .224.0.0, .240.0.0, .248.0.0, 252.0.0, 254.0.0, then 255.255.0.0 and so on).

| **CIDR range** | **Classes that can use it** |
|----------------|-----------------------------|
| /8 - /15       | Class A only                |
| /16 - /23      | Class A & B                 |
| /24 - /30      | Class A, B and C            |

That's a big reason most companies use **Class A** addresses: every mask is available, so Ney get maximum design flexibility,

**Supernetting** is the opposite of subnetting. It combines smaller networks into one address (e.g., a Class C with a /23 mask). CIDR compliant devices allow it (RFC 1518).

# ***<u>The Block-Size Method & the Five Questions (Class C):</u>***

One number, the block size, answers all five questions. **Block size = 256- the mask's last (interesting octet**.

<table>
<colgroup>
<col style="width: 38%" />
<col style="width: 61%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Question</strong></th>
<th><strong>How to answer it</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Q1:</strong> How many subnets?</td>
<td><p>2^(masked bits).</p>
<p>For /26: 2² = 4.</p></td>
</tr>
<tr class="even">
<td><strong>Q2:</strong> How many usable hosts per subnet?</td>
<td><p>2^(host bits)-2.</p>
<p>For /26: 2⁶- 2 = 62</p></td>
</tr>
<tr class="odd">
<td><strong>Q3:</strong> What are the subnet addresses?</td>
<td>Count from 0 in steps of the block size (0, 64, 128, 192)</td>
</tr>
<tr class="even">
<td><strong>Q4:</strong> What's each broadcast address?</td>
<td><p>One less than the next subnet (subnet 0 .63, subnet 64 .127).</p>
<p>The last subnet's broadcast is always .255.</p></td>
</tr>
<tr class="odd">
<td><strong>Q5:</strong> What's the valid host range?</td>
<td>(subnet + 1) through (broadcast - 1).</td>
</tr>
</tbody>
</table>

## <u>Class C Mask Reference:</u>

| **Mask (binary)** | **Decimal** | **CIDR** | **Bits on** | **Block size** | **Subnets** | **Hosts per subnet** | **Subnet addresses (4th octet )** |
|-------------------|-------------|----------|-------------|----------------|-------------|----------------------|-----------------------------------|
| 10000000          | 128         | /25      | 1           | 128            | 2           | 126                  | 0, 128                            |
| 11000000          | 192         | /26      | 2           | 64             | 4           | 62                   | 0, 64, 128, 192                   |
| 11100000          | 224         | /27      | 3           | 32             | 8           | 30                   | 0, 32, 64,96, 128, 160, 192, 224  |
| 11110000          | 240         | /28      | 4           | 16             | 16          | 14                   | 0, 16, 32...240                   |
| 11111000          | 248         | /29      | 5           | 8              | 32          | 6                    | 0, 8, 16... 248                   |
| 11111100          | 252         | /30      | 6           | 4              | 64          | 2                    | 0, 4, 8...252                     |

### *Memorise:*

- **Mask block size**: 128 128, 192 64, 224 32, 240 16, 248 8, 252 4.

- **Mask number of subnets**: 128 2, 192 4, 224 8, 240 16, 248 32, 252 64.

(The Study Guide labels this second list “block size,” but these are actually the subnet counts)

- **/31 and /32** are left out on purpose, since a normal subnet needs at least 2 host bits. (/31 shows up on some point-to-point router links, and /32 as a route to a single host.).

- **/30 gives exactly two hosts, regardless of class**, so it's suited almost exclusively to **point-to-point links**. On a WAN link you only use two addresses: a /24 wastes 252 of them per link, a /30 wastes none.

## <u>Worked Example: 192.168.10.0/27 (255.255.255.254):</u>

I subnets bits, 5 host bits.

- **Q1:** 2³ = 8 subnets. **Q2:** 2⁵ - 2 = **30 hosts**.

- **Q3:** block size 256 – 224 = 32, so subnets are **0, 32, 64, 96, 128, 160, 192, 224.**

- **Q4:** broadcasts are **31, 63, 95, 127, 159, 191, 223, 255.**

- **Q5:** subnet 0 .1 - .30, subnet 32 .33 - .62, subnet 64 .65 - .94, subnet 96 .97 - .126, and the pattern repeats

Memorise the process, not the numbers. Only the block size changes when the mask changes.

The Textbook uses a router and two or more physical networks (Figures 8.1 - 8.2) to show why you subnet:

<img src="./media-ch8/image1.png" style="width:7.75253in;height:2.74092in" />

<img src="./media-ch8/image2.png" style="width:7.91667in;height:4.92708in" />once you add a router, every physical network needs its own logical network. A /25 gives two networks, and a /26 gives fours with room for another router interface.

## <u>Subnetting in Your Head (Class C):</u>

Given a host Ip and mask: find the block size, count up from 0 until you pass the host address, and the host lives in the subnet just below it.

| **Host / mask**     | **Block** | **Subnet** | **Broadcast** | **Valid hosts** |
|---------------------|-----------|------------|---------------|-----------------|
| 192.168.10.33 /224  | 32        | .32        | .63           | .33 - .62       |
| 192.168.10.33 /240  | 16        | .32        | .47           | .33 - .46       |
| 192.168.10.174 /240 | 16        | .160       | .175          | .161 - .174     |
| 192.168.10.17 /252  | 4         | .16        | .19           | .17 - .18       |

# ***<u>Visual Example: One Network Four Subnets (Plus an Ordinary LAN):</u>***

(Study Guide only. The Textbook covers the mechanics, not necessarily these examples.)

## <u>One /24 Four /26s:</u>

Start with one /24 (192.168.10.0, 254 usable hosts, broadcast .255). Borrow **2 bits**, so the mask goes from 255.255.255.0 (/24) to **255.255.255.192 (/26)**. Block size is 64, giving **4 subnets x 62 hosts**:

| **Subnet \#** | **Subnet address** | **First host** | **Last host** | **Broadcast** | **Host** |
|---------------|--------------------|----------------|---------------|---------------|----------|
| 1             | 192.168.10.0       | .1             | .62           | .63           | 62       |
| 2             | 192.168.10.64      | .65            | .126          | .127          | 62       |
| 3             | 192.168.10.128     | .129           | .190          | .191          | 62       |
| 4             | 192.168.10.192     | .193           | .254          | .255          | 62       |

**Key insight**: the /24 still has 256 addresses, now split into 4 blocks of 64. You get 4 x 62 = **248** usable hosts instead of 254. Subnetting “costs” 6 usable addresses, because 3 extra subnets means 6 extra network/broadcast addresses. Each subnet is its own isolated broadcast domain.

**Real-world application**: one office, four departments, each on its own / 26:

| **Department** | **Computers** | **Subnet**        |
|----------------|---------------|-------------------|
| Finance        | 30            | 192.168.10.0/26   |
| HR             | 28            | 192.168.10.64/26  |
| Engineering    | 45            | 192.168.10.128/26 |
| Operations     | 35            | 192.168.10.192/26 |

A router connects the four subnets, allowing controlled inter-department communication while keeping broadcast storms isolated.

## <u>One Ordinary LAN (Before Subnetting):</u>

- A typical home or small-office network is **one /24** (192.168.1.0/24), which is **one broadcast domain**.

- The “gateway” is a home router, which is a **router and a switch in one** device. The switch part connects devices so they talk directly. The router part is used only when traffic **leaves the subnet**.

- Every host uses **192.168.1.1** as its default gateway and gets a unique address in the same subnet.

- Printers and servers are addressed just like any other host.

# ***<u>Subnetting Class B Addresses:</u>***

(Mostly Textbook. The Study Guide just notes the same method on Class B)

Class B has **16 host bits**, so you can use up to **14** for subnetting (leaving at least 2 for hosts). Mask run from /16 (not subnetting, but valid) through /30. The mask-bit pattern never changes by class, because subnet bits start at the left and can't be skipped.

The method is identical to Class C, except you **start in the third octet**. Use the same subnet numbers in the third octet and add .0 at the start and .255 at the end. For /20 (240), block 16: subnet 16.0 broadcast 31.255, then 32.0 47.255.

**Practice: Network 172.16.0.0**

| **Mask**              | **Subnets** | **Hosts** | **Block / subnet list**                                    |
|-----------------------|-------------|-----------|------------------------------------------------------------|
| /17 (255.255.128.0)   | 2           | 32, 766   | 128 in 3rd octet: 0.0, 128.0                               |
| /18 (255.255.192.0)   | 4           | 16, 382   | 64: 0.0, 64.0, 128.0, 192.0                                |
| /20 (255.255.240.0)   | 16          | 4, 094    | 16: 0.0, 16.0, 32.0, 48.0...                               |
| /23 (255.255.254.0)   | 128         | 510       | 2: 0.0, 2.0, 4.0, 6.0, 8.0...                              |
| /24 (255.255.255.0)   | 256         | 254       | 1: 0.0, 1.0, 2.0...                                        |
| /25 (255.255.255.128) | 512         | 126       | Two subnets per 3rd-octet value: 0.0, 0.128, 1.0, 1.128... |
| /26 (255.255.255.192) | 1,024       | 62        | 0.0, 0.64, 0.128, 0.192, 1.0...                            |
| /27 (255.255.255.224) | 2,048       | 30        | 0.0, 0.32, 0.64 ...255.224                                 |

**Notes**:

- 255.255.255.0 on a Class B is **not** “a Class C mask.” It's a Class B network with 8 bits of subnetting.

- Class B **/25** is one of the hardest masks, but useful in production: 512 subnets with 126 hosts each.

- Once the third octet is all 255s in the mask, whatever number is in it is a subnet number, and the fourth octet is subnetted exactly like Class C.

- First host is always **subnet + 1**, last host is **broadcast - 1**. For /20, subnet 0.0 has hosts 0.1 - 15.254 and broadcast 15.255.

## <u>Subnetting Class B in Your Head:</u>

Find the **interesting octet** (the one where the mask isn't 0 or 255), get the block size, and count.

<table>
<colgroup>
<col style="width: 3%" />
<col style="width: 26%" />
<col style="width: 26%" />
<col style="width: 16%" />
<col style="width: 26%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>#</strong></th>
<th><strong>Address / mask</strong></th>
<th><strong>Interesting octet &amp; block</strong></th>
<th><strong>Subnet</strong></th>
<th><strong>Broadcast</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>1</td>
<td>172.16.10.33 /27</td>
<td>4th, block 32</td>
<td>172.16.10.32</td>
<td>172.16.10.63</td>
</tr>
<tr class="even">
<td>2</td>
<td>172.16.66.10 /18</td>
<td>3rd, block 64</td>
<td>172.16.64.0</td>
<td>172.16.127.255</td>
</tr>
<tr class="odd">
<td>3</td>
<td>172.16.50.10 /19</td>
<td>3rd, block 32</td>
<td>172.16.32.0</td>
<td>172.16.63.255</td>
</tr>
<tr class="even">
<td>4</td>
<td>172.16.46.255 /20</td>
<td>3rd, block 16</td>
<td>172.16.32.0</td>
<td><p>172.16.47.255</p>
<p>(so 46.255 is a valid host)</p></td>
</tr>
<tr class="odd">
<td>5</td>
<td>172.16.45.14 /30</td>
<td>4th, block 4</td>
<td>172.16.45.12</td>
<td>172.16.45.15</td>
</tr>
<tr class="even">
<td>6</td>
<td>172.16.88.255 /20</td>
<td>3rd, block 16</td>
<td>172.16.80.0</td>
<td>172.16.95.255</td>
</tr>
</tbody>
</table>

**Trick question**: a router gets a packet for 172.16.46.191 /26. 191 is the broadcast address of the .128 subnet, so by default the router **discards** it.

**ip subnet-zero**: counting from zero relies on this router command, which lets you use the zero subnet as your first subnet. Most modern routers support it.

# ***<u>The Four-Stepping Sequence (Troubleshooting IP):</u>***

work four pings **from the inside out: loopbackown IPdefault gatewayremote host**. The first ping that fails tells you where the problem lives (IP stack, NIC, local segment, or something remote), instead of guessing and chasing the wrong layer.

**Scenario (Figure 8.3)**:

<img src="./media-ch8/image3.png" style="width:7.9in;height:4.30417in" />

Sally can't reach a server on a remote network. From her host:

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 28%" />
<col style="width: 34%" />
<col style="width: 29%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Step</strong></th>
<th><strong>Command</strong></th>
<th><strong>If it works</strong></th>
<th><strong>If it fails</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>1</td>
<td><p>Ping 127.0.0.1</p>
<p>(loopback)</p></td>
<td>The IP stack is initialised</td>
<td>IP stack failure, so reinstall TCP/IP. “Unable to contact IP driver, error code 2” means reinstall the TCP/IP suite.</td>
</tr>
<tr class="even">
<td>2</td>
<td><p>ping the host's <strong>own IP</strong></p>
<p>(172.16.10.2)</p></td>
<td>The NIC is working, and the IP stack can talk to it via the LAN driver. (This does not prove a cable is plugged in)</td>
<td>Problem with NIC</td>
</tr>
<tr class="odd">
<td>3</td>
<td><p>ping the <strong>default gateway</strong></p>
<p>(172.16.10.1)</p></td>
<td>The NIC is plugged into the network and can communicate locally.</td>
<td>Local physical network problem, and where from the NIC to the router.</td>
</tr>
<tr class="even">
<td>4</td>
<td><p>ping the <strong>remote server</strong></p>
<p>(172.16.20.2)</p></td>
<td>IP works end to end, and the remote physical network is working.</td>
<td>Remote physical network problem. Go to the server and repeat steps 1-3.</td>
</tr>
</tbody>
</table>

**All four succeed but the user still can't reach the server?** You probably have a **name resolution** problem, so check DNS settings.

## <u>Command-Line Tools:</u>

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 84%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Tool</strong></th>
<th><strong>What it does</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>ping</strong></td>
<td><p>ICMP echo request/reply.</p>
<p>Tests whether a host's IP stack is initialised and alive.</p></td>
</tr>
<tr class="even">
<td><strong>traceroute</strong></td>
<td><p>Lists routers along the path using TTL timeouts and ICMP errors.</p>
<p>Works on routers, Mac, Linux, not from a Windows command prompt.</p></td>
</tr>
<tr class="odd">
<td><strong>tracert</strong></td>
<td>The same thing, Windows only.</td>
</tr>
<tr class="even">
<td><strong>arp -a</strong></td>
<td>shows IP-to-MAC mappings on a Windows PC.</td>
</tr>
<tr class="odd">
<td><strong>ipconfig /all</strong></td>
<td>Shows the PC's network configuration (command prompt only).</td>
</tr>
</tbody>
</table>

# ***<u>Determining & Fixing IP Address Problems:</u>***

(Textbook only, beyond the ping sequence)

A wrong **IP, mask, or default gateway** is a common misconfiguration. After the four pings:

1.  **Draw out the network and addressing scheme**. It rarely exists, or it's out of date.

2.  **Verify each host's IP, mask, and gateway**.

**Problem 1 (Figure 8.4): Steps 1-3 Work, Step 4 Fails**

<img src="./media-ch8/image4.png" style="width:6.45109in;height:4.20376in" />

All networks use **/27** (block 32):

| **Network** | **Subnet** | **Hosts** | **Broadcast** |
|-------------|------------|-----------|---------------|
| Sales       | 32         | 33-62     | 63            |
| Marketing   | 64         | 65-94     | 95            |
| WAN         | 96         | 97-126    | 127           |

**The fault**: the Lab_B router's default gateway was set to the **broadcast address of the 64 subnet**, which can't be a valid host.

**Problem 2 (Figure 8.5)**

<img src="./media-ch8/image5.png" style="width:6.39198in;height:4.1875in" />

All networks use **/29** (block 8):

| **Network** | **Subnet** | **Hosts** | **Broadcast** |
|-------------|------------|-----------|---------------|
| Sales       | 24         | 25-30     | 31            |
| WAN         | 40         | 41-46     | 47            |
| Marketing   | 80         | 81-86     | 87            |

**The fault**: Server_B was configured with the broadcast address of its subnet.

## <u>Assigning Valid Addresses When a Host Has None:</u>

Look at neighbouring hosts to find the network, mask, and gateway, then work out the host range.

<table>
<colgroup>
<col style="width: 39%" />
<col style="width: 20%" />
<col style="width: 39%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Given</strong></th>
<th><strong>Block &amp; range</strong></th>
<th><strong>Answer</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Subnet 192.168.20.24/29.</p>
<p>Router gets the first usable, server the last.</p></td>
<td>Block 8, range 25-30</td>
<td><p>Server <strong>192.168.20.30</strong>,</p>
<p>mask 255.255.255.248,</p>
<p>gateway 192.168.20.25.</p></td>
</tr>
<tr class="even">
<td>Router Ethernet0 is 192.168.10.33/27</td>
<td>Block 32, subnet 32, range 33-62</td>
<td><p>Host 192.168.10.34-62,</p>
<p>Mask 255.255.255.224,</p>
<p>gateway 192.168.10.33</p></td>
</tr>
<tr class="odd">
<td>RouterA 192.168.10.65/26</td>
<td><p>Block 64,</p>
<p>range 65-126</p></td>
<td><p>HostA .66-.126,</p>
<p>mask 255.255.255.224</p>
<p>gateway .65</p></td>
</tr>
<tr class="even">
<td>RouterB 192.168.10.33/28</td>
<td><p>Block 16,</p>
<p>range 33-46</p></td>
<td><p>HostB .34-.46,</p>
<p>mask 255.255.255.240,</p>
<p>gateway .33</p></td>
</tr>
<tr class="odd">
<td>Serial link</td>
<td><p>Block 4 in the 3rd octet,</p>
<p>range 16.1-19.254</p></td>
<td>S0/0 could be, e.g., <strong>172.16.18.255</strong></td>
</tr>
</tbody>
</table>

# ***<u>Choosing a Mask (Reverse Subnetting) & VLSM:</u>***

## <u>Choosing a Mask for a Requirement:</u>

Work backward from the block size to the mask (use the Class C table in Topic 3).

**Always size for the network needing the most hosts**.

<img src="./media-ch8/image6.jpeg" style="width:8.48056in;height:4.89653in" />**Example (Figure 8.9)**:

one Class C network, 5 subnets needed, and the Chicago office needs 15 users.

- A block of 16 fails: subtract 2 and you only get 14 usable hosts.

- So you need **block size 32 mask 224 (/27)**.

- That gives **8 subnets with 30 hosts each**, which covers both requirements.

## <u>VLSM (Variable Length Subnet Masking):</u>

VLSM uses **different mask sizes inside one network**, introduced alongside CIDR.

Allocate the largest subnet first.

- A /30 for a two-router link and a /26 for a busy office in the same address block wastes a host nothing. One mask for everything would waste most of it.

- The WAN link payoff: /30 on point-to-point links wastes nothing, whereas a /24 would waste 252 addresses per link.

- More detail comes in later chapters.

# ***<u>Introduction to NAT:</u>***

Most **RFC 1918** private addresses to have to reach the public internet eventually, and **NAT (Network Address Translation)** translates them at the border. It's typically configured on a **border router**.

NAT was originally meant to slow the depletion of IP address space by letting many private addresses be represented by fewer public ones. It's also useful for network migrations and mergers, server load sharing and creating “virtual servers.”

### *Best time to use NAT:*

- You need to reach the Internet and your hosts don't have globally unique addresses.

- You change to an ISP that requires you to renumber.

- You need to merge two intranets with duplicate addresses.

| **Advantages**                                        | **Disadvantages**                                 |
|-------------------------------------------------------|---------------------------------------------------|
| Conserve legally registered addresses                 | Translation introduces switching-path delays      |
| Reduces address overlap occurrences                   | Loss of end-to-end IP traceability                |
| Increases flexibility when connecting to the Internet | Certain applications' won't work with NAT enabled |
| Eliminates address renumbering as the network changes |                                                   |

**Key insight**: a source IP behind PAT identifies a **shared public address, not a specific device**. To trace log entry back to a real host, you need the **port number** **and time stamp** too, not the address alone. Knowing which NAT flavour is in play tells you whether that's even possible.

## <u>Types of NAT:</u>

<table style="width:100%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Type</strong></th>
<th><strong>Mapping</strong></th>
<th><strong>Notes</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Static NAT</strong></td>
<td>One-to-one, local to global</td>
<td>Need one real Internet IP for every host</td>
</tr>
<tr class="even">
<td><strong>Dynamic NAT</strong></td>
<td>Unregistered address one from a <strong>pool</strong> of registered addresses</td>
<td>No per-host mapping to configure, but you need enough real addresses for everyone sending to the Internet</td>
</tr>
<tr class="odd">
<td><strong>Overloading (PAT / NAT Overload)</strong></td>
<td><strong>Many-to-one</strong>, using different ports</td>
<td><p>The most popular type, and a form of dynamic NAT.</p>
<p>Uses the Transport layer, so (theoretically) up to <strong>~65,000</strong> hosts share one real IP.</p>
<p>It's the specific reason IPv4 hasn't run out of usable public addresses, and almost certainly what your home router is doing right now.</p></td>
</tr>
</tbody>
</table>

(Professor Messer's 7-minute “Network Address Translation” video covers NAT and PAT.)

## <u>NAT Names:</u>

Addresses used before translation are local. Addresses used after are global (usually public.)

| **Name**           | **Meaning**                                               |
|--------------------|-----------------------------------------------------------|
| **Inside local**   | The sending host's private address, before translation    |
| **Outside local**  | The destination host's address, before translation        |
| **Inside global**  | The inside host's address, after translation              |
| **Outside global** | The outside destination host's address, after translation |

## <u>How NAT Works:</u>

<img src="./media-ch8/image7.jpeg" style="width:7.9in;height:5.28403in" />**Basic translation (Figure 8.11):** <img src="./media-ch8/image8.jpeg" style="width:7.9in;height:5.07569in" />

1.  Host 10.1.1.1 sends an outbound packet to the NAT border router.

2.  The router identifies it as an inside local address headed for an outside network, translates it, and records the translation in the NAT table.

3.  The packet leaves the outside interface with the new source address.

4.  When the external host replies, the router uses the NAT table to translate the inside global address back to the inside local address.

**With PAT (Figure 8.12)**:

<img src="./media-ch8/image8.jpeg" style="width:7.9in;height:5.07569in" />

all inside hosts get translated to one IP address. The NAT table adds **port numbers** alongside the inside local and outside global addresses, and those ports tell the router which host gets the return traffic.

**Port forwarding**: a router or firewall can also translate a packet's **port number** to a destination, typically a TCP/UDP port on a host in the private network behind the NAT router. Based on the received port number, a remote host can reach servers behind the NAT gateway.

# ***<u>Core Insights:</u>***

| **Concept**                                                                    | **Why it matters**                                                                          |
|--------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| Broadcast = one less than the next subnet. Hosts = subnet + 1 to broadcast − 1 | Same rule for every class                                                                   |
| CIDR (/26) is just the number of network bits                                  | CIDR replaced fixed classes, so a mask can end anywhere                                     |
| /30 = 2 hosts, any class                                                       | Perfect for point-to-point WAN links. Class A gets every mask, hence the flexibility        |
| Class B: start in the third octet                                              | 255.255.255.0 on Class B is 8 bits of subnetting, not a "Class C mask"                      |
| Ping inside-out: loopback → own IP → gateway → remote                          | The first failing ping tells you which layer to chase. All pass but still broken? Check DNS |
| Never assign a network or broadcast address to a host                          | Classic misconfiguration in the book's troubleshooting problems                             |
| Size masks for the biggest subnet                                              | 15 users needs block 32 (/27), not 16                                                       |
| VLSM uses different masks in one network, largest subnet first                 | A /30 for a router link and a /26 for an office wastes almost nothing                       |
| NAT types                                                                      | Static = 1:1. Dynamic = pool. PAT = many-to-one with ports                                  |
| PAT maps many private addresses to one public one using ports                  | It's why IPv4 hasn't run out, and likely what your router does                              |
| NAT adds switching-path delay and kills end-to-end traceability                | Tracing a log entry to a host needs port and timestamp, not just the address                |
| Inside/outside, local/global                                                   | Local = before translation. Global = after                                                  |

# ***<u>Abbreviations:</u>***

| **Abbreviation** | **Stands for**                                  |
|------------------|-------------------------------------------------|
| ARP              | Address Resolution Protocol                     |
| CIDR             | Classless Inter-Domain Routing                  |
| DNS              | Domain Name System                              |
| ICMP             | Internet Control Message Protocol               |
| IP               | Internet Protocol                               |
| IPv4             | Internet Protocol version 4                     |
| ISP              | Internet Service Provider                       |
| LAN              | Local Area Network                              |
| MAC              | Media Access Control                            |
| NAT              | Network Address Translation                     |
| NIC              | Network Interface Card                          |
| PAT              | Port Address Translation                        |
| RFC              | Request for Comments                            |
| TCP/IP           | Transmission Control Protocol/Internet Protocol |
| TTL              | Time To Live                                    |
| UDP              | User Datagram Protocol                          |
| VLAN             | Virtual Local Area Network                      |
| VLSM             | Variable Length Subnet Masking                  |
| WAN              | Wide Area Network                               |
