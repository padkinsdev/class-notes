# More Query Processing and Optimization

## Distributed database systems
- Storage architectures are usually geographically distributed, not for efficiency but rather out of necessity
- Data replication offers availability, parallelism, and reduced data transfer, but also has the downside of increasing the cost of updates and increasing the complexity of concurrency control
- One method for implementation of data replication is the use of a quorum system, in which both reads and writes go to a majority of machines, but not all machines

### Distributed transactions (two phase commit)
- Each computer has a transaction coordinator and a transaction manager
- Many failures in distributed systems are unique to distributed systems, such as failure of a site, loss of messages, and failures of communication links
- Network partitions and site failures are generally indistinguishable
- Two-phase commits use a centralized coordinator to coordinate commits
- Alternatives to two-phase commits are Paxos and RAFT, which solve a consensus problem to decide whether to abort or commit
- Cryptocurrency networks similarly need to navigate consensus to decide which transactions to accept into the blockchain. Cryptocurrencies, however, pick a consensus leader by having participants solve difficult problems as a proof of work