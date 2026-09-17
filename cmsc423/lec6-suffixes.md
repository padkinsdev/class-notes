# Suffix Trees and Suffix Tries

## Rabin-Karp finalized
- Again, Rabin-Karp uses a rolling hash (rolling across the source string) to quickly locate candidate string matches between a source string and a query string
- To shift the hash to the next character in the source string, the formula is $`t_{s+1} = (d*(t_s-h*T[s]) + T[s+m]) mod (q)`$, meaning that you subtract the leading character (scaled by $`h=d_{m-1}`$), shift everything left by multiplying by d, then add the icoming character. This requires constant work per shift
- Working in modulo q means that each hash doesn't exceed q and stays in a word. This also means that hash collisions can occur, so explicit checks on candidate matches are necessary
    - If the entire pattern fits in a machine word then the modulo q can be dropped entirely
    - Keeping fingerprints modulo two or three different primes makes a simultaneous collicion far less likely at a negligible extra cost
- Whereas the Z-algorithm is exact and deterministic, Rabin-Karp is probabilistic but fast in practice

## Introduction

### Tries
- A trie is a rooted tree representing a collection of strings, with one node per distinct common prefix
- It is the smallest tree such that
    - Each edge is labelled with a single character c from the alphabet sigma
    - No node has two outgoing edges labelled with the same c
    - Each key is spelled out along a path from the root
- A trie is a natural representation of a set or map whose keys are strings
- A trie is also known as a sigma-tree since every edge carries one symbol of the alphabet sigma
- A trie as an index over text takes every substring of a fixed length from a text and records where it occurs

#### Costs
- A lookup follows one edge per character of the query, so it takes O(n) steps, independent of how many keys the trie holds
- If the keys have total length N, the trie has O(n) nodes provided each step really is O(1)
- Representing a node's edges can take multiple forms
    - A hash table from character to child makes the cost per step O(1) expected and O(number of children) space per node
    - A sorted list of (character, child) makes the cost per step $`O(log|\Sigma|)`$ and O(number of children) space per node
    - An array of size $`|\Sigma|`$ gives O(1) worst case cost per step and $`O(|\Sigma|)`$ space per node

## Suffix trees
- The idea:
    - Build a trie containing all suffixes of a text T
    - T has m suffixes but writing them out takes m(m+1)/2 characters
    - The trie shares common prefixes so it will be smaller than m(m+1)/2 but the question is how much smaller