# Routing

## Default Gateway Rule

> [!important] Gateway must live in the host's own subnet
> If host is `192.168.1.130 /26` (subnet `.128–.191`), its gateway **must** be in `.129–.190`.
> A gateway of `192.168.1.200` is **invalid** — the host can't reach it.
> This is the #1 most common NetPractice mistake.

---

## Route Entries

A route says: *"to reach network X, hand the packet to next-hop Y."*

| Destination | Meaning |
|-------------|---------|
| `0.0.0.0/0` | **Default route** — "anything with no specific rule, send here" |
| `10.1.2.0/24` | Specific route — only packets for that network |

**Routing logic**: most specific match wins → default route if nothing matches → packet dropped if no default.

---

## Common Fixes

| Symptom | Fix |
|---------|-----|
| Host can't reach anything outside subnet | Add `0.0.0.0/0` → gateway on the host |
| Router can't reach a far subnet | Add specific route on router with correct next-hop |
| Two routers can't talk | Check `/30` link IPs are in same subnet |

---

## The Two Error Messages

| Log message | Root cause | Fix |
|-------------|-----------|-----|
| `destination does not match any interface` | IP/mask mismatch — interfaces that should share a subnet don't | Fix the IP or mask |
| `destination does not match any route` | Route is missing | Add `0.0.0.0/0` → gateway, or specific route on router |

---

## Related

- [[subnet-math]] — compute subnets before setting gateways
- [[network-layers]] — router vs switch
- [[cheat-sheet]] — quick reference
