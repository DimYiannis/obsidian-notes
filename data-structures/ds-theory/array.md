# Array

A contiguous block of memory holding elements of the same size. Index *i* is just `base_address + i × element_size`, so reads are `O(1)` ([[big-o]]). The cost: inserting or deleting in the middle shifts everything after it — `O(n)` — and growing past capacity means reallocating.

The opposite trade-off of a [[linked-list]]. Backing store for [[hash-table]], [[stack]], and heap implementations.

Up: [[ds-theory]]
