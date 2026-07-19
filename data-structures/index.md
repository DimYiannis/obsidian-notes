# Data Structures — Map of Content

> Core insight: every structure is a **trade-off** — you buy fast operations you need
> by paying with slow ones you don't. Big-O is the price tag.

---

## Notes

- [[cheat-sheet]] — complexity table + which-structure-when + interview one-liners
- [[ds-theory]] — hub of the vocabulary note-web (open Graph View to explore)

### Building blocks
- [[big-o]] · [[array]] · [[node-pointer]] · [[recursion]]

### Linear
- [[linked-list]] — chain of nodes, O(1) insert, O(n) access
- [[stack]] — LIFO, push/pop, powers recursion & DFS
- [[queue]] — FIFO, enqueue/dequeue, powers BFS

### Hashing
- [[hash-table]] — key→value in O(1) average
- [[hash-function]] · [[collision]]

### Trees
- [[tree]] — hierarchy vocabulary: root, leaf, height
- [[binary-tree]] — ≤2 children; balance decides log n vs n
- [[binary-search-tree]] — ordered; sorted + O(log n) search
- [[tree-traversal]] — pre/in/post-order + level-order

### Graphs
- [[graph]] — vertices + edges; directed, weighted
- [[adjacency-list]] — how graphs are stored
- [[bfs]] — queue, rings outward, shortest path
- [[dfs]] — stack, deep first, cycles & topo sort

---

## The Checklist

1. Need lookup by key? → hash table. Need it sorted too? → BST.
2. LIFO or FIFO? → stack or queue.
3. Traversing anything: pick BFS for *nearest*, DFS for *everywhere*.
4. Always track visited nodes in graphs — cycles are the default, not the exception.
5. Quote complexity in Big-O, average *and* worst case.
