# Hash table

Key → value store with average `O(1)` lookup, insert, and delete ([[big-o]]). A [[hash-function]] maps the key to an index in a backing [[array]] of buckets; two keys landing on the same index is a [[collision]].

**Load factor** = entries ÷ buckets. When it grows too high (~0.75), the table resizes: allocate a bigger array, re-hash everything — `O(n)` once, amortized away.

The workhorse behind `dict`/`map`/`set`/`object` in every language. Costs: no ordering, worst case degrades to `O(n)` if collisions pile up, memory overhead for empty buckets.

Up: [[ds-theory]]
