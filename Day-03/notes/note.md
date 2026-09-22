# Day 3 - OSI Model & TCP/IP Suite

> [!info] Course context
> Jeremy's IT Lab — **Free CCNA | OSI Model & TCP/IP Suite | Day 3 | CCNA 200-301 Complete Course**
> Video: [YouTube](https://www.youtube.com/watch?v=t-ai8JzhHuY)
> Two networking models that structure how networks work. The OSI model is the language network engineers use; TCP/IP is what's actually in use today.

## Why Models & Protocols Matter

- **Networking protocol** = a set of rules defining how network devices and software should work together (e.g., [[Day 2 - Interfaces and Cables|Ethernet cable standards]]).
- **Networking model** = categorizes and provides structure for protocols.
- Without standards: Dell PCs could only talk to Dell, iMacs only to iMacs — no cross-vendor communication. Standards let a **Cisco router talk to a Juniper router to a Dell PC to an Apple iMac**.

## The OSI Model (7 Layers)

- **OSI** = Open Systems Interconnection. Created by the **ISO** (International Organization for Standardization) in the late 1970s/early 1980s.
- An **open standard** (not proprietary), but **not actually in use today** — yet it still shapes how engineers think and _talk_ about networks.

| Layer | Name         | Function                                                                                                                                                                                                                 |
| ----- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------- |
| 7     | Application  | Closest to end user. Interacts with software apps (web browser) via protocols like **HTTP/HTTPS**. Identifies communication partners, synchronizes communication.                                                        |
| 6     | Presentation | Translates between **application format** and **network format**. Handles encryption/decryption, data format translation.                                                                                                |
| 5     | Session      | Controls **dialogues/sessions** between hosts. Establishes, manages, terminates connections (e.g., browser ↔ YouTube).                                                                                                   |
| 4     | Transport    | **Segments and reassembles** data for end-host communication. Provides **host-to-host / process-to-process** communication. Adds Layer 4 header → **Segment**.                                                           |
| 3     | Network      | Connectivity **between different networks** (between LANs). Provides **logical addressing (IP addresses)**, path selection. **Routers operate here**. Adds L3 header → **Packet**.                                       |
| 2     | Data Link    | **Node-to-node** connectivity (PC↔switch, switch↔router). Formats data for the physical medium, detects/corrects errors. Uses its own addressing (MAC). **Switches operate here**. Adds L2 header + trailer → **Frame**. |
| 1     | Physical     | Physical characteristics: voltage levels, max distances, connectors, cable specs. Converts bits to electrical (wired) or radio (wireless) signals. Everything from [[Day 2 - Interfaces and Cables                       | Day 2]] lives here. PDU = **Bit**. |

> [!info] Mnemonics
>
> - **L7 → L1:** All People Seem To Need Data Processing
> - **L1 → L7:** Please Do Not Teach Students Pointless Acronyms

## Encapsulation & Decapsulation

- **Encapsulation**: data moves _down_ the stack; each layer adds its own header (L2 also adds a trailer).
- **Decapsulation**: receiving host removes each layer's additions as data moves _up_ the stack.
- These are **adjacent-layer interaction** (between layers on the same device).

### PDU Progression (Protocol Data Units)

| Stage                            | Composition                       | PDU name    |
| -------------------------------- | --------------------------------- | ----------- |
| Application prepares data        | data                              | **Data**    |
| +L4 header (transport)           | data + L4 hdr                     | **Segment** |
| +L3 header (network)             | data + L4 + L3 hdr                | **Packet**  |
| +L2 header + trailer (data link) | data + L4 + L3 + L2 hdr + trailer | **Frame**   |
| Transmitted over medium          | converted to signals              | **Bit**     |

### Same-Layer vs Adjacent-Layer Interaction

- **Adjacent-layer interaction**: different layers on the _same_ device (encapsulation/de-encapsulation).
- **Same-layer interaction**: the _same_ layer on _different_ devices — e.g., HTTP on YouTube's server ↔ HTTP in your browser. Lets you "ignore" the layers in between.
- The Transport-layer **segment is never changed** as it crosses routers — it's as if there's direct host-to-host communication.

## TCP/IP Suite

- The conceptual model + protocol set **actually used** on the Internet and modern networks (NOT OSI).
- Named for two foundational protocols: **TCP** and **IP**.
- Developed by the **US Dept. of Defense** via **DARPA** (Defense Advanced Research Projects Agency).

### OSI ↔ TCP/IP Mapping

| OSI Model                          | TCP/IP Model                                  |
| ---------------------------------- | --------------------------------------------- |
| Application, Presentation, Session | Application                                   |
| Transport                          | Transport                                     |
| Network                            | Internet                                      |
| Data Link, Physical                | Link (aka Network Interface / Network Access) |

> [!warning] Plus important gotcha
> When engineers say **"there's a Layer 4 problem"** or **"Layer 2 issue"**, they use **OSI numbering** — Layer 4 = Transport, Layer 2 = Data Link — _not_ TCP/IP's numbering.
> A common 5-layer hybrid combines the top 3 OSI layers into one Application layer, keeps Data Link and Physical separate.

## Anatomy of a Cross-Network Trip (Host A → Host B via 2 Routers)

1. Skype on Host A sends video/audio → encapsulated through the stack → frame sent over Ethernet UTP to the first **router**.
2. Router (a Layer 3 device) strips L2 + L3 headers at its Link/Internet layers, reads the **destination IP**, re-encapsulates, forwards (possibly over long-distance fiber).
3. Second router does the same, then sends to Host B.
4. Host B de-encapsulates: frame → packet → segment → data delivered to Skype.
5. Result: **process-to-process communication** (Skype ↔ Skype), achieved via layers that never change the segment.

> [!note] Routers are Layer 3
> A router only needs its Internet (L3) and Link (L2) layers to forward packets — it ignores the higher layers.

## Quiz Recaps (from the video)

1. HTTP data from a YouTube server shown in your browser = **B: same-layer interaction** (both at Layer 7 over HTTP).
2. HTTP data with three headers + one trailer = **C: Frame** (L2 PDU — L4+L3+L2 headers, one L2 trailer).
3. Layers most relevant to a network engineer = **A: transport, network, data link, and physical** (the top 3 belong to app developers).
4. TCP/IP Link layer = OSI **D: data link and physical**.
5. Layer that provides host-to-host communication = **C: Transport** (Application = process-to-process).

## Key Takeaways

- ==OSI = how we _talk_ about networks; TCP/IP = what's actually _running_.==
- ==PDUs: Data → Segment (L4) → Packet (L3) → Frame (L2) → Bit (L1).==
- ==Routers = Layer 3, Switches = Layer 2.==
- ==Same-layer interaction = same layer, different hosts; adjacent-layer = different layers on one host.==
