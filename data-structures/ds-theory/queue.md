# Queue

**FIFO** — first in, first out, like a checkout line. `enqueue` adds at the back, `dequeue` removes from the front, both `O(1)` when built on a [[linked-list]] or circular [[array]].

The mirror image of a [[stack]]: stack explores *deepest first*, queue processes *oldest first* — which is exactly why [[bfs]] uses a queue and [[dfs]] uses a stack.

Everyday uses: task scheduling, message queues, buffering, level-order [[tree-traversal]]. Variants: **deque** (both ends), [[priority-queue]] (highest priority out first, backed by a [[heap]]).

Up: [[ds-theory]]
