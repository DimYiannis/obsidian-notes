# Subnet Math

## IP Address = 32 bits

Four octets, each 0–255. The 8 bit-values to memorise:

```
128  64  32  16  8  4  2  1
```

> [!example] Quick decode
> `192 = 128 + 64` · `224 = 128 + 64 + 32` · `252 = 128 + 64 + 32 + 16 + 8 + 4`

---

## The Mask Splits "Locked" from "Free"

- **Network bits** (1s) — locked, identical for every device on the subnet
- **Host bits** (0s) — free to vary, enumerate the devices

Same network portion → same subnet → direct communication.

---

## The Magic Number (Block Size)

For the octet where mask is **not** 255 and **not** 0:

$$\text{block} = 256 - \text{mask\_octet}$$

| CIDR | Mask (last octet) | Block | Usable hosts |
|------|-------------------|-------|--------------|
| /24  | 0                 | 256   | 254          |
| /25  | 128               | 128   | 126          |
| /26  | 192               | 64    | 62           |
| /27  | 224               | 32    | 30           |
| /28  | 240               | 16    | 14           |
| /29  | 248               | 8     | 6            |
| /30  | 252               | 4     | 2            |

Masks march: **128 → 192 → 224 → 240 → 248 → 252** (each adds the next halved bit).

---

## Network, Broadcast, Usable Range

Given a host IP and block size, in the variable octet:

| Term | Formula | Assignable? |
|------|---------|-------------|
| **Network** | largest multiple of block ≤ host octet | ✗ |
| **Broadcast** | network + block − 1 | ✗ |
| **Usable** | network+1 … broadcast−1 | ✓ |

> [!warning] Never assign
> The network address and broadcast address **cannot** be assigned to any device.

### Worked Example

`192.168.1.130 /26` → mask last octet = 192 → block = **64**

Multiples of 64: 0, 64, **128**, 192 → largest ≤ 130 is **128**

| | Value |
|-|-------|
| Network   | `192.168.1.128` |
| Broadcast | `192.168.1.128 + 64 − 1 = 192.168.1.191` |
| Usable    | `.129` → `.190` |

`.130` ✓ and `.150` ✓ share this subnet. `.200` ✗ (lives in next block `.192–.255`).

---

## The /30 Router-Link Trick

Direct router↔router links → always `/30` (block = 4, exactly 2 usable addresses):

```
.0  network  |  .1 usable  |  .2 usable  |  .3 broadcast
```

Assign `.1` to one interface, `.2` to the other.

---

## Related

- [[routing]] — gateways and routes
- [[cheat-sheet]] — quick reference
