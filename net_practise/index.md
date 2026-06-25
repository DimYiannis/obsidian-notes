# NetPractice — Map of Content

> Core insight: two devices talk directly **only if they share the same network portion**.
> Different network → must go through a [[routing|router]].

---

## Notes

- [[subnet-math]] — block size, network address, broadcast, usable range
- [[routing]] — gateways, routes, default route `0.0.0.0/0`
- [[network-layers]] — OSI 7-layer & TCP/IP model, switch vs router
- [[cheat-sheet]] — one-page study card for the defense
- [[netpractice-basics.canvas|Basics — start here (canvas)]] — what is a subnet/router/gateway, in plain words
- [[netpractice-schema.canvas|Visual schema (canvas)]] — topology + decision flow + formula cards

---

## The Defense Checklist

1. Compute block size: `256 − mask_octet`
2. Find network address: largest multiple of block ≤ host octet
3. Check gateway is **inside** the host's subnet
4. Add `0.0.0.0/0` default route on hosts pointing at gateway
5. Add specific routes on routers for far subnets
6. Router-to-router links → use `/30`
