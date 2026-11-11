# Reasoning Over Time

## Markov models

- Value of X at a given time is called the state
- Parameters: called transition probabilities or dynamics, specify how the state evolves over time (also, initial state probabilities)
- Time-homogeneous (stationary-process) assumption: transition probabilities are the same at every time step

### Stationary Distributions

- Ergodic chains
  - For most chains, the influence of the initial distribution gets less and less over time
  - The distribution we end up in is independent of the initial distribution
  - We call these chains ergodic (every state reachable from every other, no fixed cycle), and they are guaranteed to converge to a unique stationary distribution
- Stationary distributions
  - The distribution we end up with is called the stationary distribution $`P_\infty`$ of the chain

### Hidden Markov Models

- HMMs have two important independence properties:
  - Markov hidden process: future depends on past via the present
  - Current observation independent of all else given current state

### Filtering/monitoring

- Track the belief state:
  - $`B(X_t) = P(X_t | e_1,...,e_t)`$ - the distribution over the current state given all evidence so far
- Start with a prior $`P(X_1)`$, uniform if we know nothing
- At each step: predict (elapse time), observe, then update $`B(X_t)`$
