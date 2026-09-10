# Heuristic Search

## Introduction
- Uninformed search uses only information provided in the problem description and no heuristics
- Informed search uses extra knowledge (heuristics) and estimates closeness to goal
- A heuristic is a rule of thumb for solving difficult problems, and are used to simplify or break down said problems

### Greedy search
- Strategy: Expand a node (on the fringe) that you think is closest to the goal state
    - Heuristic: Estimate distance from each goal state
- Worst case scenario functions like a badly directed DFS

### Fringe search
- Idea: Take into account total path cost
- Implement fringe set as a priority queue taking into account both total path cost g(n) and heuristic value h(n)
    - f(n) = g(n) + h(n)
    - g(n) = cost from the start node to n
    - h(n) = heuristic estimate from n to the goal
- This is an **A\* search**
    - A\* is meant to combine UCS (path cost/backward cost) and greedy (goal proximity/forward cost)
    - A\* terminates when a goal is *dequeued*, not when it is *enqueued*

## Admissible vs. inadmissible heuristics
- Inadmissible (pessimistic) heuristics break optimality by trapping good plans on the fringe
- Admissible (optimistic) heuristics prevent promising plans from being unfairly deprioritized
- A heuristic is admissible (optimistic) if $0 \le h(n) \le h*(n)$ where $h*(n)$ is the true cost to a nearest goal
- Developing admissible heuristics is most of what using A\* entails in practice
- Most of the work in solving hard search problems optimally is coming up with admissible heuristics
    - Often, admissible heuristics are solutions for relaxed problems, where new actions are available

## Tree search
- Failure to detect repeated states can create exponentially more work
- Implementation:
    - Tree search + set of expanded states ("closed set")
    - Expanding the search tree node by node, but before expanding a node, make sure its state has never been expanded before
    - If new, skip it, otherwise add it to the closed set of nodes