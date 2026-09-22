# Day 5 - Ethernet LAN Switching (Part 1)

> [!info] Course context
> Jeremy's IT Lab — **Free CCNA | Ethernet LAN Switching (Part 1) | Day 5 | CCNA 200-301 Complete Course**
> Video: [YouTube](https://www.youtube.com/watch?v=u2n762WG0Vo)
> How frames move **within a LAN** (Layer 2 of the OSI model). Layered on top of the [[Day 3 - OSI Model & TCP-IP Suite|OSI model]] and [[Day 2 - Interfaces and Cables|physical/UTP standards]].

## LAN Review

- **LAN** (Local Area Network) = a network in a relatively small area (an office floor, a home).
- **Routers connect separate LANs**; switches do **NOT** split LANs — adding switches _expands_ a LAN.
- Two switches connected together with their hosts and a router interface = **one LAN**.
- If the two switches connect to _two different router interfaces_ → **two separate LANs**.

## Ethernet Frame Structure

Receiving hosts process frames via de-encapsulation from [[Day 3 - OSI Model & TCP-IP Suite|Day 3]] (Segment → Packet → **Frame**).

### Ethernet Header (5 fields)

| Field                           | Length            | Purpose                                                                                                                                                                |
| ------------------------------- | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Preamble**                    | 7 bytes = 56 bits | Alternating bits `10101010` ×7. Lets the receiver **synchronize its clock**.                                                                                           |
| **SFD** (Start Frame Delimiter) | 1 byte = 8 bits   | `10101011` — marks end of preamble, start of frame.                                                                                                                    |
| **Destination** (MAC)           | 6 bytes = 48 bits | Layer 2 address of the receiving device.                                                                                                                               |
| **Source** (MAC)                | 6 bytes = 48 bits | Layer 2 address of the sending device.                                                                                                                                 |
| **Type / Length**               | 2 bytes = 16 bits | ≤1500 → **length** of packet in bytes. ≥1536 → **type** of encapsulated packet: `0x0800` (2048 decimal) = IPv4, `0x86DD` (34525 decimal) = IPv6. (`0x` = hexadecimal.) |

> [!note] Header + trailer = 26 bytes total
> 7+1+6+6+2 = **22 header bytes**, + 4 trailer bytes = **26 bytes**.

### Ethernet Trailer (1 field)

| Field                          | Length            | Purpose                                                                   |
| ------------------------------ | ----------------- | ------------------------------------------------------------------------- |
| **FCS** (Frame Check Sequence) | 4 bytes = 32 bits | Detects corrupted data via a **CRC** (Cyclic Redundancy Check) algorithm. |

## MAC Addresses

- **MAC** = Media Access Control. A **6-byte (48-bit) physical address** assigned when the device is made → aka **BIA** (Burned-In Address).
- **Globally unique** (except "locally unique" special cases).
- Written as **12 hexadecimal characters** (often grouped as `AAAA.AA00.0001`).
- First **24 bits (3 bytes)** = **OUI** (Organizationally Unique Identifier) — assigned to the _manufacturer_ (e.g., Cisco).
- Last **24 bits (3 bytes)** = unique to the device.
- Compare: **IP address = Layer 3 (32 bits)** and _assigned by you_ in the CLI; **MAC = Layer 2 (48 bits)** and burned in.

> [!tip] Hexadecimal primer
> Based 16: digits 0-9 then A-F (A=10 ... F=15). `0x10` = 1×16 = decimal 16. IPv6 (later in the course) uses this heavily.

## Switch Interface Naming

- Switch ports look like `F0/1`, `F0/2`, `F0/3` where **F = Fast Ethernet (100 Mbps)** on that interface.

## Frame Types & Switch Forwarding

### Unicast frames

- A frame destined for a **single device** (specific destination MAC). (Broadcast/multicast come later.)

### MAC Address Table (dynamic learning)

- Every switch keeps a **MAC address table**.
- Learned **dynamically from the SOURCE MAC address** of incoming frames: source MAC ↔ interface it was received on.
- Entries live for **5 minutes of inactivity** before being purged (on Cisco switches).
- The table entry means "I can reach this MAC via this interface" — it does **not** mean the device is directly connected there.

### Unknown unicast → FLOOD

- Destination MAC **not in the table** = **unknown unicast** → the switch **floods** the frame out **every interface except the one it arrived on**.
- Non-matching hosts **drop** the frame; the matching host processes it.

### Known unicast → FORWARD

- Destination MAC **in the table** = **known unicast** → the switch **forwards** the frame only out of the interface associated with that MAC.

## MAC Learning Walkthrough (2 switches)

1. PC1 → PC3. SW1 learns PC1 (F0/1). PC1 unknown to SW1 → **flood** out F0/2, F0/3. PC2 drops; SW2 receives via F0/3.
2. SW2 learns PC1 → F0/3. Still unknown destination → **flood** out F0/1, F0/2. PC4 drops; PC3 receives.
3. PC3 replies to PC1: SW2 learns PC3; destination PC1 is **known** → **forward** to F0/3.
4. SW1 receives it, learns PC3 → F0/3, and PC1 is **known** → **forward** to F0/1. PC1 gets its reply.

> [!warning] Key concept
> Switches **learn from Source MAC**, **decide with Destination MAC**. `Unknown → flood`, `Known → forward`.

## Quiz Recaps (from the video)

1. Field that synchronizes the receiver clock = **A: Preamble**.
2. Length of a device's physical (MAC) address = **D: 48 bits** (IP is 32 bits).
3. OUI of `E8BA.7011.2874` = **B: E8BA.70** (first 24 bits / 3 bytes).
4. Field used to populate the MAC table = **C: Source MAC address** (associated with the receiving interface).
5. Frame flooded out all interfaces except the source one = **A: Unknown unicast**.
