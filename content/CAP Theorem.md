---
title: CAP Theorem
date: 2023-10-06
tags:
  - seed
enableToc: false
---
Most people want three things out of a distributed system:
- **Consistency**: different components have synced state
- **Availability**: if one component goes down, others can immediately fulfill requests
- **Partition Tolerance**: the system works even if the network is partitioned and some nodes can no longer talk to each other

CAP theorem states that you can only have two of these three wants. Let's look at why.
- To make a system consistent and available, you need a large network that constantly shares state, typically by using multiple primary nodes and replicas. In a partition, it wouldn't be possible to keep syncing state between all nodes, so partition tolerance is impossible.
- To make a system consistent and partition tolerant, you would need to bring the network down in the event of a partition in order to maintain consistency. This means the system wouldn't be available.
- To make a system available and partition tolerant, in the event of a partition you'd need to break state between the partitions to keep fulfilling requests. This means when the partition is removed, you must merge or resolve the distinct states, which breaks consistency.

In essence, if there is a network partition we must choose between consistency and availability.

Generally, we can have some combination of the three by sacrificing portions of each of these wants, but this is case by case. For financial use cases, we would prefer sacrificing availability for consistency. For something like online document editing, availability is more important since merging inconsistent states is quite feasible.