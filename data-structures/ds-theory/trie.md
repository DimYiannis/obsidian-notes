# Trie

Prefix [[tree]]: each edge is one **character**, a key is a root-to-node path. All keys sharing a prefix share that path — the structure *is* the compression.

Lookup/insert `O(L)` where L = key length — independent of how many keys stored ([[big-o]]). vs [[hash-table]]: slightly slower per lookup, but gives what hashing can't — **prefix queries**: "all keys starting with `pre`" is just a subtree walk ([[dfs]]).

Uses: autocomplete, spellcheck, IP routing tables (longest-prefix match), word games.

Up: [[ds-theory]]
