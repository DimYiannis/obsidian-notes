# DFS — depth-first search

Explore a [[graph]] by going as deep as possible before backtracking. Driven by a [[stack]] — either the call stack via [[recursion]], or an explicit one. Same visited-set trick as [[bfs]] to survive cycles.

```python
def dfs(graph, node, visited=None):
    visited = visited or set()
    visited.add(node)
    for nb in graph[node]:
        if nb not in visited:
            dfs(graph, nb, visited)
```

`O(V + E)` ([[big-o]]). Does **not** find shortest paths — it finds *a* path. Its strengths: cycle detection, topological sort (order tasks by dependency), connected components, maze solving, backtracking search. The pre/in/post orders of [[tree-traversal]] are DFS on a tree.

Rule of thumb: shortest/nearest → [[bfs]]; exhaustive exploration, ordering, or "does a path exist" → DFS.

Up: [[ds-theory]]
