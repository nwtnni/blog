+++
title = "Black-box Concurrent Data Structures for NUMA Architectures"
date = 2024-01-12
description = "Irina Calciu et al., ASPLOS 2017"
+++

[This paper](https://doi.org/10.1145/3037697.3037721) [1]
recognizes how NUMA architectures are similar to
distributed systems--namely, the high cost of synchronization
between nodes/hosts, respectively--and essentially applies
state machine replication to in-memory data structures.
The primary contribution is a NUMA-aware shared log.

<!-- more -->

The main technique for minimizing contention is flat
combining, where a leader is elected per node and takes responsibility
for propagating writes from each thread to the shared log, so that
only a few cores at a time (equal to the number of NUMA nodes)
need to acquire cache lines. The rest of the design seems focused
on allowing maximum parallelism between readers and writers.

The implementation is *not* tolerant to partial faults, as combiner
threads need to wait for preceding log entries to be filled before
the local replica can be updated, in order to ensure linearizability.
Some distributed systems (Tango?) work around this through eager
hole-filling, although I don't have a solid understanding of this technique.

Log entries can be recycled when all local replicas have read up
to a certain point. This might also not be fault tolerant: if only
partial failures are considered, then it's possible for a live node to send
a copy of its local replica to a recovering node, but if whole system
failures are possible, then checkpoints must be written somewhere.

The paper mentions that multiple data structures can be treated as
a single sequential data structure and therefore have transactional
semantics under node replication. If an entire node or host crashes,
then this should be fine, since the local replica becomes inaccessible
and would be recreated on recovery, and an incomplete update is never
visible. But if only a subset of threads on the node crash while updating
the replica, then I think detectable recoverability is required?

Unfortunately, the evaluation doesn't seem to show the memory access
characteristics across NUMA nodes, so it's hard to estimate what the
effect would be like with CXL latency/throughput numbers. Have NUMA
latencies changed since 2017? I also thought it was interesting that
the NUMA-aware stack had such excellent performance.

One downside to this technique is that the memory usage scales linearly
with the number of nodes. But that seems unavoidable if trying to
minimize cross-node memory accesses, and the number of nodes should
generally be small (< 10?).

[1] Irina Calciu, Siddhartha Sen, Mahesh Balakrishnan, and Marcos K. Aguilera. 2017. Black-box Concurrent Data Structures for NUMA Architectures. In Proceedings of the Twenty-Second International Conference on Architectural Support for Programming Languages and Operating Systems (ASPLOS '17). Association for Computing Machinery, New York, NY, USA, 207–221. https://doi.org/10.1145/3037697.3037721
