# Heap

A **complete** [[binary-tree]] with the heap invariant: parent ≤ children (min-heap) or parent ≥ children (max-heap). Root is always the min/max — `peek` is `O(1)`.

Because it's complete, no [[node-pointer|pointers]] needed — it lives flat in an [[array]]: children of index *i* sit at `2i+1` and `2i+2`. `push` and `pop` bubble up/down one root-to-leaf path — `O(log n)` ([[big-o]]). Building a heap from n items (`heapify`) is `O(n)`.

The standard implementation of a [[priority-queue]]. Note: heap order is *weaker* than [[binary-search-tree]] order — siblings unordered, so no `O(log n)` search, only fast min/max.

Up: [[ds-theory]]
