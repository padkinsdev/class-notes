# B+ Trees

<!-- All of my stuff in my bag is soaked and I was late for class so these notes are incomplete -->
## Introduction
- B+ trees group similar entries in a table by the values that they share, with entries having a common value in one column being leaves (or interior nodes if not a full entry) of their common value
- E.g. if two rows share the value "Mozart" in the teacher column, they will be leaves of the "Mozart" inner node
- B+ trees are balanced, meaning that they automatically adjust to data insertions
- B+ trees are also ordered, maintaining sorted keys
- PostreSQL uses B+ trees as the internal data representation, but they are not considered particularly efficient nowadays, and better options exist
- Nowadays log-structured merge trees are used instead

## Log-structured merge (LSM) trees
- SSDs don't like small writes, insteadd preferrring large block writes
- LSMs are very commonly used nowadays, e.g. in RocksDB, Cassandra, LevelDB, etc.
- LSMs are held in memory as SSTables (sorted string tables) and have an overhead index for in-memory searches. The index is not persisted to disk
- If the SSTable becomes too large due to inserts and whatnot, it will be written to disk
- SSTables are time-stamped. If the current SSTable cannot fulfill a read request, the most recent SSTable with the desired entry will be used
- The stories of timestamped versions of an SSTable can and likely will create redundancies by storing multiple copies of the same data if the data is consistent between table versions
- Basically, the table itself is held in volatile memory (E.g. RAM) and periodically flushed to disk with a corresponding timestamp in order to limit the amount of volatile memory used to a desired size
- Periodically, flushed table versions are compacted into a single table. This is essentially a sort merge and thus is fairly quick. After the compaction the compacted SSTable versions are discarded as they are no longer useful
- Rather than sequentially searching each SSTable version for data in a read operation, a Bloom filter is used to vastly accelerate the process of locating the desired data across table versions