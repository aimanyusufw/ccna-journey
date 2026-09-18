# Day 2 - Interfaces and Cables

> [!info] Course context
> Jeremy's IT Lab — **Free CCNA | Interfaces and Cables | Day 2 | CCNA 200-301 Complete Course**
> Video: [YouTube](https://www.youtube.com/watch?v=ieTH5lVhNaY)
> How we physically connect the devices from [[Day 1 - Network Devices]] using cables.

## Interfaces (Ports)

- **Interface** = the port on a device where you plug in a cable.
- Switches have lots of interfaces (e.g., 24), like the Cisco Catalyst front panel.
- **RJ-45** (Registered Jack) — the port/connector shape on the back of most PCs and on switches. Used on the end of a _copper Ethernet cable_.
- RJ-45 connectors have **8 pins**; variations in design/color exist but all fit standard RJ-45 ports.

## Why Standards Matter

- **Network protocols/standards** = the "common language" devices agree on so they can communicate (like two people sharing a language).
- Physical standards (connector size/shape, cables) and logical standards (e.g., IP) let different vendors' gear work together.
- Ethernet standards are defined under **IEEE 802.3** (IEEE = Institute of Electrical and Electronics Engineers).

## Bits, Bytes, and Speed

- **Bit** = a 0 or a 1. Data travels over copper as varying electrical signals interpreted as 0s/1s.
- **Byte** = 8 bits. ⚠️ Data _on a hard drive_ is measured in bytes; _network speed_ is in bits per second — a Gigabyte is 8× a Gigabit.
- Network speed units:
  - 1 kilobit = 1,000 bits
  - 1 megabit = 1,000,000 bits
  - 1 gigabit = 1,000,000,000 (1 billion) bits
  - 1 terabit = 1 trillion bits

## Copper Ethernet Standards (UTP)

- **UTP** = **Unshielded Twisted Pair**. "Unshielded" = no metallic shield (vulnerable to interference); the **twisting** helps protect against **EMI** (electromagnetic interference). 8 wires → 4 twisted pairs.

| Informal name             | Speed    | IEEE standard | Max length |
| ------------------------- | -------- | ------------- | ---------- |
| 10BASE-T (Ethernet)       | 10 Mbps  | 802.3         | 100 m      |
| 100BASE-T (Fast Ethernet) | 100 Mbps | 802.3         | 100 m      |
| 1000BASE-T (Gigabit)      | 1 Gbps   | 802.3         | 100 m      |
| 10GBASE-T (10 Gigabit)    | 10 Gbps  | 802.3         | 100 m      |

- **Full-duplex** = both devices send data at the same time using separate wires, no collisions.
- Naming: "BASE" = baseband signaling; "T" = twisted pair. (Out of CCNA scope but good to know.)

## Pin Usage — Who Transmits/Receives Where (10/100BASE-T)

10BASE-T and 100BASE-T use only **2 pairs = 4 wires**: pins **1&2** and pins **3&6**.
Gigabit (1000BASE-T) and 10GBASE-T use **all 4 pairs (8 wires)**: 1&2, 3&6, 4&5, 7&8 — and every pair is **bidirectional** (can send and receive simultaneously).

> [!warning] Key memorization point
>
> - **PCs, routers, firewalls**: transmit on pins **1&2**, receive on pins **3&6**.
> - **Switches** (the different one): receive on pins **1&2**, transmit on pins **3&6**.

## Straight-Through vs. Crossover Cables

- **Straight-through**: pin 1→1, pin 2→2, pin 3→3... Used for _different_ device types (PC↔Switch, Router↔Switch).
- **Crossover**: pairs reversed — pin 1→3, pin 2→6 (and 3→1, 6→2). Used for _same_ device types (Switch↔Switch, Router↔Router, PC↔PC). The transmit pins on one side connect to receive pins on the other.

> [!tip] Auto MDI-X
> Modern devices auto-detect which pins the neighbor is transmitting on and adjust — so straight-through/crossover no longer matters on new gear (ports are labeled **Auto-MDIX**). Old devices without Auto MDI-X still need the right cable type.

## Fiber-Optic Cables

- Sends **light over glass fibers** instead of electrical signals over copper. For longer distances (UTP is capped at 100 m).
- **SFP transceiver** (Small Form-Factor Pluggable) plugs into switches/routers to allow fiber connections — costlier than RJ-45 ports.
- **Two connectors per end**: one to transmit, one to receive (separate cables, unlike copper's separate wire pairs). Tx on one side connects to Rx on the other.
- Cable layers: (1) fiberglass core (light travels here) → (2) reflective **cladding** → (3) protective **buffer** → (4) outer **jacket**.

### Multimode vs. Single-Mode

|               | Multimode (MM)                                | Single-Mode (SM)                            |
| ------------- | --------------------------------------------- | ------------------------------------------- |
| Core diameter | Wider                                         | Narrower (thinner glass)                    |
| Light         | Multiple angles/modes, LED-based transmitters | Single angle/mode, laser-based transmitters |
| Distance      | Longer than UTP, shorter than SM              | Longest                                     |
| Cost          | Cheaper                                       | More expensive (laser transmitters)         |

### Fiber Standards

| Informal name | Speed   | IEEE standard | Cable    | Max length             |
| ------------- | ------- | ------------- | -------- | ---------------------- |
| 1000BASE-LX   | 1 Gbps  | 802.3z        | MM or SM | 550 m (MM) / 5 km (SM) |
| 10GBASE-SR    | 10 Gbps | 802.3ae       | MM       | 400 m                  |
| 10GBASE-LR    | 10 Gbps | 802.3ae       | SM       | 10 km                  |
| 10GBASE-ER    | 10 Gbps | 802.3ae       | SM       | 30 km                  |

## UTP vs. Fiber-Optic — Comparison

| UTP (Copper)                                                       | Fiber-Optic                                |
| ------------------------------------------------------------------ | ------------------------------------------ |
| Cheaper                                                            | More expensive                             |
| Shorter distance (max ~100 m)                                      | Longer distances                           |
| Vulnerable to EMI                                                  | Not vulnerable to EMI                      |
| Cheaper ports (RJ-45)                                              | Costlier ports (SFP); SM costlier than MM  |
| ⚠️ Leaks a faint signal outside the cable — possible security risk | Emits no signal outside — no security risk |
