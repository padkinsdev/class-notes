# Query Langauages (Part 2)

### Operations
- There are a wide variety of full and partial joins, such as cross product, natural join, theta join, etc.
- SQL's allows for the use of "subqueries" or nested queries. The innermost query will be evaluated first, and then evaluation moves outward
- In general, queries are "flattened" by a query optimizer
- Subqueries can also be used to construct scalars or sets to be used in other queries, or to construct a new table "on the fly" which to query from. Notably **the difference between a table and a set is contextual, but in general they are the same thing**
- Constructing tables can be done either as a nested query or as a common table expression using the `with` keyword, e.g. `with <TABLE CONSTRUCTION> <MAIN QUERY>` vs `select ... from <TABLE CONSTRUCTION> where ...`
- Subqueries can also be used to construct a "function" for computing something that requires other relations