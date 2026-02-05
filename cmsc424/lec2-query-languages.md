# Query Languages

### Relational algebra symbols
- sigma: selection operator equivalent of SQL `select * where` clause
- pi: projection operator which is equvalent to SQL `select * from` clause
- union, intersection, difference: input 2 collections, output one collection
- cartesian product (x): join two colloections

### Basic SQL/Spark query structure
- `select` A1, A2, A3, ... `from` r1, r2, r3, ... `where` P
- Apache Spark uses a "map" operation. No explicit schema is given, so code must be provided, e.g. `l.map(lambda line: re.match(r'^(\S+)', line).group(1))`
- SQL allows duplicates by default (must be specified otherwise via `select distinct...`), but relational algebra implies no duplicates
- `t.mapValues(f)` is syntactic sugar in Spark, applying on the map operation to the data
- `l.flatMap(f)` takes an object collection and outputs an object collection, applying a function to each row which may generate >=0 objects, thus "flattening" the input. An example of this is `flatMap(lambda x: x.split())`, which would further split each row into additional rows by splitting each row on a space character. `flatMap` is somewhat convoluted to implemented in SQL.
- `t.groupByKey()` takes a collection of key/value pairs and groups them by identical key, e.g. two key/value pairs a->b and a->c would be grouped as a single key/value pair a->\[b,c\]
- **Important: Rows may not be processed linearly in SQL, so order cannot be relied on when, for example, grouping by identical key**
- Relational algebra does not include support for grouping e.g. by key
- `groupBy` can often also be used in conjucntion with a mapping function to transform each value as it is being grouped, e.g. `reduceByKey` or `aggregateByKey`. An example of group by aggregates in SQL is `select` dept_name, `avg` (salary) `from` instructors `group by` department_name
- Common aggregates in SQL include `max`, `min`, `sum`, `count`, `stdev` and can be nested
- `unnest` is the opposite of `nest` or `groupBy`
- SQL removes duplicates by default in set union/intersection/difference operations, and must be specified otherwise with the `all` keyword, e.g. `union all`
- SQL does a cartesian product for all tables in the `from` clause, i.e. joining them together before filtering/selecting/etc
- A proper `join` operation is a variation on a cartesian product, joining all of the objects in two collection that satisfy a certain condition into one collection. There are 4 types of joins: inner, right, left, and full outer