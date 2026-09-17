# Shortest Common Superstring & Lander-Waterman Statistics

## Introduction
- Remember: Shotgun sequencing involves copying and breaking the full genome into fragments to sequence, then reassembling it probabilistically
    - The breaking process is not fully random, and some parts of the genome can be difficult to sequence
- Aside: How many reads do we need to be sure we cover the full genome?
    - An *island* is a contiguous group of reads that are connected by overlaps of length >= omega * L
    - Omega is the fraction of L required to detect an overlap
    - We want an expression for the number of islands given N, g, L, omega

## Islands
- The probability that k reads start in an interval of length x (`Pr(k reads start in an interval of length k)`) is $`e^{-\lambda*x}*\frac{(\lambda*x)^k}{k!}`$
    - The expected number of islands is N times the Probability that a read is at the rightmost end of an island
    - `Theta*L = N * Pr(0 reads start in (1-theta)*L)`
    - Expected # of islands 
    ```math
    = N*e^{-(1-\theta)*L*\frac{N}{g}}
    ```
    ```math
    = N*e^{-(1-\theta)*c}
    ```
    ```math
    = \frac{L/g}{L/g}*N*e^{-(1-\theta)*c}
    ```
    ```math
    = g/L*c*e^{-(1-\theta)*c}
    ```
- A base is uncovered exactly when no read starts in the L positions ending at it
    - Pr(base uncovered) = $e^{-\lambda*L} = e^{-c}$ and therefore
        - Expected uncovered bases = $ge^{-c}$
        - Expected number of gaps = $Ne^{-c}$
        - Average gap length $= g/N = L/c$

## Genome assembly
- Given a collection, R, of sequencing reads (strings), find the shortest genome (string), G, that contains all of them

### Shortest common superstring (SCS)
- Given a collection, $S = {s_1, s_2, s_3}$ of sequencing reads (strings), find the shortest possible genome (G) such that all strings in S are substrings of G
- Without the requirement of "shortest" you could just concatenate the substrings
- Idea: Select an order for the substrings in S and construct a superstring via eliminating overlaps
    - For n strings, $n!$ orderings are possible
- Idea: Treat this like a graph problem
    - Let each substring be a node, and each edge is the length of the overlap between two given nodes
    - The SCS corresponds to a path that visits every node once, minimizing the total cost along the path
    - This is actually the Traveling Salesman Problem, and thus NP-hard

### SCS as a graph problem
- Even just finding a path that visits every node just once is still NP-complete, and is the Hamiltonian path problem
- Even though SCS can be modeled with NP-hard problems, it isn't inherently NP-complete. To establish this one must find an analogous NP- complete problem for it
    - Ultimately, because SCS is analogous to other NP-complete problems, it is also NP-complete

#### Solutions
- Greedy heuristic: At each step, choose the pair of strings with the maximum overlap, merge them, and return the merged string to the collection. Once no more overlaps exist, concatenate the remaining strings in the collection
    - The greedy algorithm will by nature not give a solution that is more than 9/4 times longer than the optimal solution, per a paper published on 09/02/2026. Originally the ratio was believed to be 3.5x
    - If there are multiple pairs with the same overlap, picking the wrong one to concatenate can lead to disaster
    - The SCS can be shorter than the actual original string if the original string has significant repeats. This is called overcollapsing