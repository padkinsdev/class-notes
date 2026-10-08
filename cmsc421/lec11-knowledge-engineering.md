# Knowledge Engineering and Knowledge Representation

## Knowledge representation
- Logical agents: Inference must obey the fundamental requirement that when one asks a question to the knowledge base, the answer should follow from what has been told to the knowledge base previously (i.e. not making things up)
- Knowledge base: A set of esntences in a formal language representing facts about the world

### Steps for knowledge engineering
1) Knowledge acquisition: Gathering information from domain experts through methods like interviews, observations, text mining/document analysis, automated machine learning, and case studies/historical data
    - Protocol analysis: A psychological research method that elicits verbal responses from research participants (e.g. how the user does something, what their thought process is, etc.)
    - Competency question approach: 
        1) Identify the domain and purpose
        2) Formulate competency questions
        3) Extract key concepts and relationships needed to answer the competency questions
        4) Design and populate the ontology
        5) Test the ontology
2) Knowledge organization and research: 
    - An ontology is the "explicit specification of...onjects, concepts, and other entities...and the relationships...among them"
    - **Ontologies organize the background knowledge used in a formal language**
    - Examples of ontologies:
        - Taxonomy: SKU, Dewey Decimal System, WordNet, Cyc, etc.
3) Knowledge Formalization/Encoding
    - Steps
        1) Choose a representation language
        2) Define the vocabulary
        3) Define predicates and relations
        4) Encode facts, rules, constraints, variables, and quantifiers
        5) Validate the encoding with sample queries and expected inferences
    - Different logics mean different power, and different power means different inference procedures
    - Ontological relationships are all first order logic predicates!
    - Ontology + instances make a knowledge graph
4) Reasoning/Inference
    - Having a big knowledge base/graph, query it like a database or by asking it to deduce hidden properties and facts
        - Complex relationship traversal
        - Pattern matching across entities
        - Dynamic relationship discovery
        - Pat-based queries
        - Hierarchical reasoning
    - Logical inference is a kind of search:
        - State: The facts of the knowledge base
        - "Search": Inference via entailment
        - Successor functions: Application of the possible rules of inference

### Complications
- The main tradeoff is expressiveness and complexity in logic
    - As you add complexity to the logic, expressiveness becomes more complex (decidable vs. undecidable)
    - The nature of the language determines whether one may even do proof with it

### Overview of proof methods
- Proof methods take two forms:
    - Inference rules, which further are either forward and backward chaining or resolution
    - Model checking, which further divides into either DPLL (improved depth-first search) or WalkSat (heuristic probabilistic search)
A procedure is decidable if it is guaranteed to halt with an answer of true or false
- Soundness entails that procedure i is sound if it cannot conclude untrue statements
- Completeness entails that i is complete if it will answer any question that follows from the contents of the knowledge base
- Modus ponens: If P -> Q and P then Q

### Horn clauses
- A Horn clause is a disjunction of literals of which at most one is positive
- Hence, every Horn clause is an implication whose premise is a conjunction of positive literals with a conclusion that is a single positive literal

### Inference
- Forward chaining is data driven, automatic, unconscious processing, finding the implications of an input
- Backward chaining is goal driven, appropriate for problem solving, looking for a path to an answer
- Inference is easier with Horn clauses if you restrict the syntax of first order logic to Horn clauses to reduce inference complexity
    - Put the knowledge base into Horn clauses, then use modus ponens over the Horn clauses to forward or backward chain with modus ponens

### Inference with Prolog
1) Assert facts (TELL)
2) Use rules/implications to prove conclusions via queries (ASK)

#### Properties of Prolog's inference
- Sound for definite Horn clauses
- Not complete: Depth-first search can loop forever
- Closed-world assumption: `false` means "not provable"
- Datalog: Definite clauses + no function symbols

### Real life uses of logic
- AI Guardrails: AWS Bedrock uses formal reasoning to verify LLM outputs don't violate policies
- Knowledge Graphs: Google's KG answers billions of queries via structured inference