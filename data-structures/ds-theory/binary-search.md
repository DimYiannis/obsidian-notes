# Binary search

Find a value in a **sorted** [[array]] by halving: check the middle, discard the wrong half, repeat. `O(log n)` ([[big-o]]) — a million elements in ~20 steps.

```python
lo, hi = 0, len(a) - 1
while lo <= hi:
    mid = (lo + hi) // 2
    if a[mid] == target: return mid
    if a[mid] < target: lo = mid + 1
    else: hi = mid - 1
```

Same halving idea as walking a [[binary-search-tree]] — a BST *is* binary search frozen into a structure. Requires [[sorting|sorted]] input; notorious for off-by-one bugs (`<` vs `<=`, `mid±1`). Generalizes to "find first true" over any monotonic condition.

Up: [[ds-theory]]
