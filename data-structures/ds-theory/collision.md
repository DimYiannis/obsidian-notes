# Collision

Two different keys hash to the same bucket — unavoidable (more possible keys than buckets), so every [[hash-table]] needs a resolution strategy:

- **Chaining** — each bucket holds a small [[linked-list]] of entries; walk it on lookup
- **Open addressing** — on a full bucket, probe the next slots in the [[array]] until an empty one is found (linear probing)

Many collisions = long chains = lookups sliding from `O(1)` toward `O(n)`. Kept rare by a good [[hash-function]] and resizing on high load factor.

Up: [[ds-theory]]
