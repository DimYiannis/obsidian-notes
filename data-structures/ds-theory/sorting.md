# Sorting

Comparison sorts can't beat **`O(n log n)`** ([[big-o]]) — information-theoretic floor. The three worth knowing:

- **Quicksort** — pick pivot, partition smaller/larger, recurse ([[recursion]]). Average `O(n log n)`, worst `O(n²)` (bad pivots), in-place, fast in practice
- **Mergesort** — split in half, sort halves, merge. Always `O(n log n)`, **stable**, needs `O(n)` extra memory
- **Heapsort** — push all into a [[heap]], pop n times. `O(n log n)`, in-place, not stable

Built-ins (Python/Java: timsort) are mergesort hybrids — stable, `O(n log n)`, exploit existing runs. Non-comparison sorts (counting, radix) hit `O(n)` when keys are small integers.

Why sort at all: enables [[binary-search]], range scans, dedup, merging.

Up: [[ds-theory]]
