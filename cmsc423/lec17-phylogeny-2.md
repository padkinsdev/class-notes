# Phylogenomics, Continued

## Models

### Jukes-Cantor

- In the Jukes-Cantor model, P(same base after time t) = $`1/4 + 3/4*e^{-4at}`$, and P(a specific other base) = $`1/4-1/4*e^{-4at}`$
- Branch length b = 3at = expected substitutions per site, e.g. $`P_{aa}(b)=1/4+3/4*e^{-4b/3}`$
- A is a rate, not a probability. As b grows, each base becomes equally likely, meaning that the sequence forgets where it started

### Richter models

#### Generalized-time reversible model (GTR)

- Six exchange rates a-f and base frequencies n
- Time reversible: $`\Pi_iQ_{ij} = \Pi_jQ_{ji}`$ - the chain looks tthe same if run backwards
- Probabilities over a branch of length t: $`P(t)=e^{Qt}`$ (matrix exponential)
- Special cases:
  - JC (all rates equal, Pi uniform)
  - K80 (transitions are nt equal to transversions)
  - HKY (K80 + unequal Pi)
- This model is fairly robust if you have a decent amount of data
- Jukes-Cantor is just a special version of this where all of the transition weights are the same
- The assumption of site-independence is maintained

## Likelihood (instead of parsimony)

- Parsimony picks the best history, Likelihood sums over all histories, each weighted by its probability
- Sites remain independent: L(tree) = Pi over columns of L(column). In practice one adds log-likelihoods
- The fundamental question here is which ancestral states are most probable
  - Given a tree with branch lengths, the leaf states, and a model $`P_{ab}(t), \Pi`$, compute the likelihood, i.e. the probability of the leaf states, summed over every possible assignment of the ancestral states
- Determining the probability of a given history via brute force is computationally infeasible due to factorial complexity

### Dynamic programming for likelihood

- Some sums in the computation can be reused, and thus precomputing them drastically reduces complexity
  - Nesting the brackets in the computation actually ends up taking the same form as the Newick format for the tree
  - This is essentially Horner's rule

#### Felsenstein's pruning algorithm

- Algorithm:
  - $`L_v(a)`$ = Pr(the leaves below v | v is in state a)
  - leaf v: $`L_v(a)=1`$ if a is the observed state, 0 otherwise
  - Internal v, children u and w, branch lengths $`t_u, t_w`$: $`L_v(a)=[\Sigma_b P_{ab}(t_u)L_u(b)]*[\Sigma_bP_{ab}(t_w)L_w(b)]`$
  - Whole tree: $`L=\Sigma_a\Pi_aL_{root}(a)`$
- A post-order traversal like Sankoff, with (+, min) replaced by (*, Sigma)
- Per site: $`O(n|\Sigma|^2)`$. For k sites it's $`O(nk|\Sigma|^2)`$. Identical columns only need to be computed once
- Tousands of factors below 1 underflow a double, so store log L or rescale each $`L_v`$ and keep the scale factors

### Benefits of a likelihood model

- Compare trees on the same data: Higher likelihood means a better explanation per the model
- Fit branch lengths: Maximixe L over the t's (numerically, one branch at a time)
- The root doesn't matter for a reversible model (per Felsenstein's "pulley principle"). Rerootiing gives the same probability
- Ancestral states: Which state was each ancestor in? This raises two questions...
- Likelihood considers all histories, weighted by probability, and fits branch lengths. It's powerful, but every answer is conditional on the model
  - Wrong parameters? (rates, bases, frequencies) - estimate them too
  - Rates differ between sites? Add per-site rate categories (Gamma)
  - Sites not independent, or not Markovian? (RNA stems, codons, selection) — richer models, or accept the bias.
  - Model choice is itself a statistical question (likelihood-ratio tests, AIC/BIC).

### Ancestral states (marginal vs. joint probability models)

- Marginal probability: For one node, which state is the most probable, summing over all states of every other node?
- Joint probability: Which single assignnment of states to all internal nodes is most probable?
- At the root, $`Pr(root=a|data)=\Pi_aL_{root}(a)/L`$
- For any other node v, L_v sees only the subtree below v. One also needs the rest of the tree, i.e. a second, top-down pass (or reroot at v, which is fine for a reversible model)
- Notably, summing marginal probabilities for a series of nodes tends to give a wildly different probability than the actual joint probability of those nodes

## The large problem

- The more taxa one adds, the more complex the calculations become
  - To solve this, either use a heuristic (distance or optimize a score), or sample trees (Bayesian, MCMC)
