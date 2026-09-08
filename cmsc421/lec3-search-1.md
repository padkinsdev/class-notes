# Search Part 1

## Review

### Reflex agents
- What makes a reflex agent powerful?
    - Fast decisions
    - Simple and efficient
    - Reliable in predictable environments
    - Easy to implement and understand
    - Works well for routine tasks
- Examples:
    - Automatic emergency braking
    - Elevator door obstruction sensor
    - Smoke detector alarm
    - Motion-activated security light

### States
- A state definition is not about listing everything in the world, but rather everything relevant for what to do next
- The same environment can have multiple valid state representations
- SAT, or the problem of determining variable assignments to satisfy a boolean statement, was the first problem to be determined to be NP-complete

## Search
- **Planning:** Finding a sequence of actions that leads to an initial state or goal state
- **Search (in planning)**: Systematically exploring states or actions to find a sequence of steps that leads to a goal
- **Planning agents:**:
    - Ask "what if?"
    - Make decisions based on hypothesized actions
    - Must have a model of how the world evolves in response to actions
    - Must formulate a goal (test)
    - Consider how the world *would be*

## State space graph
- **State space graph**: A mathematical representation of a search problem
    - Nodes are (abstracted) world configurations
    - Arcs represent successors (action results)
    - The goal test is a set of goal nodes
    - In a state space graph, each state appears only once
- A search problem consists of a state space, a successor function, a start state, and a goal test
    - A solution is a sequence of actions (a plan) that transforms the start state to a goal state

## Search trees
- A tree representing each possible sequence of steps that the agent can take (a "what if" tree of plans and their outcomes)
    - The start state is the root node
    - Children correspond to successors
    - Nodes show states, but correspond to *plans* that achieve those states
    - For most problems the search tree is too large to build
- Each node in the search tree is an entire *path* in the state space graph
- Both search trees and state space graphs are constructed on demand, with as little constructed as possible
- The fringe is a list of paths (plans) that have yet to be followed to completion
- $b$ is the branching factor of the tree, and $m$ is the maximum depth

### Depth first search (DFS)
- Strategy: Expand the deepest node first
- In practice the fringe is a LIFO stack
- If m is finite, DFS can take $O(b^m)$, where m is the maximum depth of the search tree
- The fringe takes $O(bm)$ space
- DFS is not optimal because it finds the leftmost solution, regardless of depth or cost
- DFS is only complete if no cycles exist in the state space graph

### Breadth-first search (BFS)
- Strategy: Expand the most shallow node first
- In practice the fringe is a FIFO queue
- The search takes time $O(b^s)$ where $s$ is the depth of the most shallow solution
- The fringe takes space $O(b^s)$ where $s$ is the last tier examined (the current tier)
- BFS is optimal as long as costs are all 1, and complete as long as a solution exists

### Iterative deepening search
- Idea: Get DFS's space advantage with BFS's time/shallow-solution advantages
    - Run a DFS with depth limit 1. If no solution...
    - Run a DFS with depth limit 2. If no solution...
    - Keep running DFS with limit +1 for each time a solution isn't found
- Most work occurs in the lowest level searched, so this iterative approach isn't too computationally wasteful

## Cost-sensitive search
- BFS finds the shortest path in terms of actions, but it does not find the least-cost path

### Uniform cost search (UCS)
- Strategy: Expand the cheapest node first
- The fringe is a priority queue, where the priority is cumulative cost
    - In other words, the *cumulative* cost of the current path is considered, rather than just the cost of the current move (as part of the path)
- UCS processes all nodes with cost less than the cheapest solution
    - If the cheapest solution costs $C*$ and arcs cost at least $\epsilon$ then the "effective depth" is roughly $C*/\epsilon$
    - The "effective depth" is the depth of the tree taht is actually being searched during a search operation
- The fringe for UCS is composed roughly of the last tier, so it takes space $O(b^{C*/\epsilon})$
- UCS is complete and optimal, but does not give information about goal location, and blindly explores options in every direction
    - This means that if there is knowledge about the approximate location of the goal, UCS will not take that information into consideration

## Wrapping up
- BFS, DFS, and UCS are the same aside from fringe strategies
- For DFS and BFS, the $log(n)$ overhead can be avoided by using stacks and queues rather tahn a priority queue
- A bidirectional search searches from both ends, and stops when the two frontiers meet
    - Bidirectional search halves the depth of the search, and is memory-efficient, especially in exponential spaces