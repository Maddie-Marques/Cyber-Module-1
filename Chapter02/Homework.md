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
