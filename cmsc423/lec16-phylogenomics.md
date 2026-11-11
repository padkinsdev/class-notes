# Phylogenomics, Continued

## Recap

- Phylogenomics works one character (position) at a time
- The assumption is that sites (columns) evolve independently
- Types of phylogeny problems:
  - Small: Given a tree topology and the states of each character at the leaves, find states for the internal nodes that minimise the total cost (or maximize the likelihood)
    - Can be solved exactly with dynamic programming
  - Large: Given the states of each character at the leaves, find a tree topology and internal states that optimise the same objective, over all trees
    - Basically, find the best phylogenetic tree
    - This is NP-hard!

## The small phylogeny problem

- The tree is given (from other data or from the large problem)
- Leaves: One gene per species, aligned
- Boxes: The ancestral sequences to be found
- **Parsimony**: Posit as few changes as possible (Occam's Razor)
- Work proceeds one column at a time
  - If you have, say, 4 unknown boxes, choose states for the four boxes to minimise the number of edges whose two ends differ

### Parsimony

#### Fitch's algorithm

- Phase 1: Post order traversal of the tree (leaves up)
  - Fill in each internal node with a set of candidate states that might be selected for that node
- Phase 2: Pre-order traversal of the tree (root down)
  - Select a globally consistent minimum set of transitions from the candidates identified in phase 1
- Cost:
  - Phase 1: The cost is equal to the number of unions
  - Phase 2, per column: $`O(n|\sigma|)`$ time for n leaves, $`O(nk|\sigma|)`$ for k columns

#### Sankoff's algorithm

- Used when transitions have different costs
- This is also a two-phase algorithm with an up phase and a down phase
- $`S_v(a)`$ = minimum cost of v's subtree, given that v is in state a
- $`S_v(a) = min_b [ c(a, b) + S_u(b) ] + min_b [ c(a, b) + S_w(b) ]`$
- The two children's subtrees share nothing but v, so given v's state their costs add
- Phase 2:
  - root: Choose $`r^* = argmin_aS_{root}(a)`$
  - Child v of a node u already set to $`u^*`$: $`v^* = argmin_b[c(u^*,b)+S_v(b)]`$
  - Store the argmin per (edge, state) in pahse 1 to make phase 2 O(n)
  - Several labels can be simultaneously optimal (tie)

### Likelihood

- What is the most likely state of the internal nodes of the tree?
- Parsimony counts changes but ignores branch lengths. A change on a long branch is far more likely tha on a short oe
- Parsimony also igores multiple hits. A -> G -> A on one branch looks like no change
- Likelihood instead produces a probabilistic model of how a site changes along a branch and asks which ancestral states make the data most probable
  - The standard approach is to assume that sites evolve independently along each branch as a continuous-time Markov chain on {A, C, G, T}

#### Jukes-Cantor model

- The proability of transitioning from a given state to any other is $`\alpha`$ (which is a rate, not a probability)
  - As b grows, every base becomes equally likely, ad the sequence forgets where it started
