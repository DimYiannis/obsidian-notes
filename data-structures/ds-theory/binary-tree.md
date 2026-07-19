# Binary tree

A [[tree]] where each node has at most two children: `left` and `right`. Shapes matter: **complete** (filled level by level — heaps use this) · **balanced** (left and right heights differ by ≤1 everywhere) · **degenerate** (every node one child — effectively a [[linked-list]]).

Balance is the whole game: a balanced tree of *n* nodes has height `log n`, so root-to-leaf walks are `O(log n)` ([[big-o]]); a degenerate one has height `n`.

Ordered variant: [[binary-search-tree]]. Visiting orders: [[tree-traversal]].

Up: [[ds-theory]]
