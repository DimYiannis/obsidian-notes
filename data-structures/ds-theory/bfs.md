# BFS — breadth-first search

Explore a [[graph]] in expanding rings: visit the start, then all its neighbours, then *their* neighbours. Driven by a [[queue]] (FIFO) plus a **visited set** ([[hash-table]]) so cycles don't loop forever.

```python
def bfs(graph, start):
    visited = {start}
    q = deque([start])
    while q:
        node = q.popleft()
        for nb in graph[node]:
            if nb not in visited:
                visited.add(nb)
                q.append(nb)
```

`O(V + E)` ([[big-o]]). Key property: first time BFS reaches a node, it took the **shortest path** (fewest edges) — the reason it answers shortest-path questions on unweighted graphs. Also: level-order [[tree-traversal]], "degrees of separation", flood fill.

Contrast: [[dfs]].

Up: [[ds-theory]]
