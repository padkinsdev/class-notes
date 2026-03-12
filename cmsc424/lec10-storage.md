# Storage and Indexes

## Computing structure
- Computing hardware -> storage organization -> B+-tree indexes -> hash indexes -> log-structured merge (LSM) trees
- Computing hardware consists of components that are resistant to power failure, such as hard drives, disks, tapes, etc.
- Magnetic or hard disks are the primary long term storage mechanism
- The limitation of hard drives is the need to lift disks to search, as well as time to seek data
- While hard drives have a long mean time to failure, when one is dealing with a large number of drives, it can reasonably be assumed that disk failure on a single drive will occur with some frequency
- Solid state drives (SSDs) emulate the interface of a hard disk drive but use flash memory instead of disks
- SSDs still write to blocks but don't perform seek operations
- Writing to SSDs requires erasing an entire block and then rewriting it. This causes wear on the drive
- Wear leveling involves distributing block writes across an SSD so that it wears out evenly
- SSDs tend to be more expensive than HDDs
- A lot of database design decisions were made in a time when HDDs were the dominant form of storage, memory was sparse, and networks were slow. Communication between memory and disks was the main bottleneck
- PostgreSQL is notable a single-threaded system
- Networks have become so fast that it's faster to access another computer's memory than to access local disk memory