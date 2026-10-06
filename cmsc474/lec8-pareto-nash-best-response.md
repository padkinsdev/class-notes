# Pareto Optimality, Nash Equilibria, and Best Responses

## Normal form games
- Only two types of strategies exist in normal form games:
    - Pure strategy (a single action)
    - Mixed strategy: Probability distribution over pure strategies
- While the normal form game representation may seem heavily restricted, many complex games can actually be mapped to normal form games

## Reasoning about games
- In single-agent decision theory, look at an optimal strategy (maximizing payoff in the environment)
- With multiple agents the best strategy depends on others' choices. Deal with this by identifying certain subsets of outcomes called solution concepts (e.g. Nash equilibrium)
- Related: Pareto optimality and Nash equilibrium

### Pareto optimality
- Pareto optimality deals with *strategy profiles*, being a conjunction/combination of strategies from all agents
- A strategy profile s Pareto dominates a strategy profile s' if
    - No agent gets a worse payoff with s than s' (i.e. $`u_i(s) \ge u_i(s')`$ for all i)
    - At least one agent gets a better payoff with s than s' (i.e. $`u_i(s) \gt u_i(s')`$ for at least one i)
- A strategy profile s is Pareto optimal (or Pareto efficient) if there's no strategy profile s' that Pareto dominates s
    - Every game has at least one Pareto optimal profile
    - There is always at least one Pareto optimal profile in which the strategies are pure

### Best response
- If agent i knows how others are going to play then i has an ordinary optimization problem
- Use $`s_{-i}`$ to mean a strategy profile for all agents except i
    - $`s_{-i} = (s_1, s_2, ..., s_{i-1}, s_{i+1}, ..., s_n)`$
- Let $`s_i`$ be any strategy for agent i. Then $`s_{-i} = (s_1, s_2, ..., s_{i-1}, s_i, s_{i+1}, ..., s_n)`$
- $`s_i`$ is a best response to $`s_{-i}`$ if for every strategy $`s_i'`$ available to agent i, $`u_i(s_i, s_{-i}) \ge u_i(s_i', s_{-i})`$
- There is always at least one best response
- A best response is unique if $`u_i(s_i, s_{-i}) \gt u_i(s_i', s_{-i})`$ for every $`s_i' \ne s_i`$
- Given $`s_{-i}`$, there are two possibilities:
    - i has a pure strategy $`s_i`$ that is a unique best response to $`s_{-i}`$
    - i has infinitely many best responses to $`s_{-i}`$

### Nash equilibrium
- $`s = (s_1,...,s_n)`$ is a Nash equilibrium if for every i, $`s_i`$ is a best response to $`S_{-i}`$. Thus:
    - Every agent's strategy is a best response to the other agents' strategies
    - No agent can do better by *unilaterally* changing their strategy
- In 1951 Nash showed that every game with a finite number of agents and actions has at least one Nash equilibrium

#### Strict vs. weak equilibria
- A Nash equilibrium $`s = (s_1,...,s_n)`$ is **strict** if for every i, $`s_i`$ is the *only* best response to $`s_{-i}`$ (i.e. any agent who unilaterally changes strategies will be guaranteed to do worse)
    - Given that if a best response is unique it is also pure, it follows that each strategy in a strict Nash equilibrium is pure
- A weak Nash equilibrium is any Nash equilibrium that isn't strict (at least one agent i has more than one best response to $`s_{-i}`$)
- Additionally, if a Nash equilibrium includes any mixed strategies, it is weak. However, an equilibrium with only pure strategies can be weak as well
- Weak Nash equilibria are less stable than strict ones because it means that at least one agent has infinitely many best responses, and only one of them is in s
- ***In short, all strict Nash equilibria are pure, but not all pure Nash equilibria are strict***