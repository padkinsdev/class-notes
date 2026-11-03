# Space Efficient Dynamic Programming

## Pairwise alignment, continued

### Forward and backward
- The forward dynamic programming approach aligns prefixes, whereas the backward approach aligns suffixes
    - At some point the two will meet in the middle column. The task thus becomes finding where the optimal path occurs in the middle
    - Effectively, combining the forward and backward approaches allows one to partition the problem and focus on the middle column
- Remember, every path crosses the middle column, the question is just which cell(s) in the middle column represent(s) the optimal edit path
- $`OPT(m, n) = max_q[F(mid, q) + B(mid, q)]`$
- The dynamic aspect of this approach is that the forward and backward boxes can be recursively broken down into smaller sub-problems, e.g. forward and backward within the forward half of the graph

#### Hirschberg's algorithm
- Passes index ranges rather than copies of substrings
- Calls itself recursively
- Every best q lies in some optimal path, so the smallest is taken

##### Time/space complexity
- Score passes (O(n)): F's columns plus B's two working columns, 3(n + 1) numbers, freed before recursing
- Stack: Only one chain is alive at once, depth is approximately log bas 2 of m, a few indices per frame
- Path: At most m + n + 1 cells
- Each level does half the work since the problem is getting split into two halves at each iteration

### Pairwise alignment in theory
- The worst case for alignment is generally quadratic, but that's if both strings are completely different
- O(mn) is the worst case time/space alignment
    - Wavefront alignment (WFA) only visits states with a penalty less than a given score
    - BiWFA is the same as WFA but runs from both ends until they meet (same idea as Hirschberg) in O(s) space
    - A\*PA performs A\* search with a seed heuristic. On long, similar sequences it further cuts down the number of visited states

### Recap
- Score in linear space: Keep two columns, and return a whole column instead of a single number
- Forward and backward: F (prefixes) and B (suffixes) on the same cells; F(i, j) + B(i, j) = the best path through (i, j)
- Divide: the best path crosses the middle column at the q that maximises F(mid, q) + B(mid, q)
- Conquer: recurse on two rectangles of total area ≈ mn / 2: O(mn) time (≈ 2×), O(m + n) space

## Phylogenetics
- Phylogenetics algorithmically deals with an input of multiple sequences (the taxa). 
    - Each input column contains one character. 
    - The output is a phylogenetic tree. Leaves are the taxa, internal nodes are ancestors that no longer exist

### Trees
- The three domains of life are bacteria, archaea, and eukaryotes
    - Originally these were inferred from ribosomal RNA, later this was confirmed with whole genomes
- Broadly speaking, species differentiate by splitting from a common ancestor, a fact which visually lends itself to a tree structure
- Trees are rooted if they display a common ancestor (monophyletic), and unrooted if the common ancestor is not displayed, only the species side by side
- Newick format writes trees as nested parentheses, e.g. (((Human, Neanderthal), (Bonobo, Chimp)), Gorilla)
- Alignment asserts homology (genetic relatedness) and refutes analogy (convergent evolution)
    - Appearance is not a reliable indicator of ancestral/genetic relatedness, precisely due to the existence of convergent evolution

### Alignment
- Rows are taxa, each column is a character, and the residues in it are its states
- Assume that sites evolve independently. Solve one column at a time, then add or multiply over column
- Gap characters are usually treated as missing data
<!--```
Hirschberg(x, y):
    m <- |x|, n <- |y|
    if m <= 1 or n <= 1:
        return plain DP + traceback
    mid <- floor(m/2)
    F <- ScoreOnly(x[1..mid], y)
```-->