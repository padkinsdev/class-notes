# Knowledge-Based Agents and Logics

## First order logic
- Whereas propositional logic assumes the world contains only facts, first order logic (like natural language) contains objects, relations, and functions
    - Propositional logic:
        - Ontology: Facts
        - Syntax: Atomic sentences and connectives
        - Semantics: Truth tables
        - Inference: DPLL, GSAT, WalkSAT for model checking
        - Complexity: NP-Complete
    - First order logic:
        - Ontology: Objects, relations, variables
        - Syntax: Connectives, quantifiers, constants, variables, predicates & relations, functions
        - Semantics: Binding of objects (more complex)
        - Inference: Unification and resolution with automated theorem solvers
        - Complexity: Semi-decidable

## Quantification

### Universal quantification
- $`\forall x`$ means "for all x...", and allows one to make statements about all objects that have certain properties
- Universal quantification is equivalent to the conjunction of all sentences obtained via substitution of an object for the quantified variable, or the conjunction of all sentences obtained by substitution of an object for the quantified variable

### Existential quantification
- $`\exists x`$ means "there exists an x such that...", and allows one to make statements about some onject without naming it
    - **Note:** $`\land`$ is the natural connective to be used with $`\exists`$, and => is the natural connective to be used with $`\forall`$
- Existential quantification is equivalent to disjunction of all sentences obtained by substitution of an object for the quantified variable, or disjunction of all sentences obtained by substitution of an object for the quantified variable

### Quantifier ordering
- The order of "unlike" quantifiers is meaningful, whereas the order of "like" quantifiers is not, akin to the usage of AND vs. OR in a logical sentence
- Clarity in ordering can be achieved with the use of parentheses

## Axioms and theorems
- Axioms are basic factual information, defined by the user
- Theorems are sentences derived from axioms
- While a knowledge base technically only needs axioms, saving theorems in the knowledge base tends to be beneficial, as it saves time

<!--
## FOL vs. propositional encoding
- First order logic:
    1. Predicate-argument formalism:
        - Entities are terms
        - Properties and relations are high-level predicates
    2. Entity-value-atrribute:
        - The knowledge base is encoded in entity-value-attribute triplets
- Propositional logic:
    1. Atomic propositions + ground implication sentences for each entity
    2. Atomic propositions + propositional clauses for objects
    - Tradeoff: Decidable & SAT-solvable, but requires exponential expansion for large domains n
-->

## Prolog
- Translating FOL to Prolog entails reversing the structure of each statement
    - FOL: Premises => conclusion
    - Prolog (reversed): head := body
- Prolog works via backward chaining, unification/MGU substitution, depth-first search (selecting rules top to bottom in the source code and left to right in subgoals), and backtracking on failure

## Formal properties of FOL and inference systems
1. Soundness: Every sentence derived by the inference engine is systematically entailed by the knowledge base. False conclusions are never produced
2. Completeness: Any sentence entailed by the knowledge base can be formally proven by the inference engine. Resolution refutation is refutation-complete
3. Semi-decidability: If the knowledge base can produce a conclusion, it will be produced (the proof algorithm will eventually terminate with proof). Otherwise, the proof algorithm may go around in circles indefinitely. In contrast, propositional logic SAT is decidable
4. Horn clause tractability: Restricting clauses to Horn form (i.e. at most one positive literal) reduces general resolution exponential search to deterministic linear-time forward or backward chaining. In other words, Horn subsets produce efficiency

## Inference
- The statement $`KB \vdash_i a`$ states that sentence a can be derived from KB by procedure i with the following properties:
    - Decidable: the procedure is guaranteed to halt with an answer (T or F)
    - Soundness (Truth preservation): Procedure i is sound if whenever $`KB \vdash_i a`$, it is also true that $`KB \models a`$; unsound procedures can conclude untrue statements: If $`KB \vdash_i a`$ then $`KB \models a`$
    - Completeness: i is complete if whenever $`KB \models a`$, it is also true that $`KB \vdash_i a`$; a complete procedure is able to derive any sentence that is entailed. i.e., procedure will answer any question whose answer follows from what is known by the KB. If $`KB \models a`$ then $`KB \vdash_i a`$
- Inference rules are operators, and unification determines which inference rules can "fire" by matching premises and conclusions

### Sound rules of inference
- Modus ponens: $`A, A \to B`$ concludes B
- Modus tollens: $`A \to B, \neg B`$ concludes $`\neg A`$
- And Introduction: $`A, B`$ concludes $`A \land B`$
- And Elimination: $`A \land B`$ concludes $`A`$
- Double Negation: $`\neg \neg A`$ concludes $`A`$
- Unit Resolution: $`A \lor B, \neg B`$ concludes $`A`$
- Resolution: $`A \lor B, \neg B \lor C`$ concludes $`A \lor C`$

### Horn clauses
- Horn clauses allow only one positive literal in each clause, and take the form $`P_1 \land P_2 \land P_3 \land ... \nad P_n \to Q_m`$, where n >= 0 and m is either 0 or 1
    - There are necessarily 1 or more symbols on the left of the arrow, and 0 or 1 symbols on the right
- Implications can be rewritten as Horn clauses, e.g. $`A \land B \to C`$ becomes $`C := A,B`$

### Forward (FC) vs. backward chaining (BC)
- FC is data-driven, automatic, unconscious processing, finding the implications of an input
- BC is goal-driven, appropriate for problem-solving, looking for a path to an answer
- The idea of forward chaining is to fire any rule whose premises are satisfied in the KB, add the result to the KB, and keep going until the query is found
- FC derives each atomic sentence that is entailed by the KB, and reaches a fixed point where no new atomic sentences can be derived
    - The final state is a model m, assigning true/false to symbols. Every clause in m is true in the original KB, so m is a model of the KB
    - If $`KB \models q`$, q is true in every model of the KB including m
- The Herbrand Universe of a logic program is the set of all ground terms (terms with no variables) that can be built using the constants and function symbols appearing in the program
- The idea of backward chaining is to work backwards from the query q. This may be done by checking if q is known already or by proving via backward chaining all premises of some rule containing q
    - To avoid loops one must check if the new subgoal is already on the stack
    - To avoid repeated work, one must check if a new subgoal has already been proven true or failed