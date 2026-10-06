# Non-Linear and Partial Order Planning: Graph Plan, SATPlan, and More

## Recap
- Planning is a kind of search, a bit like logical inference
- States and operators are defined using a restricted version of FoL (propositions, predicates, variables)
- Calculating successors requires unification of variables to find applicable operators
- Massive branching factors
- Control search with regression, i.e. only take actions that achieve unachieved goals
- Means-end analysis: Reduce the difference between S and G
- PDDL: Language for defining planning problems

## Planning algorithms
- Planning algorithm options:
    - Forward state-space search: Action schema represents the transition model. Perform regular BFS/DFS search. Often A* with heurostocs is used to deal with the state space size
    - Backward search: Regression search. Keeps the branching factor low
    - Convert the PDDL description to a propositional form, and use an efficient solver for the Boolean Satisfiability Problem
- Traditional search assumes a linear order, i.e. moves follow each other and progression is synchronous

### Nonlinear planning
- Sussman Anolamly: Pursuing one subgoal may prevent the completion of another without undoing previous actions, i.e. pursuing one goal may interfere with another
- Both forward and backward planning methods commit to a total ordering on the selected actions
- No unnecessary ordering among subgoals, i.e. if two subgoals are independent, there's no need to order them relative to each other

#### Total vs. partial order planning
- Idea:
    - Create a directed graph that captures planning constraints
    - Topologically sort the graph
- Space consists of plans, not situations
- A consistent plan is one in which there is no cycle and no conflict between chievers and threats
- A conflict can be eliminated by constraining the ordering among the actions or by adding new actions
Observation: PLans are being searched, not states

## GraphPLAN & SATPlan
- Planning as building a plan graph

### Planning graph (GraphPLAN)
- A planning graph is a special data structure used for
    - Deriving better heuristic estimates
    - Extracting a solution to the planning problem
- At each state level, list all literals that may hold at that level
- At each action level, list all noops and all actions whose preconditions may hold at previous levels
- Repeat until plan "levels off" (no new literal appears)
- Building the planning graph is a polynomial process
- **BlackBox**: An implementation of GraphPLAN that uses an alternate plan extraction technique than the backward chaining of GraphPLAN

#### MUTEX links between actions
- Inconsistent effect: One action negates an effect of another
- Interference: An action negates the precondition of another
- Competing needs: A precondition of an action is mutex with the precondition of another

### SATPlan
- Formulate the planning problem as a Constraint Satisfaction Problem (CSP)
- Fix a horizon of k time steps
- Create a binary varaible for each possible action, Action(a, i) is TRUE if action a is used at step i
- Create variables in each proposition that can hold at different points in time, Proposition(p, i) is TRUE if proposition p holds at step i

### Back to A*
- A planning graph is a relaxed problem, in that it shows everything that *might* be reachable
- The level where a goal first appears never overestimates the cost of reaching it
- ***Planning comes back to state space search with a good heuristic***

## What to know
- Planning is search, and the planners differ in what a search node is
- Solving one subgoal at a time breaks when subgoals interact
- Partial order planning commits to as little ordering as possible
- A planning graph is a polynomial-size, optimistic picture of what's reachable
- Bounded horizon planning: Fix a length, check, extend