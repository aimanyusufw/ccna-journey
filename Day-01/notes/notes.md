## A Computer Network

- A **computer network** is a _digital telecommunications network_ that allows **nodes** to share resources.
- A **node** is any device on a network (anything connected to it) — computer, phone, server, router, switch, etc.
- A **host** is any node that sends/receives data (a computer, phone, server...).

## The Network Devices

### Client

- A device that **accesses a service** made available by a server.
- Example: your PC, your phone.

### Server

- A device that **provides a service** to clients.
- Example: a web server, a file server, a mail server.
- The _same device_ can be a client in one situation and a server in another.
- Clients and servers are also known as **end hosts** or **endpoints**.

### Switch 🔀

- Aggregates connections from end hosts (PCs, servers, printers).
- **Many network interfaces/ports** — usually 24 or more.
- Forwards traffic **within a LAN** (Local Area Network).
- **Does NOT** provide connectivity between LANs or over the Internet.
- Example hardware: Cisco Catalyst switch (enterprise-grade fleet).

### Router 🌐

- Provides connectivity **BETWEEN LANs** and over the **Internet**.
- Moves data from one LAN to another (e.g., New York branch → Tokyo branch via Internet).
- Has **fewer network interfaces** than a switch (e.g., Cisco ISR 900 series).
- Can provide some basic security features, but that's not its main job.
- Example: PCs connect to a switch → switch connects to router → router connects to Internet.

### Firewall 🛡️

- Specialty network security device that **monitors and controls traffic entering/exiting** the network.
- Must be configured with **security rules** that determine what is allowed/denied.
- Can be placed _outside_ the router (between router and Internet) or _inside_ the network.
- Two types:
  - **Network firewall** — hardware device that filters traffic between networks (the kind focused on in this course).
  - **Host-based firewall** — software app filtering traffic entering/leaving a single host machine (like the firewall on your own PC).

## How It All Fits Together (NY → Tokyo Example)

1. **PC1 (New York)** and **PC2** connect to **SW1** (switch) → forms a LAN.
2. **SRV1 (Tokyo)** and **SRV2** connect to **SW2** (switch) → forms a second LAN.
3. Switches can't reach the Internet, so each switch connects to a **router** (R1, R2).
4. Routers connect to the **Internet**, connecting the two LANs.
5. **Firewalls** (FW1 outside R1, FW2 inside the Tokyo LAN) filter incoming traffic and block the attacker.
