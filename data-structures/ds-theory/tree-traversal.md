# Tree traversal

Visiting every node of a [[binary-tree]] exactly once. Three depth-first orders — named for *when the node itself is visited* — plus one breadth-first:

- **Pre-order** (node, left, right) — copy a tree, serialize it
- **In-order** (left, node, right) — sorted output from a [[binary-search-tree]]
- **Post-order** (left, right, node) — delete a tree, evaluate expression trees (children before parent)
- **Level-order** — top to bottom, left to right; this is [[bfs]] on a tree, driven by a [[queue]]

The depth-first three are naturally written with [[recursion]]; all are `O(n)` — every node visited once.

Up: [[ds-theory]]
