# Suffix Tries and Suffix Tries 2

## Suffix tries
- The idea: Build a trie that contains all the suffixes of a text T
    - T has M suffixes but writing them out takes $`m(m+1)/2`$ characters, roughly quadratic. However, the trie shares common prefixes, making it some amount smaller than this
    - Suffix tries are unusable on a genome due to their size!
- Why use \$ as the terminator? \$ is a character tha appears nowhere else in T, and sorts before any other character:
    - No suffix of T\$ is a prefix of another suffix T\$
    - Without it, a short suffix can vanish into a longer one. With \$, every suffix ends at a leaf and leaves correspond one-for-one with suffixes
- Every node of the suffix trie is one distinct substring of T
    - A node's label is the string spelled out on the path from the root down to it
    - Every substring of T is a prefix of some suffix T, so it is spelled by some root-to-node path, and distinct substrings give distinct paths
- If a node has two or more children, its string occurs at least twice, but two or more different characters must follow it in T. If only one character followed it, the node would not branch
    - In a suffix trie, depth is the length of the substring

### Suffix trie size
- Best case: Every suffix is a prefix of every longer one, so the trie is a single clain with one \$ hanging off each node
    - The trie has 1 root and m nodes with an incoming a edge and m+1 nodes with an incoming \$ edge
    - 2m+2 nodes makes the trie size linear in m
- Worst case: $`T=a^nb^n\$`$ - exponent n=3, so m=2n=6 characters before the \$
    - 1 root, n nodes along the "b" chain, n nodes along the "a" chain, and n chains of "b" nodes, one hanging off each "a" node
    - 2n+1 leaves below \$ edges
    - This gives $`n^2+4n+2`$, with m=2n. This is quadratic in m
- The number of nodes is less than or equal to $`1 + m(m+1)/2 = O(m^2)`$. Essentially, the suffix trie is $`O(m^2)`$

### Real world use
- Due to the fact that a suffix trie is $`O(m^2)`$, it becomes unwieldy and unrealistic to use on a full genome
    - In practice, the suffix trie ends up having a size that is roughly the same as the worst-case scenario on a full genome

## Suffix trees
- A $`\Sigma^+`$-tree is a rooted tree whose edges are labelled with non-empty strings, where no node has two outgoing edges whose labels start with the same character
    - A suffix tree is compact if every internal node has at least 2 children
- **words(T)** is every string spelled out by a path from the root, and the path may stop mid-edge
- The suffix tree of s is a compact sigma plus tree with words(T) = {s': s' is a substring of s}
    - Each internal node of a suffix tree must have at least 2 children, or else it is a leaf