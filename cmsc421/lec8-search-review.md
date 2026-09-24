# Search: Reflections, Connections, Review

## Game search
- A two-player zero-sum discrete finite deterministic game of perfect information is a quintuplet (S, I, Succs, T, V) where:
    - S is a finite set of states
    - I is the initial state
    - Succs is a function which takes a state as input and returns a set of possible next states available to whoever is due to move
    - T is the terminal states, a subset of S where the game is finished
    - V is a mapping of terminal states to real numbers, specifically the amount that player A wins from player B (or loses, if V is negative)
- Conventionally, player A moves first, and players alternate turns

### MINIMAX and AND/OR trees
- - Technique for solving problems that can be decomposed into sub-problems
    - Links between branches indicate relations between problems
        - OR: At least one of the node's successors has to be "solved"
        - AND: All of a node's successors must be "solved"
    - Cost can be attached to arcs or nodes
    - Representation found in game search, logic programming, explanatory reasoning, etc.
- An AND/OR graph is a tree structure that only contains AND and OR nodes

### Alpha-beta pruning
- In essence, don't keep searching if you already know there's a better move, i.e. don't test a path in the tree if you know there's a better direction to go

### Constraint satisfaction problems
- Factoresd states: 
    - A vector of values, list of cities, etc.
    - A state is a set of values that parameterize the world's configuration
- Standard search problem:
    - State is a black box
- Constraint satisfaction problem (CSP):
    - State is defined by variables $`X_i`$ with values form domain $`D_i`$
    - Goal test is a set of constraints specifying allowable combinations of values for subsets of variables
- CSP is an example of a formal representation language
- Allows general purpose algorithms with greater power than standard search algorithms
- Examples include map-coloring, cryptarithms, sudoku, n-queens, etc.
- Notably the German WW2 Enigma machine is also a constraint satisfaction problem!
- Using local search, i.e. gradient descent, hill climbing, etc., is the standard approach to solving/approximating CSP problems

## Connecting search to mathematical optimization

### The optimization landscape
- The search landscape metaphor:
    - The objective function creates a landscape, For a minimization problem, the goal is to find the lowest point on the landscape
    - Non-convex problems have many hills and valleys, making it easy for a search algorithm to get stuck in a small valley even though it's a local, not global, minimum
    - Convex optimization problems are ones where the landscape is perfectly convex, meaning that the lowest point is easy to find
    - As dimensions increase, the search space balloons massively, making search prohibitively difficult
- Linear programming is a type of convex optimization
- Fundamental theorem: The optimal solution is guaranteed to be at one of the corners (vertices) of the polytope, meaning that techniques like the Simplex method can intelligently "walk" from corner to corner

### Multi-objective optimization
- Sometimes the objective will be to optimize conflicting goals
- A priori methods like linear programming set the goals first, then eliminate undesirable deviations from those goals
- A posteriori methods like evoliutionary algorithms forego defining goals up front, opting instead to find the entire Paretto front