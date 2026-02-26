# Normalization

## Functional dependencies (FDs)
- FDs capture "domain knowledge"
- Armstrong's axioms are sound (do not infer incorrect FDs) and complete (infer all correct FDs). They can also be used to infer more axioms via transitivity
- If `A -> B` then A is a super key, a collection of items that can uniquely identify items in the set. A candidate key is a super key with no additional attributes and which does not contain any subsets which are themselves super keys
- Basically, super keys can uniquely identify tuples in a relation, and candidate keys are super keys with no subsets that are themselves super keys
- We know that something is a super key if nothing implies it in a relation
- **Rule**: A decomposition of R into (R1,R2) is *lossless* iff `R1 intersect R2 -> R1` or `R1 intersect R2 -> R2`. This guarantees that joining R1 and R2 always produces R without extra tuples

### **Bryce-Codd Normal Form (BCNF)**
- A relation schema R is "in BCNF" if for every functional dependency the left side is a superkey or the FD is trivial (A -> B where B subset R). This aims to reduce (FD-based) redundancy
<!-- To achieve a BCNF schema, for all dependencies `A -> B` on the schema, check if A is a super key. If not, choose a dependency that breaks the BCNF rules (`A -> B`)-->

### 3NF
- BCNF can't always preserve dependencies (in the case of a circular dependency), requiring a weaker normal form to account for such cases
- **Prime attribute**: An attribute contained in some candidate key for R
- Given R and FDs, R is in 3NF if for every FD `A -> B`:
    * `A -> B` is trivial
    * A is a superkey of R, or
    * *All attributes in `(B - A)` are prime*
- 3NF is good because the first two rules are the same as BCNF but rule 3 relaxes BCNF by allowing limited redundancy

### Multi-level normal forms
- Multi-valued dependencies are denoted with two arrows, e.g. `A -> -> B`
- Unlikely to arise if the schema was constructed from an E/R diagram
<!-- In 4NF given a relation schema R, and a set of multivalued dependencies F, if every MVD-->
- The choice between 3NF and BCNF is uo ti the schema designer

## Schema creation
- You can create a schema using an E/R diagram, with a universal relation R that contains all attributes<!--, and-->
- The fundamental goal is to eliminate FD and MVD dependencies
- Sometimes denormalization is necessary for performance reasons (too many joins)