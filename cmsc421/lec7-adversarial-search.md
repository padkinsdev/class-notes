# Adversarial Search

## Introduction

### Games vs. regular search problems
- In regular search, you're finding a path to a specific solution or target space
- In games you are finding a strategy that accounts for every move your opponent might make
- Time limits usually limit exhaustive search, so a strategy must be found that is "good enough"

### Zero sum games
- Properties of zero sum games:
    - Agents have opposite utilities
    - Represented approximately by a single value that one agent tries to maximize and the other tries to minimize
    - Adversarial/competitive
- Different from general games:
    - Agents have independent utilities
    - Cooperation, indifference, etc. are also options
- Checkers, chess, Go, etc. are all zero sum games because they are strictly adversarial with two players and a single winner

## Solving a game
- Solving a game entails determining the optimal strategy for both players, assuming they play perfectly
    - The solution involves exhaustive analysis of all possible game states
    - Also involves proving whether a game will end in a win, draw, or loss for any game state
- The solution for a player takes the form of a policy, or a set of actions to take (maps states S onto actions A)
- The value of a state is the best achievable outcome (utility) from that state

### Minimax search
- A minimax search is represented by a state-space tree where players alternate turns and compute each node's minimax value, that is, the best achievable utility against a rational (optimal) adversary
- Game theoretic value: The value of a terminal that will be reached assuming both players use their optimal strategy
- Minimax trees grow very large very quickly by representing each possible move
    - Rather than storing the entire tree in memory, one may simply iteratively process each level by finding the maximum/minimum (alternating) value successor for each subsequent state (level). This is akin to performing a depth-first search
- Efficiency:
    - Minimax still has exponential time complexity and linear space complexity because it is effectively a depth-first search

### Game tree pruning (alpha-beta pruning)
- General configuration:
    - Compute the MIN-VALUE at some node n
    - Loop over n's children
    - n's estimate of the children's min drops
    - Repeat the process with MAX
- When MAX's best option so far is greater than or equal to MIN's best option so far, further exploration of the current branch cannot change the final decision, so it may be pruned
- Alpha-beta pruning has no effect on minimax value coputed for the root, but values of intermediate nodes might be wrong
- Good child ordering improves effectiveness of pruning
- WIth perfect ordering time complexity drops to $`O(b^{m/2})`$ which doubles solvable depth. However, full search of complex games like chess and Go remains infeasible
- AB pruning is an example of metareasoning, or computing about what to compute

### Resource limits
- Problem: In realistic games one cannot search two leaves. The solution is a *depth-limited search*
    - Search only a limited depth of the tree
    - Replace terminal utilities with an evaluation function for non-terminal positions
    - Guarantee of optimal play is gone, but it's usually still pretty good

### Evaluation functions
- Evaluation functions score non-terminals in depth-limited search
- An ideal function returns the actual minimax value of the position
- In practice a weighted limited sum of features is typically used:
```math
Eval(s) = w_1f_1(s)+w_2f_2(s)+...+w_nf_n(s)
```
- Evaluation functions are always imperfect, and deeper search usually reduces reliance on an imperfect evaluation function, but deeper search is more computationally intensive
    - Thus, there is a tradeoff between speed and accuracy (simple-but-fast vs. complex-but-slow evaluation functions)

## Imperfect players
- Minimax is optimal against a perfect player, but what about an imperfect agent? What if uncertain outcomes are controlled by chance, not an adversary
- In a suboptimal case, values should reflect average case (expectimax) outcomes, not worst-case (minimax) outcomes
    - A random variable represents an event whose outcome is unknown, and a probability distribution is an assignment of weights to outcomes

### Expectimax search
- Choose the action with the highest expected utility
    - Max nodes are the same as in minimax search
    - Chance nodes represent uncertain outcomes
    - Markov Decision Processes (MDPs) can be used to formalize decision-making under uncertain conditions
- Pruning requires knowledge of the probability distribution