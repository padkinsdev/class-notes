# Suffix Arrays Continued

## Review
- Suffix array SA(T) is the array of the m starting offsets of T's suffixes, listed in lexicographical order of the suffixes they name
- The suffix array is the suffix tree's leaves, and the LCP array is where the leaves meet
    - Two adjacent leaves meet at some internal node (walk into a bar, perhaps). The *string-depth* of that node is how much the two suffixes share, that is, their longest common prefix
- Querying a suffix tree is O(n). Querying a suffix array is O(n log m). Querying a suffix array + LCP tables is O(n+log m).
    - While these are the statistics for the naive implementations, the times can be improved

## Searching and sorting
- P occurs in T iff P is a prefix of at least one suffix of T
- The suffixes that have P as a prefix are consecutive in SA(T)
- Sorting groups equal prefixes together: everything beginning with a is one block, and inside it everything beginning with ab is a smaller block

### Binary search
- First, assume P contains no \$, so P is never equal to a suffix. Maintain 3 indices a nd one variant: SA[I] < P < SA[r]
    - I starts at 0. SA[0] is the suffix \$, which is smaller than any P
    - r starts at m, one past the end, standing for "larger tahn everything"
    - c is the midpoint $`\lfloor (I + r)/2 \rfloor`$
- Laying out the suffix array like a B tree allows for slightly faster searches because B trees are suitable for binary search
- The search is O(n log m)

### One iteration
- Let c = $`\lfloor (l+r)/2 \rfloor`$ and compare P against the suffix SA[c], character by character
- If P < SA[c] the answer is in the top half, set r=c
- If P > SA[c] the answer is in the bottom half, set I = c
- Stopping occurs when:
    - P < SA[c] and c = l + 1 -> the answer is c
    - P > SA[c] and c = r - 1 -> the answer is r
- When P does not occur, nothing changes. The search still ends at an insertion point, just an empty one

### Counting and reporting occurrences
- Search for P gives lo, the first row whose suffix has P as a prefix
- Search again for the first row past all of them. This gives hi
- hi-lo is the numbner of occurrences, and SA[lo...hi-1] are the offsets

### Accelerating the search
- Where is the wasted work? Every iteration starts comparing at character 0, ebven though we already know a great deal about how P relates to the suffixes at l and r

## Longest common prefix
- LCP(S, T) is the longest string that is a prefix of both S and T, i.e. everything they share before the first difference
- Suppose we already know how much P shares with teh suffixes at l and at r. Then we get a free lower bound on how much it shares with the one at c:
```math
LCP(P, SA[c]) \ge min(LCP(P, SA[I]), LCP(P, SA[r]))
```
- Why? SA[c] sits between SA[l] and SA[r] in sorted order. If SA[l] and SA[r] both agree with P on their first j characters, every suffix ordered between tehm must agree on those j characters too
    - Thus, the comparison at c can skip the first min(.,.) characters and start there
- This is called the "simple accelerant", per Gusfield

### The simple accelerant
- Carry two numbers through the search: how much P shares with SA[l] and how much with SA[r]. Both start at 0
- At each iteration, begin the character comparison at offset min(lcp_l, lcp_r) instead of 0
- Whichever side you bisect to, the number of characters you just matched becomes the new lcp for that side
- Unfortunately, the worst case remains O(n log m)

### The super accelerant
- The missing ingredient is the LCPs among T's own suffixes, i.e. how much SA[l] and SA[r] share, independently of P. THose can be precomputed once
- u is already in hand since the simple accelerant was carrying it
- D is a table lookup in constant time

#### Cases
- Write D = LCP(SA[l], SA[c]) and u = LCP(P, SA[l])
- There are 3 cases:
    - D > u/P > SA[c], so bisect right
    - D < u/P < SA[c], so bisect left
    - D = u is unknown, so compare characters