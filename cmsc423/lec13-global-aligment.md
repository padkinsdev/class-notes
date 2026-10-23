# Edit Distance and Global Alignment

## Recap
- The simplest string comparison problem is finding the edit distance between two strings (via substitutions, deletions, and insertions). This is also called the Levenshtein Distace
- Edit script/transcript: The sequence of edits one must apply to a string to turn it into another string
- The edit distance is the number of non-match operations one performs before alignment is achieved
- Notably, edges may not cross, i.e. swapping two characters is not a valid operation

### Edit transcripts
- An optimal editor walks left to right and turns x into y, inserting characters into gaps or deleting characters
    - One string is implicitly chose as the query and the other is implicitly chosen as the reference. Which one is which determines whether a deletion or insertion should be performed
- Alignment scoring:
    - Gap: The cost of a column with "-" in it (and insertion or deletion)
    - Cost(x, y) - The cost of a column holding x over y, i.e. the cost of putting character x into alignment with y
        - In proteins the cost is determined through a variety of methods, including with a point mutation model and with empirical substitution likelihoods
    - The cost of an alignment is the sum over its columns. The goal is to find the lowest-cost alignment
- For two strings m and n, there are $`f(m,n) = \Sigma_k 2^k*C(m,k)*C(m,k)`$
    - This means that the number of potential alignments balloons rapidly, from 63 alignments for strings of length 3 to a 76-digit number for strings of length 100
    <!-- One option for prioritizing alignments is to use BLAST, which is a seed-and-extend search over a database-->

## Dynamic programming

### Warm-up and background
- Dynamic programming is A geeral and powerful algorithm design technique, and is applicable when a problem has:
    - Optimal substructure: An optimal solution is built from optimal solutios to smaller suprobblems
    - Overalapping suproblems: The same suproblem is eeded again and again, so its solution is worth preservig and holding on to
- Basically, write the answer as a recurrence over subproblem, then solve the subproblems in an order where each one's ingredients are already in the table
- One example of good dynamic programming application is computing Fibonacci recursively (pretty self-explanatory and trivial to implement)
    - This is, in reality, the way that people calculate the sequence by hand
- Another example is the calculation of shortest paths in a directed acyclic graph (DAG)
    - Edges have a direction, and no path returns to where it started
    - Thus, the vertices must be put in a topological order, a numbering in which every edge points from a smaller number to a larger one
    - Edge weights are lengths
    - One solution is Kahn's algorithm, which uses a queue and repeatedly takes a vertex nothing points to
        - This algorithm runs in O(|V| + |E|)
        - Each vertex enters the queue once, and each edge is removed from the queue once
        - If, after the algorithm is done, the graph still has edges, then there is a cycle present (which defies the *acyclic* aspect of "directed acyclic graph")
    - The distance from s to x is $`d(S,X)= min_{(Y,X) \in E} \{s(S,Y) + \lambda(Y,X)\}`$
    - **Finding the shortest path in a DAG is a powerful abstraction for dynamic programming as a whole!**

## Alignment dynamic programming
- Recursively computing the optimal alignment (OPT(x,y)) between strings x and y up to a certain point leads to three outcomes at the current point (characters a and b):
    - Character a matches character b (in which case the new OPT is the last OPT plus the cost of a match between a and b)
    - x has a gap (in which case the new OPT is the last OPT plus the cost of a gap in x)
    - y has a gap (in which case the new OPT is the last OPT plus the cost of a gap in y)
- Thus, with OPT(x,y) representing the optimal alignment between x and y, one may recursively step forward across each character, adding the prior optimal alignment cost to the optimal result of comparing the current two characters

### The edit distance algorithm
```
EditDistance(a[1..m], b[1..n]):
    for i = 0..m: OPT[i, 0] = i * gap
    for j = 0..n: OPT[0, j] = j * gap
    for i = 1..m:
        for j = 1..n:
            OPT[i, j] = min(cost(a[i], b[j]) + + OPT[i-1, j-1],gap + OPT[i-1, j], gap + OPT[i, j-1] )
    return OPT[m, n]
```
- Time O(mn): (m+1)(n+1) cells, O(1) work each
- Space O(mn) for the whole table — needed to trace the alignment back
- The cost alone needs only the previous column: O(min(m, n)) space, looping over the longer string outermost. Hirschberg's algorithm recovers the alignment in linear space too

## Tracebacks and graphs

### Reading the alignment back out
1. Start at OPT(m, n). AT each cell, find a neighbor that produced its value, i.e. its value plus the cost of that move equals this cell. Step there and write down the column
2. Stop at (0, 0). The columns came out right to left, so reverse them
3. Either store a pointer per cell while filling, or recompute the three candidates on the way back: O(m + n) steps either way
- **Essentially, follow the result matrix from the result back down to the start, tracking what was done at each step**