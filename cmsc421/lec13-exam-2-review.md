# Exam 2 Review

## Logic
- Logics are formal languages for formalizing reasoning, in particular for represennting information such that conclusions can be drawn
- Inference:
    - $`KB \vdash_i a`$ says that sentence $`a`$ can be derived from KB by procedure $`i`$
    - Propreties of inference:
        - Decidable: Procedure is guaranteed to halt with a T/F answer
        - Soundness (truth preservation): Unsound/innacurate statements are not returned. In other words, the procedure will not return an inaccurate result to a query. If $`KB \vdash_i a`$ then $`KB \models a`$
        - Completeness: Able to derive any sentence that is entailed by the knowledge base. In other words the procedure will answer any query whose answer follows from what is known in the KB. If $`KB \models a`$ then $`KB \vdash_i a`$
- Syntax: Whether a sentence is grammatically correct
- Semantics: The meaning of a sentence
<!-- A model for a KB is a possible world-->
- Logical inference is a kind of search, with the knowledge base as the state and the search being inference via entailment

### Forward chaining (FC)
- FC derives every atomic sentence which is entailed by the KB, and reaches a fixed point where no new atomic sentences can be derived
- The final state is a model m, assigning true/false to symbols
- Every clause that is true in the KB is also true in m
- Essentially covering every possible logical deduction based on what's in the KB

### Backward chaining (BC)
- Work backwards from the query until a path is found
- Crucially BC only proves what needs to be proved to answer the query, whereas FC proves everything that can be proved

## First order logic (FOL)
- Entailment in FOL is the same as in propositional logic
- Inference in FOL is undecidable, so the options are:
    - Reduce FOL to propositional inference
    - Lifted inference
- Entailment in FOL is semi-decidable

### Propositionalization
- Convert FOL to propositional logic and use a SAT solver to check (un)satisfiability
- Primary issue: Doing this blows up the number of propositions

### Lifted inference
- Apply inference rules directly to first-order sentences

## AI Planning
- Representing change in the world with logic can be tricky, but not impossible
    - One way: Change the KB (add/delete sentences from the KB to reflect changes)
    - Better way: Situational calculus. A situation is a snapshot of the world at some instant in time
- Again, a situation is a snapshot of the world at an interval of time during which nothing changes. Every true/false statement is made with respect to a particular situation
- Frame axioms: If a property x doesn;t change as a result of applying action a in state s, then it stays the same
    - The proliferation of frame axioms becomes cumbersome in complex domains
- Successor-state axioms: General statement that characterizes every way in which a particular predicate can become true
    - Either it can be made true or it can already be true and also unchangeable
    - In complex worlds, where you want to reason about longer chains of action, this becomes impractical
- Planning with logic is elegant but impractical!

### Propositional planning
- Start stae, goal state are specified as conjunctions of predicates
- Actions are described in terms of their preconditions and effects
- Planning is more or less a search, akin to a simplified inference
    - Specifically planning searches not over states but rather over plans
- STRIPS cannot handle:
    - Hierarchical plans
    - Complex conditions: No universal quantification of variables
    - Resources: E.g. budget, materials, workers, etc.
    - Time: All actions are assumed to occur instantly
- Hierarchical decomposition planning:
    - An abstract (non-primitive) operator can be decomposed into a series of substeps
    - A primitive operator (STRIPS-like) is one that can be directly executed
    - A good way to think of this is like a macro (e.g. in C)
    - Issue: Interactions can occur between different parts of a plan
    - A hierarchical planner only searches decompositions that lead to solutions
    - An abstract solution is a complete and consistent plan with abstract operators