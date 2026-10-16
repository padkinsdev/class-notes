# Burrows-Wheeler Transform and the FM Index

## Recap
- The Burrows-Wheeler transform (BWT) matrix is created by finding every cyclic rotation (starting at a given point in the suffix and wrapping back around, e.g. $abcd, d$abc, cd$ab, etc.) of the suffix, putting the permutations together, and sorting them
- The first column of the BWT array is called F and the last column is called L
- Finding each cyclic rotation and sorting them is inefficient, so it is more efficient to construct the BWT matrix from the suffix array by walking through the suffix array (SA) character by character
    - The BWT is smaller than the SA because it is just each cyclic rotation permutation
    - Anything that builds a suffix array also inherently builds a BWT array

### BWT

### BWT matrix construction
- Give every character in T (the BWT array) a rank, being how many times that character has already appeared. This is called the T-ranking
    - This is conceptual. The ranks are not actually stored
    - Rows beginning with a are sorted by what comes after a. ROws ending with a are rotations of those same rows by one position, so they are sorted by what comes after that a too
    - However one chooses to number occurrences of a character, the numbers come out in the same order down F as down L

#### LF mapping
- The ith occurrence of a character c in L and the ith occurrence of c in F are the same occurrence in T
- If row i ends in c, then LF(i) is the row that begins with the same occurrence of c
- Stepping from row i to LF(i) moves one character

#### Improving the numbering
- The T-ranking is arbitrary. Ranking each character instead by its order of occurrence in L (called the B-ranking) means that F's ranks come out in ascending order because F is sorted
- Is the entire matrix needed to invert the string? No
- F costs one integer per alphabet symbol. F is a $, then a block of a's with ascending ranks, then a block of b's with ascending ranks
- It takes constant time to look up the occurrence of a given symbol with rank

#### Undoing the transform
- To invert the transform, start in row 0. Its F is guaranteed to be \$, so the character just before the end of T is L[0]
- To reconstruct the string, use the L char + rank of the current row (starting at the first row, where the F entry will be $) as the index in F to find the next row corresponding to the string reconstruction
    - Basically F[0] corresponds to L[0]. L[0] is the F index for the next row, which corresponds to an L value (char plus rank) that is then used as the F value of the next row, so on and so forth until the L value of the current row is a dollar sign
- Pseudocode:
```
reverse(L):
    C <- first column offsets of L
    row <- 0; T <- "$"
    while L[row] != $:
        c <- L[row]
        T <- c + T  # prepend
        row <- C[c] + Occ(c, row)   # the LF step
    return T
```

## FM index
- Binary search on a suffix array works, but not on a BWT matrix. The FM index stores L and $`|\Sigma|`$ integers from which F can be reconstructed
- The idea:
    - Keep a range of rows whose rotations begin with the part of P matched so far. Because the matrix is sorted, those rows are always contiguous
    - Start with the shortest suffix of P, its last character, and extend one character to the left at a time
    - TO extend by c, look at L inside the current range. Those are precisely the characters that precede the current matches. Keep the c's and apply LF to find where they land in F
    - Stop when P is exhausted (the range holds every match) or when the range becomes empty (P does not occur)
- To find which instances of char a are prceded by char b, look for rows of the matrix with L=b and F=a. This works because every F,L pair of char plus rank shows what character (F entry) follows a character (L entry). The ranks show *which* instance of the given character the F and L entries correspond to
    - In essence, this entails searching backwards from L to F