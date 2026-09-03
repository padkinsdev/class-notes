# Agents

## Introduction
- AI is the science of making machines that act like people
    - The Turing test is a common benchmark for "human-like behavior"
    - Thinking rationally involves deduction and knowledge-based reasoning
- Modern AI emphasizes rational thinking, but what constitutes "intelligence" can be up for debate
    - Rational thinking means choosing actions that best achieve one's goals
    - One component of rational thinking is choosing the action with the highest expected utility since future outcomes are uncertain

## Types of agents
- An agent is an entity that perceives and acts based on those perceptions
    - Agents follow a sense-reason-act loop
    - An **agent program** is an algorithm mapping percept history to an action
- A reflex agent is an agent without a goal. It acts based only on the current percept
- A goal-based agent pursues a specific goal, evaluating future possible actions and their outcomes
- A utility-based agent replaces binary functions with a utility function. Its decision logic involves evaluating scores for all valid options and picks an action sequence with the highest expected utility
- A general learning agent adapts to new experiences and data. It is composed of multiple components
    - Performance element: Carries out the agent's current policy
    - Critic: Evaluates the agent's actions based on environmental feedback
    - Learning element: Improves the robot's internal model and policy over time
    - Problem generator: Encourages exploration to facilitate better strategies

## Representing agents and the world
- Environmental properties determine agent complexity. Such environmental properties include:
    - Fully vs. partially observable
    - Deterministic vs. stochastic
    - Episodic vs. sequential
    - Static vs. dynamic
    - Discrete vs. continuous