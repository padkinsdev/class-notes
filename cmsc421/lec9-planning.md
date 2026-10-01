# Planning (INCOMPLETE)

## Introduction
- Entailment means, in essence, "a follows b"

### Situation calculus
- A situation is a snapshot of the world at an interval of time during which nothing changes, meaning that every true/false statement is made with respect to a particular situation

## Propositional inference
- Why not do planning as logical inference?
    - Computational complexity: First-order logic is semi-decidable
    - Frame problem: Pure logic requires explicit frame axioms to state everything that doesn;t change when an action occurs
    - Qualification and Ramification problems: Logic struggles to concisely encode endless real-world preconditions (qualification) and indirect side effects (ramification) of actions
    - Lack of Directed Search Heuristics: Standard logic deduction treats all valid inferences equally
    - Logic is usually monotonic, i.e., new facts are proven and become true---once true they are never again false. Hard to track change.

## Better idea (STRIPS)
- Start state, goal state are specified as conjunctions of predicates
- Actions are described in terms of their preconditions and effects
- Search problem: Starting with the start state $`S_0`$ find all 'applicable actions' (i.e., actions for which preconditions are satisfied: $`S_0`$ ⊨'precond') , compute the successor state, $`S_1`$, based on the effects; repeat as necessary
- Note: The goal state need not be all-encompassing. Some things don't affect whether the goal is achieved, and correspondingly need not be defined in the goal state
- Determining which actions can be taken requires checking the preconditions for a given action

### Unification
- Unification is the machinery for variable substitution
    - I.e. when two things become one
    - Akin to assignment or substitution, equality in algebra, and pattern matching

### Applying STRIPS
- Often it can be costly to map out each possible action for each possible state, so backtracking from the goal (regression) can be a better approach
     - The regression of a goal G through an action A is the weakest precondition R[G,A] such that if a state S satisfies R[G,A] the preconditon of A is satisfied in S and applying A to S yields a state that satisfies G
- Forward chaining is data-driven, automatic, unconscious processing, finding the implications of an input
- Backward chaining is goal-driven, tailored to solve specific problems, looks for a path to an answer

## PDDL: Planning Domain Definition Language
- PDDL sits at a specific point in a hierarchy of logical languages
- Components of PDDL:
    - Objects: Things in the world that interest us.
    - Predicates: Properties of objects that we are interested in; can be true or false.
    - Initial State: The state of the world that we start in.
    - Goal: Things that we want to be true.
    - Actions/Operators: Ways of changing the state of the world.
- Planning tasks are specified in two files:
    - Domain file for predicates and actions
    - Problem file for objects, initial state, and goal specification