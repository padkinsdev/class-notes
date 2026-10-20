# FM Index and Sequence Similarity

## Making backward search fast

### Problem 1: precompute Occ
- Store, at every row i and every symbol c, the value of Occ(c, i) itself
- Occ(c, k) becomes an O(1) lookup

#### Checkpoints
- Pick a period b and keep the tally only at every b-th row. Call those rows checkpoints
- A lookup at a checkpoint is still one read. A lookup elsewhere requires that one walks along L to the nearest checkpoint, counting c's as they pass, and adjust
- With checkpoints every b rows the walk is at most b-1 steps, and the table shrinks by a factor of b
- Another way to phrase this:
    - Keep one bit vector per alphabet symbol: a 1 wherever that symbol occurs in L, a 0 elsewhere. Then Occ(c, k) is exactly rank(V_c, k), or the number of 1s among the first k entries. The checkpoint table counts the same way, so there is no convention and no off-by-one

### Problem 3: a range of rows is not the answer
- Backward search ends holding two range indices. Finding 
- Sample the suffix array:
    - Which rows survive is decided by the text offset, not by the row number, so the kept rows are scattered through the matrix
    - Sampling every a-th text position costs m/a integers, which significantly saves space

### Storing the sample without storing gaps
- A dictionary from row offset would waste the very space we saved. Instead, track one bit per row saying "kept" and a dense array of the kept value in row order

### Building the FM index
- The naive route is to write every rotation and sort them. FOr a large genome this is not only slow, it also doesn't fit on a disk
- Alternatively one may build via the creation of a suffix array. This is linear, but still large
- Direct FM index creation algorithms avoid building the suffix array for efficiency. Blockwise suffix sorting sorts the suffixes in chunks, trading a constant factor of time for a working set the user chooses

## Sequence similarity and global alignment

### From exact matching to similarity
- Exact matching assumes that the pattern is actually in the text, but read errors in sequences make this unrealistically optimistic. So, then, how do you quantify the distance (dissimilarity) between two strings?
- One good application of this is the creation of phylogenetic trees and determining the relatedness of two different organisms
    - The sequence tells a story! If two sequences are similar, they probably stem from a common ancestor, and some sequences are conserved at different rates

### Simple string comparison problem
- Given two strings over an alphabet $`\Sigma`$, $`a=a_1, a_2, a_3, ..., a_m`$ and $`b=b_1, b_2, b_3, ...b_m`$. Compute how similar they are
- The edit distance between a and b is the smallest number of the following operations that turns a into b:
    - Substitute (mutate) one character into another
    - Delete a character
    - Insert a character