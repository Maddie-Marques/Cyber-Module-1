STEP 1 — Your answers to the book's Exercise 2.1:
EXCERISE 2.1 — Investigating the Applications and the OSI Model
Instructions — This exercise will help you investigate the OSI model and understand the various layers. Find an application on your computer that interacts with the network. Answer the following questions:

ANSWER: Application Chosen — Discord
I use Discord to stay in contact with friends through voice/video calls, texts/DMs in mutual community & private friend servers (or just private DMs), and screen sharing/streaming while gaming, working, or doomscrolling on our desktops. I also use it for school-related communication, as well as joining community (public) servers on various topics.

Q1 — How do you interact with the application?
ANSWER: I interact with Discord primarily through text chats (or DMs), voice calls, video calls, and screen sharing (streaming my desktop/gameplay for friends to watch during said calls).

Q2 — Does the application support compression/decompression or encryption/decryption?
ANSWER: Yes - to both. Discord compresses audio/video using lossy codecs (e.g., Opus for audio & H.264 for video) before transmission.
For encryption, voice & video calls are protected by two layers. First, DTLS-SRTP secures the media in transit over UDP. On top of that, Discord made their own 'Discord's audio and video end-to-end encryption ("E2EE A/V" or "E2EE" for short)'- which they refer to as their DAVE protocol - which adds end-to-end encryption, so only the participants in a call—not even Discord's own servers (the SFU that routes the call)—can decrypt the actual audio/video content. DAVE uses MLS to securely exchange encryption keys among everyone in the call.
The following is a diagram provided via the Discord website introducing DAVE that visually describes the process a bit better:
<img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/3c752bc1-3096-45fc-87e9-cfe8a259f518" />
Text messages/DMs, however, are NOT end-to-end encrypted, since Discord needs to be able to read them for content moderation purposes.
{Terminology used from research:
— a codec shrinks raw media into a smaller stream for sending, then decompresses it on the receiving end for playback.
— lossy is a process/material, or data-reduction method the permanently removes or discards/dissipates unneeded/redundant information/data/energy to achieve much smaller file sizes.
— Opus is Discord's primary open-source audio codec, used to compress & transmit crystal clear voice data in real-time during voice calls and server channels.
— DTLS = Datagram Transport Layer Security.
— SRTP = Secure Real-Time Transport Protocol.
— SFU = Selective Forwarding Unit.
— MLS = Messaging Layer Security.}

Q3 — How does the application communicate (half-duplex, full-duplex, simplex)?
ANSWER: Voice & video calls are full-duplex. So, my device/PC continuously sends encrypted audio/video up to Discord's SFU — a server that routes calls between participants — while simultaneously receiving audio/video streams from everyone else in the call. Both directions happen at the same time, with no need to take turns — which is the defining trait of full-duplex communication. This stays true even though the call isn't a direct peer-to-peer connection — routing through SFU doesn't change the duplex classification.
Text chat/DM-ing is also effectively full-duplex at the network level, since either person can send a message at any time, without waiting for a response.

Q4 — Which Transport layer protocol does it use to communicate?
ANSWER: Discord uses TCP for text chat/DMs, friend requests & server browsing, where reliable delivery matters. It uses UDP for voice & video calls, where speed matters more than guaranteed delivery of every packet — this is also why DTLS runs over UDP rather than TCP, to avoid connection delays & maintain real-time performance.

Q5 — What is the IP address of your computer?
ANSWER:  192.168.51.23 — this is a private IP address, only used by my home network. My router will translate this to a public IP address (via NAT) when communicating with Discord's servers over the internet.

Q6 — What method of connectivity do you have to the network?
ANSWER: Wi-Fi, via a USB adapter on my PC (since my PC doesn't have a built-in wireless card, and I'm too lazy to run an Ethernet cable all the way to my PC anyway), and Wi-Fi, via the built-in wireless card in my laptop.

Q7 — Is it connected with wired or wireless?
Both are wireless.


STEP 2 — My own OSI stack sketch (7 layers + PDU names) — a phone photo or exported diagram:
ANSWER: OSI seven-layer stack with PDU names:
<img width="1472" height="1200" alt="image" src="https://github.com/user-attachments/assets/e14defc2-e48d-4857-afa3-ad88ce7bc3a6" />


STEP 3 — A step-by-step trace of a real web request through all 5 encapsulation steps:
ANSWER: Below is a diagram representing my five-step encapsulation for a request to unitecafrica.co.za — followed by the written steps:
<img width="1472" height="1200" alt="image" src="https://github.com/user-attachments/assets/bea1df83-934b-4278-bb86-d30c2ca3678e" />
Site traced: unitecafrica.co.za
Step 1 — Data (Application → Session, L7-L5)
My browser sends an HTTPS GET request for www.unitecafrica.co.za. Since it's HTTPS, the request is TLS-encrypted at the Presentation layer & the Session layer tracks this as one, ongoing exchange with the server.

Step 2 — Segments (Transport layer, L4)
The Transport layer wraps the data into a TCP segment. A three-way handshake (SYN → SYN-ACK → ACK) establishes the connection on port 443 (HTTPS), with my laptop assigning a random source port to track the session.

Step 3 — Packets (Network layer, L3)
The Network layer adds IP addressing: source IP - 192.168.51.23 (my laptop), destination IP - the resolved address of Unitec Africa's server (resolved via DNS beforehand — I couldn't find the exact address here, but this is where it would go).

Step 4 — Frames (Data Link layer, L2)
The Data Link layer adds MAC addressing for the next hop only — not the final destination (in other words; my router, not the final server):
Source MAC (my laptop's Wi-Fi adapter): C8-15-4E-B7-1B-82
Destination MAC (my router at: 192.168.51.1): F0-2F-74-E2-F6-94
{Ethernet frame addressed to my router as the first stop.}

Step 5 — Bits (Physical layer, L1)
The frame is converted into binary and sent as a Wi-Fi radio signal from my laptop to my router, which forwards it onward toward the destination server— being Unitec Africa's server.
{Raw bits, transmitted as a Wi-Fi radio signal to my gateway.}

Upon the return trip: the response reverses through the same five layers on the server's end, then decapsulates back up to my laptop's stacks (Bits → Frames → Packets → Segments → Data) until my browser renders the page.


STEP 4 — A diagram of the TCP three-way handshake for a request you traced (or attempted with Wireshark):
ANSWER: I did the activity in Packet Tracer:
