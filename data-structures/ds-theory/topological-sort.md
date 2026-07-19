# Topological sort

Order the vertices of a **directed acyclic graph** (DAG) so every edge points forward — dependencies before dependents. Only DAGs qualify: a cycle means no valid order, so topo sort doubles as **cycle detection**.

Two ways, both `O(V + E)` ([[big-o]]):
- **[[dfs]] post-order, reversed** — finish a vertex only after all its descendants, then flip the list
- **Kahn's algorithm** — repeatedly take a vertex with in-degree 0 ([[queue]] of "ready" nodes), remove its edges

Uses: build systems (make, cargo), package installs, task scheduling, spreadsheet recalc, course prerequisites.

Up: [[ds-theory]]
