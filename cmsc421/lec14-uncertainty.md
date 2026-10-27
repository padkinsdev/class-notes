# Uncertainty

## Probability

### Introduction
- Probabilities hinge on the statement P(Query | Evidence), or "What is the probability of the query/event given the evidence?"
- Expectimax search happens to rely on probabilities, i.e. the probability of a given node being taken in a graph (in the case of a game, the probability of a player picking a given node)

### Terminology
- Probability is weight on possible worlds
    - $`\Omega`$ is the sample space
    - $`\omega`$ is a sample point/possible world
    - The probability model assigns a weight to each possible world (the weights sum to 1)
    - An event is a set of possible worlds
    - P(event) is the total weight of worlds in that event
- A random variable is a question, e.g. "what is the probability that the roll of a die is even?"
- Notation:
    - Capitalized terms are variables
    - Lowercase letters are values
- Values can have different meanings. For example, 0.2 can refer to the frequency of something like a d6 rolling a 6, or it can refer to a belief about the outcome of a single trial (e.g. "I will get 0.2 out of 1 of the questions on this exam correct")
- Prior probability is the probability before observing an outcome
- Posterior probability is the probability after observing an outcome

### Probability domain and joint probability
- The probability domain gives values for all possible assignments (outcomes). These values sum to 1
- A joint probability distribution over a set of random variables specifies the probability of every complete assignment to those variables
    - Every probability query can be answered from the joint distribution because every event is a set of possible worlds, and its probability is the sum of the probabilities of those worlds

### Possible worlds
- Articulating possible worlds entails begininng with random variables and their domains, then constructing possible worlds as assignments of values to all variables
    - For all but the smallest distributions, this is infeasible to enumerate by hand

### Marginal distributions
- Marginal distributions are sub-tables which eliminate variables
- Marginalization (or summing out) collapses a dimension by adding outcomes with the same qualities (e.g. all outcomes where a die roll is even, and separately all outcomes where a die roll is odd, regardless of what else happens alongside the menaingful aspects of the outcomes)
    - $`P(O=librarian) = \Sigma_{t \in \{true, false\}} P(O = librarian, T = t)`$
    - $`P(O=farmer) = \Sigma_{t \in \{true, false\}} P(O = farmer, T = t)`$
    
### Conditional probabilities
- A conditional probability focuses on a subset of the population, i.e. `P(a | b)` or the probability of `a` given `b`
- `P(a)` is the prior probability of a, and `P(a | b)` is the posterior or conditional probability of `a` given `b`
- The probability of `a` given `b` is $`P(a|b) = \frac{P(a,b)}{P(b)}`$, where $`P(a,b)`$ is the joint probability of `a` and `b`
    - This is actually identical to Bayes Rule because $`P(a,b) = P(b|a)*P(a)`$, thus we substitute and get $`P(a|b) = \frac{P(b|a)*P(a)}{P(b)}`$

### Chain rule
- To get the joint probability of multiple outcomes, $`P(x_1, x_2, x_3, x_n) = P(x_1)*P(x_2 | x_1)*P(x_3 | x_1, x_2)*...*P(x_n | x_1, x_2, ..., x_n-1)`$ or in other words, $`P(x_1, x_2, x_3, x_n) = \Pi_i P(x_i | x_1, x_2, ..., x_{i-1})`$

### Inference
- Probabilistic inference involves computing a desired probability from a probability model, typically for a query variable given evidence

#### Inference by enumeration
- Inference by enumeration of possible worlds entails listing out the possible outcomes of a process (such as a game) and using ratios among the possible outcomes to make decisions
    - For example, if two players play a first-to-three game with a pot of x dollars, one player wins twice and the other wins once, if the game is interrupted then splitting the pot between players can be accomplished with the ratio of possible worlds in which one player wins versus the other
- Details:
    - Goal: Compute a probability query
    - Some variables may be hidden (their values are unknown)
    - Enumerate all possible values of the hidden variables
    - Sum the corresponding probabilities
    - Steps:
        1. Select the entries consistent with the evidence
        2. Sum out H to get the joint of query and evidence
        3. Normalize
- Problems:
    - The worst-case time complexity is $`O(d^n)`$ (i.e. exponential in the number of hidden variables)
    - The space complexity to store the joint distribution is also $`O(d^n)`$
    - It takes $`O(d^n)`$ data points to estimate the entries in the joint distribution