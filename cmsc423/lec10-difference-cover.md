# DC3

## Implementing Difference Cover 3 (DC3)
- Step 0:
    - The sample: Color each position by i mod 3. DC3 pads T on the right with three sentinels so that every position has a full triple, and takes the groups over 0 … n — so position n = 12, whose suffix is the empty one, is a group-0 position.
- Step 1:
    - Sort the sample suffixes: Cut the sample positions into triples, in the order B₁ then B₂. Sorting the suffixes of this sequence of triples is the same as sorting the sample suffixes.
    - The recursive call runs the same algorithm on R', and every symbol of R' already stands for three characters of T, so a triple of them stands for nine
    - The recursion returns the sample suffixes in sorted order. Number them 1, 2, 3, ..., n. This is $`rank(S_i)`$, defined only for sample positions
- Step 2:
    - Sort the non-sample suffixes: A group-0 suffix is one character followed by a group-1 suffix, and we already know the rank of that. Thus, represent $`S_i`$ by the pair $`(T[i], rank(S_{i+1}))`$ and radix sort the pairs
- Step 3:
    - Merge: Two sorted lists, merged the ordinary way. Compare suffixes one from each with ranks, in constant time
        - Rule 1: If $`i \in B_1`$ then $`i+1 \in B_2`$ and $`j+1 \in B_1`$. Both sample positions, so both ranks exist
        - Rule 2: If $`i \in B_2`$ then $`i+1 \in B_0`$, whose rank is not known. Tanke one more character: $`i+2 \in B_1`$ and $`j+2 \in B_2`$, and both of those ranks exist
    - Comparing $`S_i`$ (sample) with $`S_j`$ (group 0):
        - $`i \in B_1`$: $`(T[i], rank(S_{i+1}))`$ vs. $`(T[j], rank(S_{j+1}))`$
        - $`i \in B_2`$: $`(T[i], T[i+1], rank(S_{i+2}))`$ vs. $`(T[j], T[j+1], rank(S_{j+2}))`$
- Result: Merging the two sorted lists gives the suffix array of T
- Why is the DC3 algorithm linear?
    - $`T(m) = O(m)+T(2m/3)`$
    - Expand the O: $`T(m) \le cm + T(2m/3)`$ for some constant c
    - Guess $`T(m) \le 3cm`$ and assume it holds for everything smaller than m
    - Then $`T(m) \le cm + 3c(2m/3) = cm + 2cm = 3cm`$

## Suffix arrays in practice

### k-mer lookup table
- A k-mer lookup table adds a practical speedup
- If you'll never search for a pattern shorter than k, then you can precompute, for every k-mer, the suffix array interval of suffixes starting with it
- Any query of length greater than or equal to k begins with some k-mer, so look that up first and binary search only inside the interval it names
- With a k-mer lookup table, query cost becomes $`O(n + \log(j-i))`$ instead of $`O(n+\log(m))`$, and j-i is typically tiny

### Other ideas to improve efficiency
- A k-ary B tree layout of the array: One cache line holds k comparison keys instead of one, so a search touches $`log_k m`$ cache lines instead of $`log m`$
- A lookup table keyed by fixed-length Huffman codes: Variable-length prefixes hash to fixed-width keys, so the table stays dense on skewed alphabets
- Galloping/doubling instead of bisection: finds a nearby boundary in $`O(\log d)`$ for a distance d, which beats $`\log m`$ when consecutive queries are similar
- Adding auxiliary tables allows a suffix array to do what a suffix tree does
- Compressed suffix arrays store the same order in space closer to the entropy of T than to 4m bytes, at the cost of slower access to individual entries

## Doing better with the FM index
- The suffix array threw away the tree's pointers and kept its order. The FM index goes further by storing a permutation of T itself (one character per text character) plus a handful of sparse side tables and searches backward inside it without storing any suffixes

### Burrows-Wheeler transform
- A reversible permutation of the characters of T
- Write down every rotation of T, sort the rotations (represented as a matrix, called the Burrows-Wheeler matrix) alphabetically, and read off the last column
- The first column of the Burrows-Wheeler matrix is written F and the last column L. The transform is just L
- What's the point?
    - Sorting the rotations sorts each character by what follows it, that is, by its right-context. Characters that appear in similar contexts end up adjacent in L, so L has long runs even when T does not
    - The fact that L has long runs allows for efficient run-length encoding, i.e. encoding each run with how long that run is
- ROtations and suffixes sort in teh same order. Everything before the dollar sign in a rotation is its suffix. Everything after it is the wrap-around, and no comparison ever reaches it because the collar sign is unique and smallest