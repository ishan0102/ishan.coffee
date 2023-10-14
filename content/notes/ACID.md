---
title: ACID
date: 2023-10-14
tags:
  - seed
enableToc: false
---
## Intro
Let's say you want to write some data to a database. There's a few things you probably want without realizing:
- If you're writing some data and there's a power outage halfway, you want to undo the partially-written data.
- If you write some data, you might want it to match a specific format. For instance, dates might need to be stored as YYYY-MM-DD.
- If two users are reading and writing data simultaneously, their writes should only be visible to other users once complete, not midway.
- If there's a power outage, you want to keep the data safe.

## ACID
These wants may seem obvious, but they're actually non-trivial to guarantee, and make up the core components of ACID transactions. In short, ACID is:
- **Atomicity**: transactions are atomic, they either happy fully or not at all.
- **Consistency**: transactions keep data consistent, they abide by preset constraints.
- **Isolation**: transactions are isolated from one another, simultaneous transactions have no knowledge of one another.
- **Durability**: databases are durable, they ensure data is not lost if a system crashes.

What's hard about this is that to have perfect ACID transactions, you have to sacrifice performance to some extent. All of them hinder perf in some way

### Freshness Consistency

### References
Source: [Atomicity](https://www.youtube.com/watch?v=bwgvaLP7Ucg), [Consistency](https://www.youtube.com/watch?v=IUOmz-KMb7k), [Isolation](https://www.youtube.com/watch?v=mBNucbfl2vM), [Durability](https://www.youtube.com/watch?v=O2otAXjEXTk&list=PL6FzkbJhLW0QM8XObve_mf9OPpPvfHh7t&index=3)