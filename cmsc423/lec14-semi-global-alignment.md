# Semi-Global and Local Alignment, and Gap Penalties

## Recap
- There can be multiple optimal alignments, depending on the factors (costs, diffs) involved
    - Calculating the number of optimal alignments can be performed with the same complexity as finding the cost of an optimal alignment
- Remember that the optimal alignment at a given position is just the optimal alignment up to a that point plus the optimal alignment at the given position
    - Tracing back the steps in the optimal alignment involves going backwards from the end result of the alignment

### Edit graph
- The edit graph is a directed acyclic graph (DAG), and the problem of alignment can be framed as finding the shortest path through the graph (i.e. from one end to the other)
    - In this case, the number in each node is its distance from 0, 0 (the start)
    - Each node (except the starting one) has three edges going to it, and three edges going out from it
- Column-by-column (or row-by-row or diagonal) is a topological order of the edit graph. This is the condition the DAG algorithm needs

## Semi-global/local alignment + gap penalties
- Alignment can be formulated as a minimization problem for the edit distance between two strings, or a maximization problem
- Global alignment:
    - Aligning all of x against all of y
    - Used for two versions of the same gene or protein from closely related species
- Semi-global:
    - Gaps at the ends are free, e.g. all of x against part of y or the end of one string against the start of another
    - Used for matching a sequencing read against a stretch of genome it came from (called *fitting*) or two reads whose ends overlap (called *overlap*)
- Local:
    - Some substring of x against a substring of y, i.e. where a substring of x matches best to a substring of y
    - Used for proteins from distant species. The whole chain need not be conserved, but a section (domain) often is

### Semi-global alignment
- Fitting: Align all of a short y to the best-matching window of x. Read mapping, once the neighborhood index is found, is a fitting problem
- Overlap: A suffix of x against a prefix of y. This is how assemblers decide that two reads came from overlapping places

#### Fitting
- The alignment can start at any position, so on the graphical representation each value in the bottom row is 0. This represents adding a gap
    - The key here is that alignment is formulated as a maximization problem rather than a minimization problem as in global alignment
- Key changes:
    - Semi-global alignment is just global alignment but where the path through the graph starts and ends can be different
    - Gap costs outside of the alignment range are moot (represented as 0 in the alignment graph, with every entry in the bottom row being 0)

### Local alignment
- Local alignment of x and y: The best alignment of a substring x' of x with a subsubstring y' of y 
- The empty alignment is allowed, so a score of 0 is acceptable
- The best alignment is the largest OPT(i, j) anywhere in the table
- Since the starting point for either string can be anywhere, the bottom row and leftmost column are both filled with 0
    - The score at a given point should never be negative, since it is always better to stop the alignment than let it go negative (i.e. allowing the alignment to go negative drags down each subsequent cell unnecessarily)
- A gap in a local alignment is fine as long as the resulting alignment pays for that gap

#### Smith-Waterman
- The best alignment is the largest value in the matrix, not the corner
- To recover the alignment, trace back from the max value until a 0 is reached
- As before, the time complexity is O(mn)
- Obviously there can still be multiple optimal alignments, represented as multiple of the same max value in the matrix

#### Edit graph changes
1. Start with the global alignment graph
2. To represent that an alignment can start anywhere, add edges with weight 0 from (0, 0) to every node in the graph
3. To represent that an alignment can end anywhere, add edges with weight 0 from every node in the graph to (m, n)

## Putting it together
- While the underlying matrix construction is very similar between alignment types, the optimal alignment varies depending on the type of alignment
    - The semi-global alignment is different from the global because gaps outside of the alignment range are ignored in semi-local but not in global
    - The local alignment is different from the semi-global because it only keeps the part of the alignment that pays best, and ignores parts of the strings that down produce maxima
- Sometimes gaps come in runs, meaning that an alignment can be significantly improved just by removing a section of a string within the alignment (e.g. AAAGGGGAAA can be aligned well to AAAAAA by just removing GGGG). This merits scoring gaps with a function that isn't just a constant cost per position