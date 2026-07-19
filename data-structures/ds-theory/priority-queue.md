# Priority queue

A [[queue]] where the **highest-priority** element comes out first, not the oldest. Interface: `push(item, priority)` · `pop()` → most urgent.

Almost always backed by a [[heap]]: push and pop `O(log n)`, peek `O(1)` ([[big-o]]). A sorted [[array]] or [[linked-list]] would make one of the two operations `O(n)`.

Uses: [[dijkstra]] (always expand the nearest unvisited vertex), OS schedulers, event simulation, top-k problems, merging k sorted lists, [[sorting|heapsort]].

Up: [[ds-theory]]
