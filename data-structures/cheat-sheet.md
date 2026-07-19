# DSA Cheat Sheet

## Complexity table (average case)

| Structure | Access | Search | Insert | Delete | Notes |
|---|---|---|---|---|---|
| Array | O(1) | O(n) | O(n) | O(n) | insert/delete shift elements |
| Linked list | O(n) | O(n) | O(1) | O(1) | O(1) given pointer to position |
| Stack | — | — | O(1) | O(1) | push/pop top only |
| Queue | — | — | O(1) | O(1) | enqueue back, dequeue front |
| Hash table / set | — | O(1) | O(1) | O(1) | O(n) worst case (collisions) |
| BST (balanced) | O(log n) | O(log n) | O(log n) | O(log n) | O(n) if degenerate |
| Heap | O(1) top | — | O(log n) | O(log n) pop | heapify all: O(n) |
| Trie | — | O(L) | O(L) | O(L) | L = key length |
| Binary search | — | O(log n) | — | — | needs sorted array |
| Sorting | — | — | — | — | O(n log n) comparison floor |
| BFS / DFS | — | O(V+E) | — | — | graph traversal |
| Dijkstra | — | O((V+E) log V) | — | — | weighted, non-negative |

## Which structure when

- Key → value lookup, no order needed → **hash table**
- Sorted data, range queries, min/max → **BST** (red-black / B-tree in practice)
- Undo, backtracking, matching brackets, recursion → **stack**
- Process in arrival order, buffering → **queue**
- Frequent insert/delete mid-sequence, unknown size → **linked list**
- Fast indexed access, fixed-ish size → **array**
- Always need the min/max next, top-k → **heap / priority queue**
- Membership test, dedup → **set**
- Prefix queries, autocomplete → **trie**

## Which traversal when

- Shortest path (unweighted), nearest-first, levels → **BFS** (queue)
- Cycle detection, topological sort, "does path exist", exhaustive search → **DFS** (stack/recursion)
- Shortest path, **weighted** edges → **Dijkstra** (BFS + priority queue); negative edges → Bellman-Ford
- Dependency ordering of a DAG → **topological sort** (reversed DFS post-order, or Kahn's)
- Both BFS & DFS: O(V+E), both need a visited set

## One-liners for the interview

- Load factor = entries/buckets; resize ~0.75 keeps hash table O(1)
- Balanced tree height = log n — that's where O(log n) comes from
- Degenerate BST = linked list = O(n)
- In-order traversal of BST → sorted output
- BFS first-arrival = shortest path (unweighted only; weighted → Dijkstra)
- Any recursion = iteration + explicit stack
