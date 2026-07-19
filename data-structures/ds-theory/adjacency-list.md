# Adjacency list

Standard [[graph]] representation: a [[hash-table]] (or [[array]]) mapping each vertex → list of its neighbours. Memory `O(V + E)` — pays only for edges that exist, so it wins for sparse graphs (most real ones).

The alternative, an **adjacency matrix** (`V × V` grid of booleans), answers "is there an edge u→v?" in `O(1)` but costs `O(V²)` memory — only worth it for dense graphs.

`graph = {A: [B, C], B: [D], ...}` — this dict is what [[bfs]] and [[dfs]] iterate over.

Up: [[ds-theory]]
