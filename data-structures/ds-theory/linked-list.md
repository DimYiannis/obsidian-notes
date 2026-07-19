# Linked list

A chain of [[node-pointer|nodes]], each pointing to the next. **Singly** linked: one `next` pointer. **Doubly** linked: `next` + `prev`, so you can walk backwards and delete a node in `O(1)` given a pointer to it.

Trade-offs vs [[array]] ([[big-o]]):
- Insert/delete at a known position: `O(1)` — rewire pointers, no shifting
- Access element *i*: `O(n)` — must walk from the head, no indexing
- Extra memory per element (the pointers), poor cache locality

Classic backing structure for [[stack]] and [[queue]]. Also how [[hash-table]] chaining resolves a [[collision]].

Up: [[ds-theory]]
