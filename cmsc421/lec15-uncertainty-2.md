# Uncertainty, Continued

## Independence
- Two variables are independent if $`\forall x, y: P(x,y) = P(x)*P(y)`$
    - Another way to write this is $`\forall x, y: \thickspace P(x | y) = P(x) \: and \: P(y | x) = P(y)`$
    - Independence is eqpressed as $`x \perp\!\!\!\!\perp y`$
- Independence survives negation. In other words, negating x or y preserves independence

### Conditional independence
- x is conditionally independent from y given z iff $`\forall x,y,z \thickspace (P(x | y, z) = P(x | z))`$
    - An equivalent statement is $`\forall x,y,z \thickspace (P(x, y | z) = P(x | z)*P(y | z))`$
    - In other words, learning y does not change your determination of x if you already know z

## Probabilistic models

### Bayes nets
- Bayes nets are a technique for describing complex joint distributions (models) using simple, local distributions (conditional probabilities)
    - Bayes nets are also called graphical models
    - These describe how variables interact locally, and these local interactions chain together to give global, indirect interactions. This allows for 
- Structure:
    - Nodes are variables (each with a domain)
        - Each node has a conditional distribution (in the form of a conditional probability table or CPT), which is a collection of distributions over X, one for each combination of the parents' values
    - Arcs are interactions, akin to CSP constraints
        - Arcs indicate direct influence between variables, and formally encode conditional independence
    - The overall structure is a directed, acyclic graph
    - In essence, a Bayes net is a topology (graph) plus local conditional probabilities
- Building a Bayes net:
    1. Choose a set of relevant variables
    2. Choose an ordering for those variables
    3. Label the variables $`X_1, X_2,...,X_m`$
    4. For each node:
        1. Add that node to the network
        2. Determine the parents of the node as the smallest subset of previous variables that makes that node conditionally independent of all other predecessors
        3. Define the conditional probability table
- Bayes nets implicitly encode joint distributions as a product of local conditional distributions
- To see what probability a BN gives to a full assignment, multiply all the relevant conditionals together: $`P(x_1, x_2, ..., x_n)= \Pi_{i=1}^n P(x_i | parents(x_i))`$

#### Compactness of representation
- A CPT for Boolean $`X_i`$ with k Boolean variables has $`2^k`$ rows for the combinations of parent values
    - Each row requires one number p for $`X_i = true`$ (obviously the number for false is 1 - p)
    - If each varaible has no more than k parents, the complete network requires $`O(n * 2^k)`$ numbers

#### Causality
- Bayes nets need not be causal, but they do often reflect true causal patterns
    - As such, arrows in a Bayes net do not reflect causality, but rather conditional independence. This may in practice mean causality, though
    - If a causal net does not exist, the arrows in the Bayes net reflect correlation