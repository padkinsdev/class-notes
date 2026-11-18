# Decisions

## Utilities

- Rational preferences: Need constraints on preferences before calling them rational, such as the Axiom of Transitivity
- Maximum expected utility principle: Choose the action that maximizes expected utility

### Human utilities

- Money isn't a utility function, but one can talk about the utility of having money or debt
- Human rationality is famously flawed!

## Decision networks

- Evaluating decision networks:
  - Set the evidece variables for the current state
  - For each possile value of the decision node:
    - Set the decision node to that value
    - Calculate posterior probabilities

## Information

- The value of information is the expected increase in utility from gaining that information
  - There's no such thing as perfect information in practice

## Markov Decision Processes

- State transitionn process is Markovia
- Utility fuctio is additive discounted rewards
- An MDP is defined by:
  - Set of states
  - Set of actions
  - Trasition model
  - Reward function
  - Start state
  - Possible terminal state
  - Utility function (which is additive discounted rewards)
  - Probability that a leads to s leads to s', i.e. P(s' | s, a)
- "Markov" generally means that given the present state, the past and future are independent, i.e. actions depend only on the current state
  - Much like search
- Policy: In deterministic single-agent search problems, one needs an optimal plan, or sequence of actions, from start to goal

### Partially Observable Markov Decision Process (POMDPs)

- Value iteration assumes that states are visible. If they aren't, iterate over beliefs
