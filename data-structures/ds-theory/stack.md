# Stack

**LIFO** — last in, first out, like a stack of plates. Two operations, both `O(1)`: `push` (add on top) and `pop` (remove top). Peek reads the top without removing.

Implemented with an [[array]] or a [[linked-list]]. The call stack that runs your programs is one — which is why [[recursion]] and stacks are interchangeable, and why iterative [[dfs]] uses an explicit stack.

Everyday uses: undo history, matching brackets, expression evaluation, backtracking.

Up: [[ds-theory]]
