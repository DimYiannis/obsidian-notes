# Network Layers

## Switch vs Router

| | Switch | Router |
|---|--------|--------|
| OSI layer | 2 (Data Link) | 3 (Network) |
| Addresses | MAC | IP |
| Scope | **within** one subnet | **between** subnets |
| Decision basis | MAC table | routing table |
| NetPractice config? | None — transparent | Yes — IPs + routes |

> [!tip] In NetPractice
> Switches are invisible — all your work is on host and router interfaces.

---

## OSI Model (7 layers)

| Layer | Name | Protocols / Units |
|-------|------|-------------------|
| 7 | Application | HTTP, DNS, FTP |
| 6 | Presentation | Encoding, encryption |
| 5 | Session | Session management |
| 4 | Transport | TCP, UDP — ports, reliability |
| 3 | Network | **IP, routing** |
| 2 | Data Link | **MAC, switches, frames** |
| 1 | Physical | Cables, signals, bits |

**Mnemonic (bottom-up):** **P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way

NetPractice lives almost entirely at **layers 2 and 3**.

---

## TCP/IP Model (4 layers)

Collapses OSI 7 into 4:

| TCP/IP Layer | Maps to OSI |
|---|---|
| Application | 5 + 6 + 7 |
| Transport | 4 |
| Internet (IP) | 3 |
| Network Access | 1 + 2 |

---

## TCP vs UDP

| | TCP | UDP |
|---|-----|-----|
| Connection | Yes (3-way handshake) | No |
| Reliability | Ordered, acknowledged | Fire-and-forget |
| Speed | Slower | Faster |
| Use case | HTTP, file transfer | DNS, video, games |

---

## Related

- [[subnet-math]] — IP addressing
- [[routing]] — how routers use layer 3
- [[cheat-sheet]] — quick reference
