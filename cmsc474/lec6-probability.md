# Probability

## Conditionals
- If A and B are events with Pr(A) > 0 the conditional probability of B given A is $$Pr(B|A) = \frac{Pr(AB)}{Pr(A)}$$

### Conditional independence
- Event A and B are conditionally independent given C if Pr(AB|C) = Pr(A|C)Pr(B|C)
- A set of events {$A_i$} is conditionally independent if $Pr(\cap_i A_i|C) = \Pi_i Pr(A_i|C)$

### Bayes' rule
- Given two events A and B and suppose that Pr(A) > 0. Then $$Pr(A|B) = \frac{Pr(AB)}{Pr(B)} = \frac{Pr(B|A)Pr(A)}{Pr(B)}$$
- Why do we care?
    - Often P(B|A), P(A), P(B) are easier to get
    - Prior P(A) is the probability of A before evidence
    - Likelihood P(B|A) is the probability of evidence assuming A
    - Posterior P(A|B) is the conditional probability after knowing the evidence
    - Inference is deriving unknown probability from known ones
- Suppose that $B_1, B_2, ..., B_k$ form a partition of S: $$B_i \cap B_j = \varnothing, \cup_i B_i = S$$
- Suppose that $Pr(B_i) \gt 0$ and $Pr(A) \gt 0$. Then $$Pr(B_i|A) = \frac{Pr(A|B_i)Pr(B_i)}{Pr(A)} =$$ $$\frac{Pr(A|B_i)Pr(B_i)}{\sum_{k}^{j=1} Pr(AB_i)} =$$ $$\frac{Pr(A|B_i)Pr(B_i)}{\sum_{k}^{j=1} Pr(B_j)Pr(A|B_j)}$$

## Random variable and distribution
- A random variable X is a numerical outcome of a random experiment
- The distribution of a random variable is the collection of possicle outcomes along with their probabilities:
    - Discrete case: $Pr(X=x) = p_\theta(x)$
    - Continuous case: $Pr(a \le X \le b) = \int_{a}^{b} p_\theta (x)dx$
- The support (outcomes with probability greater than 0) of a discrete distribution is the set of all x for which $Pr(X=x)>0$
- The joint distribution of two random variables X and Y is the collection of possible outcomes along with the joint probability $Pr(X=x, Y=y)$

## Expectation
- A random variable X~Pr(X=x). Then its expectation is $$E[x] = \sum_x xPr(X=x)$$
    - In an empirical sample $x_1,x_2,...,x_N$ $$E[X] = \frac{1}{N} \sum_{i=1}^{N}x_i$$
- Continuous case: $$E[X]=\int_{-\in}^{\in} xp_\theta(x)dx$$
- In the discrete case expectation is the average of numbers in the support weighted by their probabilities
- Expectation of sum of random variables: $$E[X_1 + X_2] = E[X_1] + E[X_2]$$

## Variance
- The variance of a random variable X is the expectation of $(X-E[X])^2$: $$Var(X) = E[X^2] - E[X]^2$$