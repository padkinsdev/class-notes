# Data Modeling Layer

### Entity-relationship model
- The distinction between entities and relationships may not be so clear, so the process is iterative
- The types of attributes include primary, composite, multi-valued, and derived keys
- Constraints limit the number of relationships an entity can participate in, and are enforced by the database
- Sometimes a relationship associates an entity set with itself, and thus need roles to distinguish
- A **weak entity set** is an entity set without enough attributes to have a primary key. Discriminator attributes distinguish between weak entities
- The E/R model has the capacity for inheritance, similar to object oriented programming
- The E/R model is a newer construct than the relational model. It is usually used for initial design and then translated to a relational model to allow for use of SQL on the relational model. As such, the E/R model is much more about representing data than about actually storing or querying it

## Mapping E/R to relational model
- A mapping between the two converts entity sets into a relational schema with the same set of attributes, which typically has a constraining effect
- In some cases rather than having separate tables to bridge other tables (thus potentially requiring three way joins to related the two tables of interest), it is reasonable to simply join the two tables such that the bridging table is eliminated
- Mapping weak entity sets involves adding data to the weak entity table such that a primary (unique) key can be used in the table. In other words, find a way to make each row in the weak entity set table representation unique
- Multi-valued attributes must be either combined into a single column (e.g. an array column), flattened, or broken from the main table to create a separate table that is linked to by the main table
- For tables which inherit or link to data from one another, one may keep the tables separate thus potentially requiring table joins for composite queries, or create a common table for common information and separate tables for additional information

## Design mistakes
- Try not to repeat data between tables unless entries are expressly intended to be used to link the tables together (e.g. for joins). In other words, any foreign key should be a relationship, not a stand-alone attribute
- When should something be an entity vs. an attribute? Generally if an attribute would otherwise be repeated, or if there is data to be associated with that thing specifically, it should be an entity (table). There usually isn't necessarily a correct answer though
- N-ary vs. binary relationships: Sometimes it's advantageous to use n-ary relationships, but there is a standard process for breaking n-ary relationships into binary relationships

## Unified modeling language (UML)
- More comprehensive than relational models, and has the ability to model E/R models
- More standardized than E/R diagrams

## Normalization
- Asks the question "Is this schema good? Is it the best possible?" and attempts to formalize the definition/approach for a "good schema"
- Fundamentally, why not just combine as many tables together as possible for simplicity? Generally speaking, doing so creates repetitions in data representation. Such redundancy requires more storage as well as update/insertion anomalies
- Try to avoid sets within single attributes because they are hard to represent and hard to query
- Conversely, splitting data excessively between tables requires frequent table joins and can create a lossy decomposition by potentially removing relationships/constraints between data

### What makes a good schema then?
- No sets
-No lossy decomposition
- No redundancy (avoids anomalies)
- Nulls shouldn;t be required to store information
- Dependency preservation

### Functional dependencies
- Written A -> B (A "implies" B)
- For example, in a table listing students, entries with the same student id value will also have the same student name
<!-- Let R be a relational schema and `a subset R` and `b subset R. The functional dependency a -> R holds on R iff -->
- There is a difference between an instance vs. legal dependency. The difference is whether the dependency *actually* holds in all cases
- FDs cause redundancy by repeating information. This can be circumvented by splitting the dependency into two tables and making the predicate (i.e. A in A -> B) a key for the new table
- A set of FDs may imply other FDs, e.g. if `A-> B and B -> C then A -> C`
- Armstrong's axioms:
    * If `b subset a then a -> b`
    * If `A -> B then AC -> BC` for any C
    * If `A -> B and B -> C then A -> C`