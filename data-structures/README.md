# Data Structures — why bother

A note-web of the ~25 data structures and algorithms every programmer actually uses. Entry points: `index.md` (map of content), `cheat-sheet.md` (complexity tables), `ds-theory.canvas` (visual essence), `ds-theory/ds-theory.md` (graph hub).

## What you get by learning these patterns

1. **You stop guessing about performance.** Without these patterns, "the app is slow" is a mystery you debug by trial and error. With them, you can predict cost before writing a line: a lookup inside a loop over n items is O(n²) if it's a list scan, O(n) if it's a hash table. That one substitution — list to dict — is the single most common 100×–1000× speedup in real codebases, and you only see it if you know what you're looking at.

2. **Libraries stop being magic.** Every language hands you `dict`, `set`, `sort()`, `heapq`. Learning the structures tells you what those actually cost and when they betray you — why dict keys must be immutable, why sorting first makes everything after cheaper, why deleting from the middle of a list is a trap. You already use these structures every day; learning the theory means using them *on purpose*.

3. **New problems reduce to solved ones.** This is the biggest one. "Find shortest route" → BFS or Dijkstra. "Order builds by dependency" → topological sort. "Undo feature" → stack. "Autocomplete" → trie. Most problems you'll ever face are one of ~20 patterns wearing a costume. Recognition replaces invention — you go from "how do I even start?" to "this is graph traversal, I know this."

4. **You can read real infrastructure.** Databases are B-trees, git is a DAG, the filesystem is a tree, network routers do longest-prefix match on tries, every cache and index is a hash table, every scheduler is a priority queue. The whole stack becomes legible instead of opaque.

5. **Interviews, bluntly.** ~80% of technical interviews are exactly these structures plus their traversals. This cluster is the syllabus.

The compressed version of all five: these patterns are the **shared vocabulary of computing**. Value isn't knowing trivia — it's that thinking in them makes both your code and everyone else's code predictable.

