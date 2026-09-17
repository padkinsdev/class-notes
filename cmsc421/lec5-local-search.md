# Local Search: Hill Climbing, Gradient Descent, Simulated Annealing, and Genetic Algorithms

## Review
- Search space: A graph that connects states (nodes)
- Search tree: The node order imposed by an algorithm traversing this graph
- Failure to detect repeated states can cause exponentially more work
- To avoid expanding a node twice, perform a tree search with a set of expanded states, then expand the search tree node-by-node, but before expanding a node, check to make sure its state has never been expanded before. If the node is not new, skip it
    - Store the closed set as a set data structure ($O(log*(n))$ or $`\theta(\alpha(n))`$), not as a list ($O(n)$ or $O(log(n))$)
- Main idea: estimated heuristic costs should be less than the actual costs
    - Admissibility: heuristic cost <= actual cost from A to G
    - Consistency: heuristic "arc" cost <= actual cost for each arc
- Consequences of consistency:
    - The f value along a path never decreases
    - A* graph search is optimal
- All fringes are priority queue data structures

## Introduction
- Classes of search problems:
    - Combinatorial optimization (vehicle routing, shortest path, knapsack)
    - Constraint satisfaction (map coloring, sudoku, crossword)
- Characteristics of search problems:
    - There is a combinatorial structure being optimized
    - There is a cost function
    - Depth first searches are too expensive
    - Searching all possible structures is intractable
    - More or less similar solutions have similar costs

## Iterative improvement algorithms
- Intuition: Consider the configurations as if laid out on the surface of a landscape
- Examples:
    - Hill climbing and gradient descent: Moves in the direction of the lowest (or highest) value
    - Simulated annealing: Uses random variables to make random moves, iterations get less random as the solution gets closer

### Hill climbing
- Trivial to program
- Requires no memory
- Moveset design is critical
- Evaluation function design is often critical
- If the number of moves is enormous, the algorithm may be inefficient
- If the number of moves is tiny, the algorithm can get stuck easily
- It's often cheaper to evaluate an incremental change of a previously evaluated object than to evaluate from scratch

### Gradient descent
- Rather than climb the hill, follow the gradient down
    - Why? Sometimes it's more intuitive to evaluate for a decrease in utility

#### N-queens
- Put n queens on a chessboard so they are non-attacking
- States: put 8 queer on the board, one per column
- Initial state

#### Boolean satisfiability (SAT)
- Given a formula with phi clauses over n variables check if there exists TRUE/FALSE assignments to the variables that satisfies the formula
- Any SAT can be rewritten into conjunctive normal form (CNF) a conjunction of clauses, where a clause is a disjunction of literals
- SAT is not just logic. It is also a search and optimization framework
- WALKSAT (with hill climbing):
    - Pick a random unsatisfied clause
    - Consider 3 moves, flipping each variable
    - If any improve eval, accept the best
    - If none improve Eval, then 50% of the time, pick the move that is the least bad; 50% of the time, pick a random one

#### Traveling salesman problem
- Decision problem: Given a weighted graph, find the Hamiltonian cycle of least weight
- Optimization problem: Given a weighted graph and an integer k, is there a Hamiltonian cycle with total weight at most k?

### Anthropomorphic algorithms
- Metal cooling/annealing
- Evolution
- Thermodynamics
- and more...

#### Simulated annealing
- Iteratively choose a random move from the moveset. If it produces a better eval, then accept it. Otherwise, accept it with some probability even though it makes eval worse
    - Accepting a worse eval allows for the potential to escape local minima and find better local maxima
- Issues with simulated annealing:
    - Moveset design is critical
    - Evaluating function design is often critical
    - Annealing *schedule* is often critical
- Simulated annealing is sometimes empirically better at avoiding local minima than hill climbing, but without advanced statistical analysis there is not much space to speak formally

## Genetic algorithms
- What if the search space is massive? It is difficult to define a heuristic at all, let alone something *admissible*
    - Perhaps in the absence of a precise evaluation function one may assess relative solution quality (fitness)
    - There can be multiple "good" solutions, and the structural quality of a "good" solution is not immediately evident
- Genetic algorithms operate on the set of candidate solutions. Each iteration of the search is called a *generation*
    - Keep best N hypotheses at each step based on a fitness function
    - Use pairwise crossover operators (with optional mutation) to give variety

### Genetic algorithms for SAT
- In the basic GA objects are encoded as binary strings, with the goal being to optimize some function of the bit strings
- Why is this different from a traditional search?
    - A search problem consists of a state space, successor function, start state, and goal state
    - The solution is a sequence of actions that leads to a goal
    - The cost is a function of the length traveled from start to goal
- GA SAT challenges:
    - Bit string representation is critical
    - Evaluation function design is critical
    - It is often cheaper to evaluate an incremental change than evaluate from scratch

### Hyper-parameters

#### Single point crossover
- Two parents selected at random
- k points selected at random
- Chromosomes are cut at the crossover point
- Tall part of chromosome spliced with head part of the other chromosome
- A k-point crossover is the same thing but with k points selected and swaps occur at the appropriate points

#### Uniform crossover
- Iterate across two chromosomes and swap individual genes with a certain probability

#### Mutation
- Iterate across a chromosome and with a certain probability alter individual genes

#### Elitism
- Copy the best members of each generation into the next generation unchanged

### When to stop a genetic algorithm?
- When an optimal solution is found
- When a time limit is reached
- When a given number of generations is reached
- When the population converges to a single individual
- When the fitness function converges to a single value