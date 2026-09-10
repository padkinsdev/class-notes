# Exact Pattern Matching (The Z Algorithm)

## Recap
- Performing an assembly can be complicated by tandem repeats because the number of repeats is not necessarily known
    - In other words, if the repeating region is longer than the read length, reassembling the sequence from fragments can be difficult because one doesn't know how many times the sequence repeats

### Practical implementation of SCS
- All vs. all string comparison makes computing the initial edge set slow
    - In practice, this is often solved by requiring minimal overlap thresholds between joined strings, and using a hash table to turn all-pairs comparison into lookup
- Searching/scanning for the highest-scoring edge in the overlap graph requires O(E) work in each iteration
    - In practice, this can be solved with a combination of an efficient priority queue and a [union-find data structure](https://en.wikipedia.org/wiki/Disjoint-set_data_structure) to keep track of merged strings
- Ultimately, while the greedy solution to SCS may seem simple, actually implementing it can be deceptively complex
- There is a need for *correct* and *efficient* algorithms for solving *well-specified* problems
    - Thus, it is important to consider how the problem is *posed*

## Introduction
- Initially, an exact-search component is developed, but later (with many patterns) indexing will facilitate faster search
- A mapper finds short exact matches between a read and the reference. Those seeds identify regions worth aligning in detail
    - Find exact seeds for fast exact matching
    - Filter candidates and discard weak regions
    - Align candidates using an inexact method
- The problem at hand: **Given a very long reference string and a short query string, how does one find each instance of the query string in the reference string?**
    - Naive approach: Direct comparison by trying each alignment, with a worst case of O(nm)
    - Z algorithm: Reuse prefix matches, with a worst case of O(n + m)
    - Output: The starting index of every occurrence of the query string
    - Every occurrence is an alignment. An alignment becomes an occurrence only when all characters match

### Naive approach
- The number of possible alignments for a length m query string to a length n reference string is n-m
- Trying every possible alignment takes worst case time O(n) * O(m) or O(nm)
- The best case is O(n+m) if each alignment but the last fails on the first character mismatch (i.e. the first character of the query string only appears once in the entire reference string)
- In the end, the performance will usually be roughly quadratic time, which is not ideal

### Closing the gap with the Z algorithm
- Ideally the algorithm being used would operate in O(n+m) rather than O(nm) time
- Z[i] is the length of the longest substring starting at i that matches a prefix of S, where S is the reference string
    - Basically, where does S repeat itself and for how long
    - Naively computing the z-array takes time O(n^2)
    - Because S will fully match itself, the first value in the Z-array will always be the length of S
- For $S=P\$T$, every position with Z[i]=|P| marks an occurrence of P in T
- The Z-box at i is $[i, i+Z[i]-1]$. Every character in that interval matches the corresponding prefix character
- At step k, the algorithm keeps the Z-box with the largest right endpoint R among positions already processed
    - If k lies inside [L,R], a mirrored prefix value often determines Z[k] without any character comparisons