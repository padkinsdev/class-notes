# Architectures

## Database architectures
* Originally databases were built for limited memory, single core processors, and data stored on a disk
* Nowadays databases have access to multicore processors, large caches, large memory, and disks as backup storage to handle memory volatility

## Data representation
* Disks are usually divided into blocks of data, usually 4/8/16 KB
* Memory is divided into the same size blocks, and the buffer manager handles transferring data back and forth in the form of blocks
* Cache is divided into cache-lines, usually 64 B or 128 B
* Each relation is stored on a separate set of blocks, and assumed to be contiguous
* There is no best way to store data. Storing data in a sorted format allows for better searches but worse inserts, and so on
* Data should be laid out column-wise so that cache lines can be aligned in such a way that specific data can be predictable found. This improves cache performance
* Binary searches are not sufficiently efficient for finding specific records, thus requiring the use of B+ trees
* Column oriented storage stores column values together rather than tuples, which makes reconstructing a tuple more expensive, but makes getting multiple values from a single column easy. It also makes adding new tuples costly
* High performance database systems compress data on disk to minimize read time, but require fast decompression algorithms
* Data warehouses (almost) exclusively use columnar storage
* One caveat in data storage is that DBMS don't usually directly control how data is actually stored on disk, but rather access the disk through the OS
* It is possible but uncommon to have a DBMS that directly works with the disk or uses a lightweight OS
* It is possible to have separate files for each relation, but more common to have a single file that is broken up into a series of blocks holding all of the relations together