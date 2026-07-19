# Data Structures — Map of Content

> Core insight: every structure is a **trade-off** — you buy fast operations you need
> by paying with slow ones you don't. Big-O is the price tag.

---

## Notes

- [[cheat-sheet]] — complexity table + which-structure-when + interview one-liners
- [[ds-theory.canvas|DS theory — the essence (canvas)]] — all terms grouped + the thread tying them
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
- [[set]] — keys only; "have I seen this?" in O(1)

### Trees
- [[tree]] — hierarchy vocabulary: root, leaf, height
- [[binary-tree]] — ≤2 children; balance decides log n vs n
- [[binary-search-tree]] — ordered; sorted + O(log n) search
- [[tree-traversal]] — pre/in/post-order + level-order
- [[heap]] — complete binary tree in an array; O(1) min/max
- [[trie]] — prefix tree; autocomplete, longest-prefix match

### Graphs
- [[graph]] — vertices + edges; directed, weighted
- [[adjacency-list]] — how graphs are stored
- [[bfs]] — queue, rings outward, shortest path
- [[dfs]] — stack, deep first, cycles & topo sort
- [[dijkstra]] — weighted shortest path via priority queue
- [[topological-sort]] — dependency ordering of a DAG

### Algorithms
- [[binary-search]] — halve a sorted array, O(log n)
- [[sorting]] — quicksort, mergesort, heapsort; the n log n floor
- [[priority-queue]] — most urgent out first, backed by a heap

---

## The Checklist

1. Need lookup by key? → hash table. Need it sorted too? → BST.
2. LIFO or FIFO? → stack or queue.
3. Traversing anything: pick BFS for *nearest*, DFS for *everywhere*.
4. Always track visited nodes in graphs — cycles are the default, not the exception.
5. Quote complexity in Big-O, average *and* worst case.
