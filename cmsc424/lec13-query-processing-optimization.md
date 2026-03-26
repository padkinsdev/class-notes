# Query Processing and Optimization

## Introduction
- Each operator implements three functions: open(), next(), close()
- Systems like PostgreSQL use demand-driven pipelining with the "iterator" model
- The system (e.g. PostgreSQL) asks the operating system for a large chunk of memory, then builds a query plan
- The cost of performing a query is difficult to assess, as the question is no longer simply about the number of blocks written. Factors now include CPU instructions, disk I/Os, network usage, memory uses, etc.