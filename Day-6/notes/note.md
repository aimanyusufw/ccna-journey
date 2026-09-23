Day 6 - Ethernet LAN Switching (Part 2)

> [!info] Course context
> Jeremy's IT Lab — **Free CCNA | Ethernet LAN Switching (Part 2) | Day 6 | CCNA 200-301 Complete Course**
> Video: [YouTube](https://www.youtube.com/watch?v=5q1pqdmdPjo)
> Deeper into Ethernet frames + how hosts resolve IP → MAC with **ARP**. Continues [[Day 5 - Ethernet LAN Switching (Part 1)]].

## Ethernet Frame Details (Part 2)

- **Preamble + SFD are usually NOT counted** as part of the Ethernet header (they're sent with every frame though).
- Real header = Destination (6) + Source (6) + Type (2) = 14 bytes; header + trailer = **18 bytes**.
- **Minimum Ethernet frame size = 64 bytes** (includes payload, excludes preamble/SFD).
- → Minimum **payload (packet) size = 64 − 18 = 46 bytes**.
- If the enclosed packet is smaller than 46 bytes, **padding bytes (0s)** are added. E.g., a 36-byte ping gets **10 bytes** of padding.

> [!tip] EtherTypes to remember
> | Protocol | EtherType |
> |---|---|
> | IPv4 | `0x0800` |
> | IPv6 | `0x86DD` |
> | ARP | `0x0806` |

## The Problem: IP is Known, MAC is Not

- A frame carries MAC addresses (Layer 2) _and_ an encapsulated IP packet with Layer 3 addresses.
- Users specify the **destination IP**, but switches are **Layer 2 devices** that forward based on **MAC addresses**.
- So the sending host must **discover the destination's MAC** before sending → **ARP**.

## ARP (Address Resolution Protocol)

- **ARP** = resolves a known **Layer 3 (IP) address → Layer 2 (MAC) address**.
- Two messages:
  - **ARP Request** — sent as a **broadcast** (destination MAC = `FFFF.FFFF.FFFF`, "Who has 192.168.1.3? Tell 192.168.1.1").
  - **ARP Reply** — sent as a **unicast** back to the requester (since the requester's MAC was in the ARP request's source field).

### ARP Walkthrough (PC1 → PC3 on the 192.168.1.0/24 LAN)

1. PC1 doesn't know PC3's MAC → sends an **ARP Request** (broadcast) with its own IP/MAC as source.
2. SW1 learns PC1's MAC (G0/0); destination MAC is all-Fs → **broadcasts** out G0/1, G0/2.
3. PC2 ignores it (IP doesn't match). SW2 learns PC1's MAC (G0/2); broadcasts out G0/0, G0/1.
4. PC4 ignores it. **PC3** sees its IP → sends an **ARP Reply** (unicast) directly to PC1.
5. SW2 learns PC3 (G0/0); PC1 is **known unicast** → forwards toward SW1 (G0/2).
6. SW1 already knows PC1 (G0/0) → forwards. PC1 learns PC3's MAC and stores it in its **ARP table**.
7. Now PC1 can send its real traffic to PC3 with the proper destination MAC.

### ARP Table

| Viewing command | OS                          |
| --------------- | --------------------------- |
| `arp -a`        | Windows / macOS / Linux     |
| `show arp`      | Cisco IOS (privileged EXEC) |

- Columns (Windows `arp -a`): **Internet address** (IP) | **Physical address** (MAC) | **Type**.
- `type static` = default entry (not learned); `type dynamic` = learned via ARP.

> [!note] Device-role check
> Every device (PC, router, host) keeps an **ARP table**. The _switch_ maintains a **MAC address table** instead — same idea, different device.

## Ping (ICMP)

- **Ping** = utility to test reachability; measures **round-trip time**.
- Uses two messages: **ICMP Echo Request** and **ICMP Echo Reply** (both unicast — that's why ARP must happen first).
- In Cisco IOS: `ping <ip>`, e.g. `ping 192.168.1.3`. Defaults: **5 echo requests, 100 bytes each**.
  - `.` = failed; `!` = success (the first ping often fails because ARP must resolve the MAC first).
- Wireshark lets you view packet captures (`ARP`, `ICMP`, etc.) to see this in practice.

## MAC Address Table (Cisco Switch)

View with `show mac address-table`.

| Column      | Meaning                                                       |
| ----------- | ------------------------------------------------------------- |
| VLAN        | Virtual LAN membership (default **1** for now)                |
| MAC address | The learned MAC                                               |
| Type        | **dynamic** = learned automatically (not manually configured) |
| Ports       | The interface to reach that MAC                               |

- **Aging**: dynamic entries are removed after **5 minutes of inactivity**.

### Clearing the MAC table

| Command                                           | Effect                                |
| ------------------------------------------------- | ------------------------------------- |
| `clear mac address-table dynamic`                 | Removes _all_ dynamic entries         |
| `clear mac address-table dynamic address <MAC>`   | Removes one specific MAC              |
| `clear mac address-table dynamic interface <int>` | Removes all entries for one interface |

## Frame flooding recap

A switch floods out **all interfaces except the source interface** for frames that are:

- **Broadcast** (destination `FFFF.FFFF.FFFF`)
- **Unknown unicast** (destination MAC not in table)

Known unicast frames are forwarded out only the matching port.

## Quiz Recaps (from the video)

1. Long series of `00000000` at the end of a 36-byte ping's Ethernet payload = **B: padding bytes** (payload minimum is 46 bytes).
2. Message sent to _all hosts on the local network_ = **A: ARP Request** (Reply is unicast; both ping messages are unicast).
3. Fields in `show mac address-table` output = **C: VLAN, MAC address, type, and ports** (D is the `arp -a` output on a PC).
4. Frames flooded out all interfaces except the received one = **A: broadcast and unknown unicast**.
5. Clear all dynamic MACs on a specific interface = **D: `clear mac address-table dynamic interface <interface-id>`**.
