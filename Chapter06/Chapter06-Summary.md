# ***<u>TCP/IP Background & the DoD Model vs. OSI Model:</u>***

**TCP/IP** was created by the **DoD** (US Department of Defense) to preserve data integrity and communications even in catastrophic scenarios like war.

## <u>Timeline:</u>

- **1969**: the first **RFC** (Request for Comments) is published. **ARPAnet** is created by **DARPA** (Defence Advanced Research Projects Agency) as a reaction to Sputnik.

- **1974**: TCP arrives.

- **1978**: it's split into TCP and IP.

- **1980**: formally documented.

- **1983**: replaces **NCP** (Network Control Protocol) as ARPAnet's official standard,

Most development happened at **UC Berkeley** alongside **BSD UNIX** (Berkeley Software Distribution), which bundled happened TCP/IP and spread it through academia. That open vendor-neutral approach (with government-run standard testing to prevent proprietary forks) is a big reason it because so dominant.

## <u>DoD Model vs. OSI:</u>

The DoD model condenses OSI's 7 layers into 4. “Internet layer” and “Network layer” describe the same thing, and so do “Host-to-Host” and “Transport.”

<table>
<colgroup>
<col style="width: 21%" />
<col style="width: 16%" />
<col style="width: 62%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>DoD layer</strong></th>
<th><strong>OSI equivalent</strong></th>
<th><strong>What it does</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Process/Application</strong></td>
<td><p>Application,</p>
<p>Presentation,</p>
<p>Session</p></td>
<td>Node-to-node app communication and user-interface specs.</td>
</tr>
<tr class="even">
<td><strong>Host-to-Host</strong></td>
<td>Transport</td>
<td>Reliable end-to-end communication, sequencing, data integrity.</td>
</tr>
<tr class="odd">
<td><strong>Internet</strong></td>
<td>Network</td>
<td>Logical (IP) addressing and routing.</td>
</tr>
<tr class="even">
<td><strong>Network Access</strong></td>
<td><p>Data Link,</p>
<p>Physical</p></td>
<td>Hardware addressing and physical transmission.</td>
</tr>
</tbody>
</table>

# ***<u>The Process/Application Layer, Port by Port:</u>***

Spot the plaintext encrypted pairs: FTP SFTP, Telnet SSH, HTTP HTTPS, SMTP SMTPS, LDAP LDAPS, IMAP IMAPS, POP3 POP3S.

## <u>File Transfer & Remote Access:</u>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 11%" />
<col style="width: 54%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Protocol</strong></th>
<th><strong>Port</strong></th>
<th><strong>What it does</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>FTP</strong> (File Transfer Protocol)</td>
<td>TCP 20/21</td>
<td><p>Transfers files.</p>
<p>Both a protocol and a standalone program.</p>
<p>Supports directory browsing/manipulation, but sends <strong>everything in clear text</strong>, including logins.</p>
<p>An “anonymous” login gets limited access without real credentials.</p></td>
</tr>
<tr class="even">
<td><strong>SSH</strong> (Secure shell)</td>
<td>TCP 22</td>
<td><p>Encrypted remote logins, running programs remotely, and moving files.</p>
<p>Replaced rsh, rlogin, and Telnet.</p></td>
</tr>
<tr class="odd">
<td><strong>SFTP</strong> (SSH File Transfer Protocol)</td>
<td>TCP 22</td>
<td><p>File transfer over an SSH session, hence sharing port 22.</p>
<p>Use it when security matters.</p></td>
</tr>
<tr class="even">
<td><strong>Telnet</strong></td>
<td>TCP 23</td>
<td><p>Terminal emulation, making a remote didn't look locally attached.</p>
<p>Text-mode only, zero <strong>security/encryption</strong>.</p>
<p>Supervised by SSH.</p></td>
</tr>
<tr class="odd">
<td><strong>TFTP</strong> (Trivial File Transfer Protocol)</td>
<td>UDP 69</td>
<td><p>Stripped down FTP:fast and simple, send/receive only.</p>
<p>No directory browsing, no authentication.</p>
<p>Insecure, so rarely exposed publicly.</p></td>
</tr>
<tr class="even">
<td><strong>RDP</strong> (Remote Desktop Protocol)</td>
<td>TCP3389</td>
<td><p>Microsoft's remote <strong>GUI</strong> access (unlike Telnet's text-only).</p>
<p>Server side = Remote Desktop Services,</p>
<p>client side = Remote Desktop Connection.</p></td>
</tr>
</tbody>
</table>

**Real-world tip**: FTP beats email for large files. Many mail servers cap attachments around 10 MB, and since FTP is TCP-based it can sometimes resume interrupted transfers.

## <u>Web & Email:</u>

<table>
<colgroup>
<col style="width: 41%" />
<col style="width: 15%" />
<col style="width: 43%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Protocol</strong></th>
<th><strong>Port</strong></th>
<th><strong>What it does</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>HTTP</strong> (Hypertext Transfer Protocol)</td>
<td>TCP 80</td>
<td><p>Manages browser/server communication.</p>
<p>Unencrypted.</p></td>
</tr>
<tr class="even">
<td><strong>HTTPS</strong> (HTTP Secure)</td>
<td>TCP 443</td>
<td><p>Encrypted HTTP for secure web transactions.</p>
<p>Chromium browsers flag sites without it.</p></td>
</tr>
<tr class="odd">
<td><strong>SMTP</strong> (Simple Mail Transfer Protocol)</td>
<td>TCP 25</td>
<td><strong>Sends</strong> email using a spooled/queued delivery method.</td>
</tr>
<tr class="even">
<td><strong>SMTPS</strong> (SMTP Secure)</td>
<td>TCP 587</td>
<td>Encrypted SMTP via TLS</td>
</tr>
<tr class="odd">
<td><strong>POP3</strong> (Post Office Protocol v3)</td>
<td>TCP 110</td>
<td><p><strong>Receives</strong> mail by downloading in bulk (no selective download), then ends the session.</p>
<p>Increasingly replaced by IMAP.</p></td>
</tr>
<tr class="even">
<td><strong>POP3 over SSL</strong></td>
<td>TCP 995</td>
<td>Encrypted POP3</td>
</tr>
<tr class="odd">
<td><strong>IMAP4</strong> (Internet Message Access Protocol v4)</td>
<td>TCP 143</td>
<td><p>Granular control over mail download (headers, partial messages).</p>
<p>Server-side folder hierarchies, search, and <strong>Kerberos</strong> authentication.</p></td>
</tr>
<tr class="even">
<td><strong>IMAP over SSL</strong></td>
<td>TCP 993</td>
<td>Encrypted IMAP.</td>
</tr>
<tr class="odd">
<td><strong>TLS/SSL</strong> (Transport Layer Security/Secure Sockets Layer)</td>
<td>Commonly TCP 995/465, but not fixed</td>
<td>Cryptographic protocols using <strong>X.509</strong> certs and asymmetric crypto to authenticate and exchange keys for encrypting session data.</td>
</tr>
</tbody>
</table>

## <u>Network Services:</u>

<table>
<colgroup>
<col style="width: 28%" />
<col style="width: 22%" />
<col style="width: 49%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Protocol</strong></th>
<th><strong>Port</strong></th>
<th><strong>What it does</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>DNS</strong> (Domain Name System)</td>
<td>TCP &amp; UDP 53</td>
<td><p>Resolves FQDNs to IP addresses.</p>
<p>A device can auto-append a domain suffix (e.g., Cisco's ip domain-name command) so you can type short names.</p>
<p><strong>Can ping an IP but not its FQDN? Suspect DNS</strong>.</p></td>
</tr>
<tr class="even">
<td><strong>DHCP/BootP</strong></td>
<td>UDP 67/68</td>
<td>Assigns IP info dynamically (details below).</td>
</tr>
<tr class="odd">
<td><strong>NTP</strong> (Network Time Protocol)</td>
<td>UDP 123</td>
<td>Sychronises clocks, critical for time-stamped transactions and logs.</td>
</tr>
<tr class="even">
<td><strong>SNMP</strong> (Simple Network Management Protocol)</td>
<td><p>UDP 161/162 (v1/v2),</p>
<p>TCP &amp; UDP 161 (v3)</p></td>
<td>Polls devices for health info (details below).</td>
</tr>
<tr class="odd">
<td><strong>LDAP</strong> (Lightweight Directory Access Protocol)</td>
<td>TCP 389</td>
<td><p>Queries directory services like Active Directory.</p>
<p>Current version standardised in RFC 33771.</p></td>
</tr>
<tr class="even">
<td><strong>LDAPS</strong> (LDAP Secure)</td>
<td>TCP 636</td>
<td><p>Encrypted LDAP.</p>
<p>Needs a proper CA certificate.</p></td>
</tr>
<tr class="odd">
<td><strong>SMB</strong> (Server Message Block)</td>
<td>TCP445 (also UDP 137/138, TCP 137/139 via NetBIOS)</td>
<td>Windows file/printer sharing and host communication.</td>
</tr>
<tr class="even">
<td><strong>NetBIOS</strong> (Network Basic Input/Output System)</td>
<td>TCP/UDP 137-139</td>
<td>Upper-layer interface originally for IBM LANs, later adopted by Microsoft as the de facto host-to-server standard.</td>
</tr>
<tr class="odd">
<td><strong>Syslog</strong></td>
<td>UDP 514</td>
<td>Centralised logging (details below).</td>
</tr>
<tr class="even">
<td><strong>IGMP</strong> (Internet Group Management Protocol)</td>
<td>No port</td>
<td>Manages IP multicast group membership at the Network layer.</td>
</tr>
</tbody>
</table>

## <u>Databases:</u>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 11%" />
<col style="width: 63%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Protocol</strong></th>
<th><strong>Port</strong></th>
<th><strong>What it is</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>SQL Server</strong></td>
<td>TCP 1433</td>
<td>Microsoft's database engine default port (official IANA-assigned)</td>
</tr>
<tr class="even">
<td><strong>SQLnet / SQL*Net / Net8</strong></td>
<td>TCP 1521</td>
<td><p>Oracle's networking layer for remote database access.</p>
<p>All three names are the same thing.</p></td>
</tr>
<tr class="odd">
<td><strong>MySQL</strong></td>
<td>TCP 3306</td>
<td>Open relational database, common for cloud-based databases.</td>
</tr>
</tbody>
</table>

## <u>Voice & Video:</u>

<table>
<colgroup>
<col style="width: 40%" />
<col style="width: 21%" />
<col style="width: 38%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Protocol</strong></th>
<th><strong>Port</strong></th>
<th><strong>What it does</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>SIP</strong> (Session Initiation Protocol)</td>
<td><p>TCP/UDP 5060,</p>
<p>TCP 5061</p></td>
<td><p>Signaling to set up/tear down multimedia sessions (voice, video, IM).</p>
<p>Often paired with RTP.</p></td>
</tr>
<tr class="even">
<td><strong>RTP</strong> (Real-time Transport Protocol)</td>
<td>UDP 5004 / TCP 5005</td>
<td><p>Packet format for delivering audio/video.</p>
<p>Core to VoIP.</p></td>
</tr>
<tr class="odd">
<td><strong>MGCP</strong> (Media Gateway Control Protocol)</td>
<td>TCP 2427/2727</td>
<td>Signaling/session management between media gateways and controllers for multimedia conferencing.</td>
</tr>
<tr class="even">
<td><strong>H.323</strong></td>
<td>TCP 1720</td>
<td><p>Real-time audio/video/data over IP.</p>
<p>Uses RTP underneath.</p></td>
</tr>
</tbody>
</table>

## <u>DHCP in Detail:</u>

- **Bootp** needed manual MAC-to-IP table entries and could serve a bootable OS, which DHCP can't.

- Common info provided: IP address, subnet mask, domain name, default gateway, DNS.

- DHCP is a **connectionless (UDP)**.

- **DORA** process:

1.  **Discover**: client broadcast (port 67).

2.  **Offer**: server unicasts an offer.

3.  **Request**: client broadcasts a request.

4.  **Acknowledgement**: server unicasts an ACK.

- **APIPA** (Automatic Private IP Addressing): with no DHCP server, Windows hosty self-assign from **169.254.0.1 - 169.254.255.254** (Class B mask 255.255.0.0).

Seeing this on a host that should have a DHCP lease is a reg flag that DHCP failed.

## <u>SNMP in Detail:</u>

- A management station (**NMS**, Network Management Station) polls devices for health info.

- **Agents** send **traps** (alerts) when something's wrong.

- The **MIB** (Management Information Base) is the database of predefined queries the NMS can ask agents.

- **v1/v2**: largely obsolete and vulnerable to sniffing (plaintext). v2 added **GETBULK** for large data sets but never caught on.

- v3: the current standard. Adds real security (authentication, encryption, integrity) and uses both TCP and UDP.

## <u>Syslog in Detail:</u>

Centralised logging with timestamps, sequencing, search by keyword or severity, and email alerting. Severity levels run from Emergency (0, most severe) down to Debugging (7, least severe). Informational is the default logging level.

## ***<u>TCP vs. UDP (the Host-to-Host Layer)</u>***

The Host-to-Host layer shields upper-layer apps from network complexity. It takes the data stream and instructions and gets them ready to send. The choice between TCP and UDP is the **application developers** call, not the user's.

**Phone call vs. postcard**: TCP is a phone call. You establish a connection, confirm (“did you get that?”), and formally end the call. UDP is a postcard: no setup, no confirmation, just send it.

| **TCP** (Transmission Control Protocol) | **UDP** (User Datagram Protocol) |
|-----------------------------------------|----------------------------------|
| Sequenced                               | Unsequenced                      |
| Reliable                                | Unreliable                       |
| Connection-orientated (virtual circuit) | Connectionless, low overhead     |
| Acknowledgements                        | No acknowledgements              |
| Windowing / flow control                | No windowing or flow control     |

## <u>TCP:</u>

- Breaks app data into **numbered, sequenced segments**, waits for acknowledgement, and **retransmits** anything unacknowledged.

- Connection-oriented, **full duplex**, and reliable. Needs a **three-way handshake** (call setup) before data transfer, and 9 formal teardown after.

- All that reliability makes TCP complex and costly in overhead, often more than modern reliable networks need. Most programmers still prefer it, since it removes a lot of manual error handling.

### *TCP header fields:*

| **Field**                   | **Purpose**                                                     |
|-----------------------------|-----------------------------------------------------------------|
| **Source/Destination Port** | App identifiers                                                 |
| **Sequence Number**         | Reorders/retransmits data (“sequencing”)                        |
| **Acknowledgement Number**  | The next expected octet                                         |
| **Header Length**           | In 32-bit words                                                 |
| **Reserved**                | Always zero                                                     |
| **Code Bits/Flags**         | Session setup/teardown control                                  |
| **Window**                  | How much data the sender can accept, in octets                  |
| **Checksum**                | CRC covering header and data                                    |
| **Urgent**                  | Offset to non-urgent data. Only valid if the urgent flag is set |
| **Options**                 | Padded to 32-bit boundaries if used                             |
| **Payload/Data**            | The actual data                                                 |

The header itself is **24 bytes (up to 60 with options)**, a lot of overhead per segment.

## <u>UDP:</u>

- The “thin protocol”: no sequencing, no acknowledgements, no virtual circuit, no flow control. It just sends and forgets, so it's unreliable but **fast and low-overhead**.

- Right choice when reliability is handled elsewhere (e.g., **DNS** manages its own), or when per-message overhead would bog things down (e.g., frequent **SNMP** status updates).

- Real-time **video/VoIP** usually favour UDP for its low overhead.

# ***<u>Port Numbers: Well-Known vs. Dynamic:</u>***

TCP and UDP use **port numbers** to track simultaneous conversations.

| **Range**       | **Name**                        | **Who uses it**                                                                     |
|-----------------|---------------------------------|-------------------------------------------------------------------------------------|
| **0-1023**      | **Well-known ports** (RFC 3232) | Tied to specific services (see tables in topic 2)                                   |
| **1024 and up** | **Dynamic** ports               | The source host assigns its own source port from here for every outbound connection |

- The **source port** (dynamic) identifies the conversation on the sender's side.

- The **destination port** tells the recieving host which application/process the data is actually for.

- **DNS** is one of the only protocols that legitimately uses both **TCP and UDP**, depending on the operation.

# ***<u>The Internet Layer: IP and Its Helpers:</u>***

The Internet layer exists for two reasons:

1.  **Routing**

2.  Giving upper-layer protocols **one single, consistent network interface**, so app developers don't need separate versions of their software for every Network Access protocol

## <u>IP (Internet Protocol):</u>

IP essentially *is* the Internet layer. Every other protocol here just supports it.

- It looks at each packet's destination, consults a **routing table**, and picks the best path.

- It **fragments** segments into packets and reassembles them on the receiving end.

- Identifying a device takes both a **logical address** (which network: the IP address) and a **hardware address** (which specific device: the MAC address).

### *IP header fields:*

<table>
<colgroup>
<col style="width: 41%" />
<col style="width: 58%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Field</strong></th>
<th><strong>Purpose</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Version</strong></td>
<td>IP version</td>
</tr>
<tr class="even">
<td><strong>HLEN</strong> (Header Length)</td>
<td>Length of the header</td>
</tr>
<tr class="odd">
<td><strong>Priority / Type of Service</strong></td>
<td>How to handle the datagram</td>
</tr>
<tr class="even">
<td><strong>Total Length</strong></td>
<td>Length of whole packet</td>
</tr>
<tr class="odd">
<td><strong>Identification</strong></td>
<td>Distinguishes fragments from different datagrams</td>
</tr>
<tr class="even">
<td><strong>Flags</strong></td>
<td>Whether fragmentation is allowed</td>
</tr>
<tr class="odd">
<td><strong>Fragment Offset</strong></td>
<td>Handles reassembly and differing MTUs</td>
</tr>
<tr class="even">
<td><strong>TTL</strong> (Time To Live)</td>
<td>Stops packets circling forever</td>
</tr>
<tr class="odd">
<td><strong>Protocol</strong></td>
<td><p>Which upper-layer protocol gets the data:</p>
<p><strong>TCP = 6</strong>, <strong>UDP=17</strong>, or another Network-layer protocol like ARP/ICMP</p></td>
</tr>
<tr class="even">
<td><strong>Header Checksum</strong></td>
<td>CRC on the header only</td>
</tr>
<tr class="odd">
<td><strong>Source/Destination IP Address</strong></td>
<td>Who sent it, who it's for</td>
</tr>
<tr class="even">
<td><strong>Options</strong></td>
<td>Optional extras</td>
</tr>
<tr class="odd">
<td><strong>Data</strong></td>
<td>The payload</td>
</tr>
</tbody>
</table>

## <u>ICMP (Internet Control Message Protocol):</u>

IP's management/messaging service, carried inside IP packets. It reports problems and powers Ping and Traceroute.

| **Message**                 | **Meaning**                                      |
|-----------------------------|--------------------------------------------------|
| **Destination Unreachable** | A router can't forward the packet any further    |
| **Butter Full**             | A router's receive buffer is full                |
| **Hops**                    | TTL exceeded, so the packet was deleted          |
| **Ping**                    | Echo request/reply to Check connectivity         |
| **Traceroute**              | Uses TTL timeouts to map the path a packet takes |

## <u>ARP & RARP:</u>

- **ARP** (Address Resolution Protocol): the “detective” protocol. Resolves a known **IP MAC** by broadcasting a request on the local network.

- **RARP** (Reverse ARP): the reverse, **MAC IP**, for diskless machines that know their MAC but not their IP. They querya RARP server for an assignment.

## GRE & IPSec :

**GRE** (Generic Routing Encapsulation) tunnels other protocols (like EIGRP, OSPF, IPv6) inside IP. It's stateless, has no flow control, and critically has **no security of its own**. Overhead is at least 24 bytes per tunneled packet.

**IPSec** (IP Security) adds that missing security, but it **doesn't support IP broadcast/multicast or multiprotocol traffic**. That's exactly why **GRE over IPSec** is common: GRE's flexibility (carrying broadcast, multicast, and non-IP traffic) plus IPSec’s encryption.

### *IPSec's two core protocols:*

|                    | **AH** (Authentication Header)                                             | **ESP** (Encapsulating Security Protocol)                                                                                                   |
|--------------------|----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| **Authentication** | Yes, of the **entire packet** (including the IP header) via a one-way hash | Yes                                                                                                                                         |
| **Encryption**     | **None**                                                                   | Yes (confidentially)                                                                                                                        |
| **Other**          |                                                                            | Data integrity, **anti-replay** protection (via sequence numbers), limited traffic-flow confidentiality (most effective in **tunnel mode**) |
| **Algorithms**     |                                                                            | TripleDES-CBC, AES-CBC, AES-GCM, ChaCha20-Poly1305                                                                                          |

## <u>IKE (Internet Key Exchange):</u>

IKE negotiates the **SAs** (security associations): the agreed authentication, encryption, and IPSec protocols between endpoints. It uses **ISAKMP** (Internet Security Association and key Management Protocol) across two phases:

1.  **Phase 1 (Main Mode)**: negotiates the **HAGLE** (Hash, Authentication, Group, Lifetime Encryption) and builds an initial encrypted tunnel.

2.  **Phase 2 (Quick Mode)**: uses that tunnel to negotiate the actual **IPSec transform set** (AH/ESP details, encryption, hashing, mode) for the real IPSec connection.

# ***<u>Data Encapsulation, Step by Step:</u>***

(ach layer wraps the data with its own control info using **PDUs** (Protocol Data Units). Only the matching per layer on the receiving device reads that info, then strips it off before handing the data up.

**Data Segment Packet/Datagram Frame - Bits**

## <u>Sending Side:</u>

1.  **Transport (Host-to-Host) segment**: a virtual circuit is set up (a sync packet), the data stream is broken into pieces, a Transport header is attached, and each segment is sequenced. Port numbers defire both the virtual circuit (**source port**, dynamically chosen from 1024 up) and the destination app (**destination port**).

2.  **Network (Internet) packet**: logical (IP) addressing is added. The header's Protocol field records whether the segment came from TCP or UDP, so the received knows where to hand it back up. The Network layer also resolves the destination hardware address via **ARP**:

- **Local destination**: ARP for that host directly.

- **Remote destination**: ARP for the **default gateway's** MAC address instead.

3.  **Data Link (Network Access) frame**: the packet gets a **header and a trailer** (hence “frame,” like bookends). An **Ether-Type** field names the Network-layer protocol inside, and a **CRC** runs, with the result going in the trailer's **FCS** (Frame Check Sequence) field.

4.  **Physical bits**: the frame is encoded into a digital signal using bit-timing rules.

## <u>Receiving Side (Every Stop in Reverse):</u>

1.  Devices sync to the signal, decode the bits, and rebuild the frame.

2.  They run their own CRC and compare it to the FCS. If it matches, de-encapsulation continues.

3.  The packet is pulled from the frame (frame discarded).

4.  The Network layer checks the address and pulls the segment from the packet (packet discarded.

5.  The Transport layer reassembles the data stream, **acknowledges receipt**, and hands the finished stream up to the Application layer.

# ***<u>Core Insights:</u>***

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 66%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Concept</strong></th>
<th><strong>The thing to remember</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>DoD vs. OSI</strong></td>
<td><p>Process/Application = OSI 5-7.</p>
<p>Host-to-Host = Transport.</p>
<p>Internet = Network.</p>
<p>Network Access = Data Link + Physical</p></td>
</tr>
<tr class="even">
<td><strong>Plaintext encrypted pairs</strong></td>
<td><p>FTP (20/21) SFTP (22).</p>
<p>Telnet (23) SSH (22).</p>
<p>HTTP (80) HTTPS (443).</p>
<p>SMTP (25) SMTPS (587).</p>
<p>LDAP (389) LDAPS (636)</p>
<p>IMAP (143) 993.</p>
<p>POP3 (110) 995.</p></td>
</tr>
<tr class="odd">
<td><strong>Other key ports</strong></td>
<td>DNS 53 (TCP &amp; UDP), DHCP 67/68, TFTP 69, NTP 123, SNMP 161/162, SMB 445, Syslog 514, SQL Server 1433, Oracle 1521, MySQL 3306, RDP 3389, SIP 5060/5061, H.323 1720.</td>
</tr>
<tr class="even">
<td><strong>DORA</strong></td>
<td>Discover (broadcast) Offer Request (broadcast) Acknowledge</td>
</tr>
<tr class="odd">
<td><strong>APIPA</strong></td>
<td>169.254.x.x = DHCP failed</td>
</tr>
<tr class="even">
<td><strong>SNMP</strong></td>
<td><p>Agents sends traps, MIB holds the queries.</p>
<p>v3 is the secure one.</p></td>
</tr>
<tr class="odd">
<td><strong>Syslog</strong></td>
<td><p>0 = Emergency (worst) to 7 = Debugging.</p>
<p>Informational is default.</p></td>
</tr>
<tr class="even">
<td><strong>TCP vs. UDP</strong></td>
<td><p>TCP = phone call (handshake, sequenced,acked, reliable).</p>
<p>UDP = postcard (fast, no guarantees).</p></td>
</tr>
<tr class="odd">
<td><strong>Ports</strong></td>
<td><p>0-1023 well-known.</p>
<p>Source ports are dynamic from 1024 up.</p>
<p>Destination port picks the app.</p></td>
</tr>
<tr class="even">
<td><strong>IP</strong></td>
<td><p>Routes and fragments.</p>
<p>Protocol field: TCP = 6, UDP = 17-</p>
<p>TTL stops endless loops.</p></td>
</tr>
<tr class="odd">
<td><strong>ICMP</strong></td>
<td>Destination Unreadable, Buffer Full, Hops, Ping, Traceroute.</td>
</tr>
<tr class="even">
<td><strong>ARP / RARP</strong></td>
<td><p>ARP = IP MAC.</p>
<p>RARP = MAC IP (diskless machines).</p></td>
</tr>
<tr class="odd">
<td><strong>GRE VS. IPSec</strong></td>
<td><p>GRE tunnels anything but has no security.</p>
<p>IPSec secures but can't carry broadcast/multicast.</p>
<p>Hence GRE over IPSec.</p></td>
</tr>
<tr class="even">
<td><strong>AH vs. ESP</strong></td>
<td><p>AH = authenticates whole packet, no encryption.</p>
<p>ESP = encryption + integrity + anti-replay.</p></td>
</tr>
<tr class="odd">
<td><strong>IKE</strong></td>
<td><p>Phase 1 (Main Mode) = HAGLE + initial tunnel.</p>
<p>Phase 2 (Quick Mode) = IPSec transform set.</p></td>
</tr>
<tr class="even">
<td><strong>Encapsulation</strong></td>
<td><p>Data Segment Packet Frame Bits.</p>
<p>Remote destination = ARP for the default gateway.</p>
<p>Receiver checks CRC against FCS first.</p></td>
</tr>
</tbody>
</table>

# ***<u>Abbreviations:</u>*** 

| **Abbreviations** | **Stands for**                                            |
|-------------------|-----------------------------------------------------------|
| **AES**           | Advanced Encryption standard                              |
| **AH**            | Authentication Header                                     |
| **APIPA**         | Automatic Private IP Addressing                           |
| **ARP**           | Address Resolution Protocol                               |
| **ARPAnet**       | Advanced Research Projects Agency                         |
| **BootP**         | Bootstrap Protocol                                        |
| **BSD**           | Berkeley Software Distribution                            |
| **CA**            | Certificate Authority                                     |
| **CBC**           | Cipher Block Chaining                                     |
| **CRC**           | Cyclic Redundancy Check                                   |
| **DARPA**         | Defense Advanced Research Projects Agency                 |
| **DES**           | Data Encryption Standard                                  |
| **DHCP**          | Dynamic Host Configuration Protocol                       |
| **DNS**           | Domain Name System                                        |
| **DoD**           | Department of Defense                                     |
| **DORA**          | Discover, Offer, Request, Acknowledge                     |
| **ESP**           | Encapsulating Security Payload                            |
| **FCS**           | Frame Check Sequence                                      |
| **FQDN**          | Fully Qualified Domain Name                               |
| **FTP**           | File Transfer Protocol                                    |
| **GCM**           | Galois/Counter Mode                                       |
| **GRE**           | Generic Routing Encapsulation                             |
| **GUI**           | Graphical User Interface                                  |
| **HAGLE**         | Hash, Authentication, Group, Lifetime, Encryption         |
| **HLEN**          | Header Length                                             |
| **HTTP / HTTPS**  | Hypertext Transfer Protocol / Secure                      |
| **IANA**          | Internet Assigned Numbers Authority                       |
| **ICMP**          | Internet Control Message Protocol                         |
| **IGMP**          | Internet Group Management Protocol                        |
| **IKE**           | Internet Key Exchange                                     |
| **IMAP4**         | Internet Message Access Protocol version 4                |
| **IP**            | Internet Protocol                                         |
| **IPSec**         | Internet Protocol Security                                |
| **ISAKMP**        | Internet Security Association and Key Management Protocol |
| **LDAP / LDAPS**  | Lightweight Directory Access Protocol / Secure            |
| **MAC**           | Media Access Control                                      |
| **MGCP**          | Media Gateway Control Protocol                            |
| **MIB**           | Management Information Base                               |
| **MTU**           | Maximum Transmission Unit                                 |
| **NCP**           | Network Control Protocol                                  |
| **NetBios**       | Network Basic Input/Output System                         |
| **NMS**           | Network Management Station                                |
| **NTP**           | Network Time Protocol                                     |
| **OSI**           | Open Systems Interconnection                              |
| **PDU**           | Protocol Data Unit                                        |
| **POP3**          | Post Office Protocol version 3                            |
| **RARP**          | Reverse Address Resolution Protocol                       |
| **RDP**           | Remote Desktop Protocol                                   |
| **RFC**           | Request for comments                                      |
| **RTP**           | Real-time Transport Protocol                              |
| **SA**            | Security Association                                      |
| **SFTP**          | SSH File Transfer Protocol                                |
| **SIP**           | Session Initiation Protocol                               |
| **SMB**           | Server Message Block                                      |
| **SMTP / SMTPS**  | Simple Mail Transfer Protocol / Secure                    |
| **SNMP**          | Simple Network Management Protocol                        |
| **SQL**           | Structured Query Language                                 |
| **SSH**           | Secure Shell                                              |
| **SSL**           | Secure Socket Layer                                       |
| **TCP**           | Transmission Control Protocol                             |
| **TFTP**          | Trivial File Transfer Protocol                            |
| **TLS**           | Transport Layer Security                                  |
| **TTL**           | Time To Live                                              |
| **UDP**           | User Datagram Protocol                                    |
| **VoIP**          | Voice over Internet Protocol                              |
