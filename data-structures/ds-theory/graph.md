# Graph

**Vertices** (nodes) connected by **edges** — the most general structure; a [[tree]] is just a connected graph with no cycles. Flavors: **directed** (edges have direction — links, dependencies) vs **undirected** (friendships), **weighted** (edges carry a cost — road distances) vs unweighted.

Vocabulary: **neighbour** (directly connected vertex) · **path** (sequence of edges) · **cycle** (path back to start) · **connected component** (island of mutually reachable vertices).

Stored as an [[adjacency-list]] (usual) or adjacency matrix. Explored with [[bfs]] and [[dfs]].

Up: [[ds-theory]]
