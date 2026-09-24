# Suffix Trees and Suffix Arrays

## Suffix trees
- `node-depth(v)` is the number of edges you follow drom the root to reach v, and `string-depth(v)` is the total length of those edge labels

### Building a suffix tree from a suffix trie
- Idea: Coalesce non-branching paths
    - A node with exactly one child carries no information. There was never a choice to make there, so replace the chain with one edge, labelled by the string it spelled
    - The result is that each internal node is a place where the text genuinely branches, i.e. a substring that is followed by two different characters somewhere in T
    - This does not, however, produce a tree that is O(m)
- Idea 2: Label windows into T
    - Keep T itself and replace every edge label with an (offset, length) pair - a pointer into the text we already have
    - Every edge carries an (offset, length) pair instead of a string
    - Each leaf carries the offset where its suffix begins, so once we reach a node, the answers to “where does S occur?” are sitting in the leaves below it
    - Node labels — the strings themselves — are never stored; they are recovered by walking down from the root.
- Best option: Ukkonen's algorithm
    - Ukkonen's algorithm has the *online* property. That is, if T arrives one character at a time, the tree is updated after each arrival, still in O(m) total
- Idea 1 is $`O(m^2)`$ time and space, idea 2 is $`O(m^2)`$ time and $`O(m)`$ space, and Ukkonen's algorithm is $`O(m)`$ time and space

### Application: longest common extension
- Given S and T, answer many queries (i,j): how far do S-from-i and T-from-j agree
    - Build a generalized suffix tree for S and T - $`O(|S|+|T|)`$
    - Preprocess it so lowest common ancestors are found in O(1), via range-minimum queries - $`O(|S|+|T|)`$
    - Store an array mapping each suffix number to its leaf O(|S|+|T|)
    - Then LCE(i,j) = string-depth of LCA(leaf i, leaf j) - O(1) per query

### Application: all maximal palindromes
- A maximal even palindrome at i: the longest block whose left half is the reverse of its right half
- Reversing S swaps the two halves, so the palindrome centered at i in S is the one centered at |S|-i in S^r, and its radius is how far the two agree
    - Build S^r, the reverse of S - $`O(|S|)`$
    - Preprocess S and S^r for O(1) LCE queries - $`O(|S|)`$
    - For each i, LCE(i, |S|-i) is the radius - O(1) each

### Application: MUMmer
- MUMmer first builds the index over T, then reports maximal matches between T and P
- While very efficient, MUMmer still can't reasonably work on a human genome (3.1 Gb) because that would require almost 50 GB of memory to index

## Suffix arrays

### Basics
- A suffix array is a single linear array
- Keep the query time of a suffix tree, throw the tree itself away, and keep the order the leaves were in
- T is the text that's indexed, P is the pattern being searched for, Sigma is the alphabet, and k is the number of occurrences that are reported
- Suffix array SA(T) is the array of the m starting offsets of T's suffixes, listed in lexicographical order of the suffixes they name