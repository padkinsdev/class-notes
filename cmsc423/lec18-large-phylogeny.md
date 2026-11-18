# Large Phylogeny Problem

## Introduction

- Brute forcing is infeasible given the sheer number of possible trees
<!-- These lines are copied -->
- *distances -> a fast algorithm (NJ); optimise a score by local search; sample trees (Bayesian MCMC)*
- *p-distance: the fraction of aligned sites that differ. It under-counts: multiple hits at a site are invisible, and the expected p levels off at 3/4*
- *Jukes–Cantor correction: invert the JC curve*
- *d = −3/4 · ln(1 − 4p/3)*
- *d estimates the total branch length between the two sequences; it is undefined for p ≥ 3/4*
<!-- End copied lines -->

## Distance methods

### UPGMA

- Assumes a molecular clock with every leaf equally far from the root (an ultametric M)
- If rates differ between lineages, the closest pair need not be neighbors in the tree, and UPGMA can return the wrong topology even with perfect, additive distances
- $`O(n^2)`$, and gives a rooted tree
- UPGMA fails on a perfect tree but neighbor joining doesn't

### Alignment-free distances (k-mers and MinHash)

- Treat a genome as a series of k-mers with no alignment needed
- D estimates the per-base divergence
- MinHash: keep only the s smallest hash values of each set; the overlap of these sketches estimates J. Comparing two genomes costs O(s), not O(genome)
- Mash builds distance matrices for tens of thousands of genomes, then NJ makes the tree

### Neighbor joining

- Q rewards pairs that are close to each other but far from everything else, which corrects for unequal rates
- IF M is additive, NJ returns the true tree. Each step is $`O(n^2)`$, so $`O(n^3)`$ total. This makes NJ fast and thus the default for big distance matrices

## Tree search

### Local search: nearest neighbor interchange

- Pick an internal edge and swap a subtree across it. Each of the n-3 internal edges gives 2 neighbors: 2(n-3) in all
- Hill-climb: Move to the best neighbor while the score improves. Stop at a local optimum
- Bigger moves search wider: SPR (prune a subtree, regraft anywhere) and TBR (bisect, reconnnect). Restart from many starting trees

### Long-branch attraction

- Parsimony becomes more certain of the wrong tree as data grows, whereas ML converges to the correct tree under the right model

### Bayesian inference (tree sampling)

- Metropolis-Hastings, in short
  - MH is very general, but broadly applicable here
  - Note that sometimes a suboptimal tree is accepted
  - The idea is that as you proceed, the frequency that you see a tree T' is proportional to its actual posterior probability

```pseudocode
# Metropolis-Hastings MCMC
start at some tree T (with branch lengths and model parameters)
repeat many times:
  propose T' by a small random change (an NNI, a new branch length)
  accept T' with probability
    min( 1, Pr(data | T') Pr(T') / Pr(data | T) Pr(T) )
  # (times a proposal correction if moves are not symmetric)
```

- The walk is a Markov chain whose stationary distribution is the posterior Pr(T | data)
- After a "burn-in", the fraction of samples with a given clade estimates that clade's posterior probability
- Output: a distribution of trees rather than a single tree
