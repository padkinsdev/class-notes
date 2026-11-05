# Uncertainty 2: Bayes Nets

## Recap

- Bayes nets can be represented as directed acyclic graphs (DAG) with one node per random variable
  - Directed edges represent direct probabilistic influence
  - Each node has a conditional probability distribution (CPT)
  - The graph plus each node's CPT defines the full distribution
- The fundamental question to be asked about a Bayes net is "Given some evidence, are two nodes independent?"

## Inference

### D-separation

- Used to study independence properties of simple triples, i.e. how can we determine whether two nodes are conditionally independent given some evidence?
- For larger networks, analyze paths by combining the properties of triples
- **D-separation: A graphical criterion for answering conditional independence queries**

#### Dependence

- Observing an effect with two or more possible explanations can make the explanations dependent (called explaining away)
- Canonical triples:
  - Causal chain (chain): Active unless middle node is observed
  - Common cause (fork): Active unless middle node observed
  - Common effect (collider): Inactive unless descendant observed
- Active path: Evidence can influence beliefs along the path
  - A path is active if every triple on the path is active, and one inactive triple blocks the entire path
- Any complex example can be broken down into repetitions of the three canonical triples
- Local semantics means that each node is conditionally independent of its non-descendants given its parents

#### Markov blanket

- A Markov blanket of a node X consists of:
  - Its parents
  - Its children
  - Its children's other parents (co-parents)
- A Markov blanket is the smallest set of variables that shields X from the rest of the network, or in other words all information necessary to predict X

### Variable separation

- Prediction (cause -> effect)
- Diagnosis (effect -> cause)
- Explaining away (intercausal, if one thing happens, how likely is it that another did too)
- Most probable explanation (what combination of events is most likely to have caused a given result)
- Formulating inference involves:
  - Simple queries: Computing a marginal or conditional probability for a variable
  - Conjunctive queries: Computing joint probabilities for a set of variables
  - Most probable explanation: the best joint assignment to the hidden faults, given the evidence
- Benefits of inference:
  - Decision making
  - Gathering information
  - Understanding what matters (which variables have the biggest influence on the diagnosis?)

#### Inference by enumeration (in Bayes nets)

- Given unlimited time, inference in BNs is easy. Unfortunately, in practice it's quite lengthy and involved (i.e. exponential)
- It's slow because you join up the whole distribution before summing out the hidden values

#### Variable elimination

- Track objects called factors. Initial factors are local CPTs (one per node)
  - Like a database join
  - Get all factors over the joining variable
  - Build a new factor over the union of the variables involved
- Any known values are selected
  - Take a factor and sum out a variable
    - Shrinks a factor to a smaller one
    - This is a projection operation
- Procedure: Join all factors, eliminate all hidden variables, normalize
