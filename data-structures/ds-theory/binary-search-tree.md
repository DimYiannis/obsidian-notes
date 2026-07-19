# Binary search tree

A [[binary-tree]] with an ordering invariant: everything in the left subtree < node < everything in the right subtree. Search, insert, and delete all walk one root-to-leaf path — `O(log n)` when balanced, `O(n)` when degenerate ([[big-o]]).

In-order [[tree-traversal]] of a BST visits keys in sorted order — the property that makes it useful.

vs [[hash-table]]: slower lookups (`O(log n)` vs `O(1)`) but keeps keys **sorted** — range queries, min/max, "next largest" all cheap. Production variants self-balance: red-black trees (behind `std::map`, Java `TreeMap`), AVL, B-trees (databases).

Up: [[ds-theory]]
