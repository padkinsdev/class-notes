# Gene Expression

## Phylogenomics, concluded

- Metropolis-Hastings: Make a small move, ask how probable the new tree is relative to the tree it came from, then accept the new tree with probability proportional to that
- Each gene has its own history. Gene trees can disagree with the species tree via incomplete lineage sorting (ancestral polymorphism), horizontal transfer, or just noise
- Concatenation: Glue all genes together into one alignment and infer one ML tree
- Summary methods: Infer a tree per gene, then find the species tree that agrees with the most gene-tree quartets (ASTRAL)

### Bootstrap

- The support of an edge is the percent of replicate trees that contain it
- Measures stability under resampling, not the probability that the clade is true

### Splitting

- Removig a edge cuts the taxa i twosides, called a split (bipartition)
- A tree is determined by its set of splits. A binary unrooted tree on n taxa has nn-3 non-trivial splits
- Robinson-Foulds distance: The number of splits found in exactly one of the two trees. This is the standard way to compare trees
- Splits that are pairwise compatible always fit together in one tree

#### Majority-rule consensus (consensus tree)

- Collect the splits of every tree (from bootstrap replicates, MCMC samples, or equally good trees) and count them
- Keep the splits in more than half of the trees. Their frequency is the edge's support
- Unresolved conflict becomes a polytony
- The consensus tree is essentially just the components of the sample trees that appear in more than half of all of the samples (e.g. if two taxa appear together in more than half of the samples)
- The majority rule consensus always exists

### Takeaways

- Sometimes the tree is an inaccurate visualization, due to horizontal gene transfer, reassortment, etc.
