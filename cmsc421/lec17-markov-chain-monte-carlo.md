# Markov Chain Monte Carlo

## Recap

- In D-separation, a path represents a distinct path through the network
  - Leaving a node and then returning to the same node is just a loop, not a new dependency channel
  - D-separation considers simple paths
    - A path cannot visit the same node more than once
<!-- Explaining away:
  - Before observing: $`P(B,E)=P(B)P(E)`$-->

### Variable elimination

- Operation 1: Joining factors
  - Like a database join
  - Get all factors over the joining variable
  - Build a new factor over the union of the variables involved
- Operation 2: Eliminate
  - Take a factor and sum out a variable
    - Shrinks a factor to a smaller one
    - A projection operation
  - If there is evidence, start with factors that select that evidence

### General variable elimination

- Query: $`P(Q|E_1=e_1,...,E_k=e_k)`$
- Start with initial factors (local CPTs instantiated with evidence)
- While there are still hidden variables (not Q or evidence):
  1. Pick a hidden variable h
  2. Join all factors mentioning H
  3. Eliminate (sum out) H
- Join all factors and normalize
- The ordering for variable elimination is not always clear, and finding the best ordering is NP-hard (impacted by the largest factor)

### Polytrees

- A polytree is a DAG with at most one undorected path between any two nodes
- Exact inference can be performed in linear time on a polytree

## Sampling and stochasticity

### Sampling

- Sampling is a lot like repeated simulation
- Basic idea:
  - Draw N samples from a sampling distribution S
  - Compute an approximate posterior probability
  - Show this converges to the true probability P
- Sampling helps for learning (building a model) and inference
- Rejection sampling means only keeping tallies for samples that actually matter (e.g. a specific result that matches the desired evidence)
  - If the desired evidence is unlikely, this rejecys a lot of samples
- Likelihood weighing: Fix evidence variables, sample only nonevidence variables, and weight each sample by the likelihood it accords the evidence
  - Limitations:
    - WHen the evidence is unlikely, most samples will have ear-zero weights
    - The estimate is dominated by a few "lucky" samples with high weights, leading to a massive variance and inaccuracy unless N is huge

### Markov Chain Monte Carlo (MCMC)

#### Gibbs Sampling

- Steps:
  1. Fix evidence
  2. Initialize other varaibles randomly
  3. Repeat: Choose a non-evidence variable X, then resample X from P(X | all other variables)
- Ultimately, only the Markov blanket matters
- Crucially, only one variable can change at each sampling step

#### Markov process

- Markov property: The next state depends only on the current state, not on how the current state was reached
- Markov chain: A sequence of states that follows the Markov property
- Gibbs sampling creates a Markov chain whose states are complete assignments (possible worlds)
  - The Bayes net defines the probability distribution we want to sample from
  - Gibbs sampling uses Markov blanket probabilities to generate transitions between states
