# LSM Tree Fundamentals
## Background
Standard disk-based index structures as B-Tree is not suitable for maintaining index structures, as it will double the I/O cost of transaction to maintain an index in real-time, which resulted in the 50% rise of total system cost. For that reason, as the world is moving more rapidly, the more data is generated and needed to be monitored and analyzed; therefore, a new data structured called LSM-Tree is proposed and implemented in new database engines to mitigate the overhead of high write throughput.

LSM-Tree, fully known as Log-Structured Merge-Tree, is also a disk-based data structure designed to optimize the process of inserting/deleting indexes at real-time. LSM-Tree uses an algorithm to defer and batch index changes, cascading the changes from from a memory-based component to one or more disk-based components efficiently via merge sort. The algorithm has truly reduced the overhead of continuously writing into the disk compared to traditional access method of B-Tree, which overwhelm the storage media cost. However, there's a catch, the LSM-Tree lacks I/O efficiency of immediate response upon finding or retrieving indexes. In general, LSM-Tree is the most useful when the application is write-intensive rather than read-intensive (Time-series DB, etc.) [1].

Some of the common storage engine which takes advantage of the LSM-Tree data structure which is LevelDB (by Google), RocksDB (by Meta/Facebook). Interestingly, there are many more high-performance databases which are built upon those storage engine such as [TiKV](https://github.com/tikv/tikv), [CockroachDB](https://github.com/cockroachdb/cockroach) and many more.
![RocksDB](Attachments/Pasted%20image%2020220922170659.png)
<center>Figure 1. RocksDB logo by Facebook (now known as Meta)</center>

> Interesting Info: RocksDB is a persistent key-value storage engine, developed by Facebook (Meta), in which keys and values are arbitrary byte streams [2]. It is built as a C++ library for system's persistence layer (disk storage).
## Functionality
### Structure
An LSM-Tree is a data structure, like other trees, it maintains key-value pairs via nodes. 

One of the most fundamental implementation of LSM-Tree is a two-level LSM Tree based on the idea of [Patrick O'Neil](https://en.wikipedia.org/wiki/Patrick_O%27Neil). It consists of C0 and C1 trees, in which the smaller C0 tree resides in the system's memory (RAM) and the C1 one resides on disk. Records which are inserted into to the DB, must first go through the in-memory C0 component. If the size of the C0 tree exceeds its initial storage, part of the C0 tree (contiguous segment of entries) is removed from C0 and merged into C1 on disk [3].

Nowadays, there are multiple levels in LSM-Tree in order to improve the write throughput to the DB. To be specific, if the C0's capacity is exceeded, a segment of entries from C0 is written to C1 same as two-level LSM-Tree. However, even after the written entries to C1 exceeds its capacity, the C1 tree becomes immutable and the C0 tree will continue to write into C2, C3, and more. Such implementation will always have one active Cn LSM-tree for C0 to write entries on.
![LSM-Tree's Structure](Attachments/Pasted%20image%2020220922170936.png)
<center>Figure 2. LSM-Tree structure levels</center>

### Architectural Components
The back-end components inside a fully functional LSM-Tree of the database is quite complex in order to achieve durability upon high throughput of write requests to the Database [4]. Below is the diagram indicated the interaction of different components inside LSM-Tree for its functionality.

LSMT-based Databases providing high throughput usually consists of six essential components:
- **WAL** (Write-ahead ordered logs to preverse append/insert operations in case of RAM failures)
- **MemTable** (in-memory Binary Search Tree for sake of complexity)
- **SSTable** (Sorted Strings Table)
- **Index** Trees
- **Compactor** (to clean unused keys flushed from MemTable to SSTable)
- **Bloom Filter** (enhance query/retrieval operation)
![LSM-Tree's Architecture](Attachments/Pasted%20image%2020220922170833.png)
<center>Figure 3. Architecture of LSM-Tree inside Database</center>

## References
[1] https://www.cs.umb.edu/~poneil/lsmtree.pdf

[2] https://github.com/facebook/rocksdb/wiki

[3] https://en.wikipedia.org/wiki/Log-structured_merge-tree

[4] https://medium.com/swlh/log-structured-merge-trees-9c8e2bea89e8
