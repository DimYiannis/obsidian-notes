# Cheat Sheet

> One page. Reproduce this from memory and you pass any level by inspection.

---

## Core Formula

```
block  = 256 − mask_octet
network   = largest multiple of block ≤ host_octet
broadcast = network + block − 1
usable    = network+1 … broadcast−1
```

> [!warning] Never assign `network` or `broadcast` to a device.

---

## CIDR Quick Table

| CIDR | Block | Mask | Usable |
|------|-------|------|--------|
| /24  | 256   | 0    | 254    |
| /25  | 128   | 128  | 126    |
| /26  | 64    | 192  | 62     |
| /27  | 32    | 224  | 30     |
| /28  | 16    | 240  | 14     |
| /29  | 8     | 248  | 6      |
| /30  | 4     | 252  | 2      |

Mask pattern: **128 → 192 → 224 → 240 → 248 → 252**

---

## Rules

| Rule | Detail |
|------|--------|
| Gateway ∈ host's subnet | Gateway must be a usable address on the same subnet as the host |
| Default route | `0.0.0.0/0` → gateway on every host |
| Router-to-router link | Use `/30` — assign `.1` and `.2` |
| Switch | Layer 2, no config needed |

---

## Error → Fix

| Error | Fix |
|-------|-----|
| `does not match any interface` | IP or mask wrong — realign subnets |
| `does not match any route` | Missing route — add `0.0.0.0/0` or specific route |

---

## Related

- [[subnet-math]] — worked examples
- [[routing]] — gateway and route details
- [[network-layers]] — OSI layers
