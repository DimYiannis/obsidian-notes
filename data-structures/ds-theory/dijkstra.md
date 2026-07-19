# Dijkstra

Shortest path in a **weighted** [[graph]] with non-negative edges. [[bfs]] handles unweighted (every edge costs 1); Dijkstra generalizes: instead of a plain [[queue]], a [[priority-queue]] always expands the **nearest unvisited vertex** by accumulated distance.

Greedy, and correct because weights are non-negative: once a vertex is popped, its distance is final. `O((V + E) log V)` with a [[heap]] ([[big-o]]).

Negative edges break the greedy step → use Bellman-Ford. Real uses: GPS routing, network routing protocols (OSPF), game pathfinding (A* = Dijkstra + heuristic).

Up: [[ds-theory]]
