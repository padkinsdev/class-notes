# SQL + Programming Languages

### Background
- Most developers don't use SQL directly, opting instead for a programming language like Java or Python. This creates an *impedance mismatch* between how data is represented in memory(usually objects) and how it is stored (relational database)

### Protocols

#### JDBC/ODBC
- JDBC/ODBC (Java Database Connectivity/Object Database Connectivity): Doesn't solve the impedance mismatch problem but rather converts result tuples into objects
- JDBC does expose the application to SQL injection or malformed queries if query strings are built using string concatenation instead of format strings (a common issue across platforms)
- JDBC is specific to Java, ODBC is an open standard shared between languages
- SQL standard defines embeddings of SQL in a number of programming languages, including C, C++, Java, Fortran, PL/1
- Often times there are vendor-specific libraries for various platforms (e.g. PostgreSQL) that use internal protocols

#### Object-relational mappers
- Aimed at solving the impedance mismatch
- Takes care of the mapping between objects and the database
- Programmer works with objects, not raw SQL
- ORMs work with entities/objects, not relations
- Examples are Django, React, Vue, etc.

#### Other mappers
- Sometimes higher level languages will simply map to SQL, allowing an intermixing of code and SQL
- Used to provide alternate data models such as graphs that are mapped to relational databases behind the scenes
