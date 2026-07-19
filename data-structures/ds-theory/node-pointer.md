# Node & pointer

A **node** is a small record holding a value plus one or more **pointers** (references) to other nodes. Chaining nodes through pointers builds every non-contiguous structure: [[linked-list]] (one `next` pointer), [[binary-tree]] (`left`/`right`), [[graph]] (a list of neighbours).

Pointer-based structures trade [[array]]'s `O(1)` indexing for cheap `O(1)` insertion/removal — you only rewire pointers, never shift memory.

Up: [[ds-theory]]
