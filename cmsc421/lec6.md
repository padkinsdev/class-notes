# In Search of Lost Time - Time, Space, and Approximation

## Scale
- A* is very memory-intensive, being O(number of states)
- Effective branching factor: The exponential savings relative to baseline that a heuristic produces
- Despite being relatively simple, problems like ther 8-puzzle and solving a Rubik's cube have very large search space sizes
    - Many such puzzles would require more memory than the average computer has to hold the entire search space for exhaustive checking of a solution

## Time
- Time in the context of programming consists of
    - CPU time, or the number of seconds taken to execute something
    - Actual (wall clock) time, or the number of elapsed seconds between the start and completion of something

## Performance vs. Effectiveness
- The fundamental tradeoff in AI is performance vs. effectiveness
    - Performance is how many steps, how much memory, etc. it takes to run something
    - Effectiveness is how well something works, e.g. how optimal it is, what the cost of the solution is, whether a solution succeeds, etc.
    - Effectiveness becomes a factor when solution quality or solution existence are not guaranteed
- Provably complete algorithms are mathematically guaranteed to find the optimal solution if it exists
- Stochastic or probabilistic algorithms may or may not find a solution if one exists
- Effectiveness may be absolute or relative, with thresholds
    - Absolute: How close to optimal a solution got
    - Relative: The proportional improvement from baseline
    - Thresholds may impose a binary cutoff

## Measuring success in AI
- Designing reproducible experiments, including standardizing the number of samples and choosing what data to collect/analyze is essential

### AI memory
- Memory is stored implicitly
- Interactions produce excitation in the network, not a search over explicit states
- Queries are executed as patterns of computation over the weights

## Algorithms for A*
- Iterative deepending A*: Cutoff information is the f-cost (g+h) instead of depth
- Recursive best-first search: Recursive algorithm that attempts to mimic standard best-first search with linear space
- Memory-bounded A* ((S)MA*): Drop the worst-leaf node when memory is full

### Iterative deepening A* (IDA*)
- Idea: Use f(n) = g(n) + h(n) with admissible and consstent h
- Each iteration is depth-first with cutoff on the value of f of expanded nodes
- IDA* is complete if one keeps increasing the cost threshold, guaranteed to find the optimal solution if h(n) never overestimates the cost of the goal, and not generally more costly than A*
    - More precisely, IDA* is only more costly than A* by a constant factor of 2
- *Sometimes there's not enough time and you just have to sacrifice effectiveness for better performance!*

## Time (again) and approximation
- When you have a hard problem and no optimal solution, you *approximate*
- An algorithm, ALG, for a minimization problem is an alpha-approximation algorithm (meaning that its approximation factor is alpha) if for every input instance x, $`OPT(x) \le ALG(x) \le \alpha * OPT(x)`$

### Anytime algorithm
- A subset of approximation algorithms used when real-time computation is important
- Stable and interruptible, and may return several answers
- Essentially useful when you need something "good enough" given real-time constraints (like robotics), i.e. given the extent of what has been explored so far
- Some difficult problems like the knapsack problem and route finding are suitable for anytime algorithms, but others, like an 8 puzzle, aren't

#### Newton-Raphson method
- The classical algorithm for numerical analysis. Find the root r for an equation f(x) such that f(r) = 0
- Notably interruption still yields a solution, but more iteration yields a better solution

#### Anytime algorithms for A*
- The core principle is to find a working but not necessarily optimal solution which can be improved upon given more time
- Thus, A* with a weighted heuristic, or in other words go deep quickly and improve as necessary
    - $`f(n) = g(n)+\epsilon*h(n)`$, $`\epsilon \ge 1`$
- Anytime repairing A* works by executing A* multiple times, starting with a large epsilon and decreasing prior to each execution until epsilon = 1
    - Rather than running A* from scratch each time you decrease epsilon, update the f values in the path based on the new epsilon and continue running A*

#### Contract algorithms, a subset of anytime algorithms
- Iteratively produce solutions of improved quality
- Whenever unexpectedly interrupted, return the best solution produced so far
- Time is allocated a priori, and a high quality solution is produced at the end of the allotted time

## Conclusion
- GPUs are great for neural networks because neural nets are just a whole lot of linear algebra
- The key attribute of AI is manipulation of symbolic data, managing lists, and pattern matching
- Sometimes you just need something that's good enough
- Again, the key tradeoff in AI is performance vs. effectiveness