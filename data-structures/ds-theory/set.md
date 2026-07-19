# Set

A [[hash-table]] with keys only, no values. One question, answered in `O(1)` average: **"have I seen this before?"** ([[big-o]]).

Uses: dedup, membership tests, the **visited set** in [[bfs]]/[[dfs]], set algebra (union, intersection, difference — "users in A but not B").

Same constraints as its parent: elements must be hashable/immutable ([[hash-function]]), no ordering. Need sorted membership? Tree set ([[binary-search-tree]]) — `O(log n)` but ordered.

Up: [[ds-theory]]
