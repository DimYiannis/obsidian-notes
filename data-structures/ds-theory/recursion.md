# Recursion

A function that calls itself on a smaller piece of the problem, with a **base case** that stops the descent. Each call sits on the call [[stack]] until its children return — which is why deep recursion can overflow, and why any recursion can be rewritten iteratively with an explicit stack.

Trees and graphs are recursively defined (a tree is a root plus subtrees), so [[tree-traversal]] and [[dfs]] are naturally recursive.

Up: [[ds-theory]]
