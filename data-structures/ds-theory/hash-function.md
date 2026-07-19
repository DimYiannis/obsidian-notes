# Hash function

Turns a key of any size into a fixed-size integer, then `hash % bucket_count` picks the [[array]] slot in a [[hash-table]]. A good one is **fast**, **deterministic** (same key → same hash, always), and **uniform** (spreads keys evenly so [[collision|collisions]] stay rare).

This is why "hashable" keys must be immutable: mutate a key after insert and its hash no longer finds its bucket.

Up: [[ds-theory]]
