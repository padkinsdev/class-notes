# Reinforcemennt Learning (RL)

## Introduction

- Reinforcement learning essentially centers on freely learning to perform tasks without explicitly coding in strategies
  - Reinforcement is performed through scoring results rather than approaches
- The successor function is an *action* function, being *what action to take*
- In RL:
  - The model is unknown. Agent learns dynamics and rewards from interactions
  - Trial and error with stochastic transitions and rewards
  - Learns policies or value functions from sampled results
  - The goal is to maximize long-term cumulative reward (as opposed to minimizing prediction error or loss)
- Policy: WHat to do
- Reward: What is good
- Value: What is good because it predicts reward
- Model: What follows what
- RL is good when the world isn't deterministic, is continuous, is noisy, etc.
- RL is fundamentally Markovian in that it reacts to current states rather than entirely planning ahead
  - As such, RL is like search where the path depends on the current state
- If the Markov Decision Process (MDP) is known, it may be solved offline. Otherwise, it must be approximated/modeled via interacting with it
- The agent must be given a utility function, with rewards assigned to states, e.g. good outcomes have positive utility, bad ones have negative utility
- A fixed horizon means that there is a fixed time N after which nothing matters. An infinite horizon means that there is no fixed time
- RL methods specify how the agent changes its policy as a result of its experience
- The agent's policy is a product of a balance between risk and reward
- Solving MDPs:
  - Value iteration: Search over state rewards
  - Policy iteration: Search over policies. Start with a random policy and iterate until the best one is found
- Episodic games: Game is over after N steps
- Continuing game: Game has no predefined end

## Model-based learning

### Q function and Bellman equation

- Expected return/reward when starting in s, performing a, and following the policy $`\pi`$
- Useful when finding the optimal policy
- The Bellman equation deals with solving an optimization as a sequence of simpler subproblems, intuitively descriped as "bottom-up divide and conquer"
- Bellman-Ford proceeds by *relaxation*, in which solutions/paths are replaced by better ones until an optimal solution/path is found
- The value of a given policy $`\pi`$ is the expected sum of discounted rewards within that policy
- If there are n possible states, there are n Bellman equations

### Value vs policy search on known transition states

- What matters most in value search is that utilities are good enough to recommend optimal actions for each state
- Value iteration is slow, and the policy doesn't change, so iterating over policies is better
- Often a hybrid approach between policy and value search is the best one

#### Passive RL

- Passive RL entails running the policy for a number of steps to learn from experience
- In this sense, the learner is along for the ride
- After enough runs, the solutions tend to converge to an optimal policy

### Model-based reinforcement learning

- Model-based idea:
  - Learn an approximate model based on experiences
  - Solve for values as if the learned model was correct
- If neither transition probabilities nor reward dynamics are known, experience the world and use those experiences to build a model
- Essentially working in a simulator
<!-- Adaptive Dynamic Programming:-->

### Takeaways

- It's important to ge tthe level of abstraction correct, i.e. what should and shouldn't matter when discerning the model/optimal policy?
- RL is fomralized as MDPs
- The learner is not told which actions to take, but rather figures them out via trial and error
