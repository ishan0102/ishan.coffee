---
title: ACID
date: 2023-10-14
tags:
  - seed
enableToc: true
---
Let's say you want to write some data to a database. There's a few things you probably want without realizing:
- **Atomicity:** If you're writing some data and there's a power outage halfway, you want to undo the partially-written data. Transactions are atomic, meaning they either happen fully or not at all.
- **Consistency**: You might want data to match a specific format, like ensuring dates are stored in the `YYYY-MM-DD` format. Transactions ensure data remains consistent and adhere to preset constraints.
- **Isolation**: If two users are reading and writing data simultaneously, their changes should only be visible to other users once complete and not midway through. Transactions are isolated from each other, ensuring that simultaneous operations do not interfere with one another.
- **Durability**:  In the event of power outages or system failures, the last thing you'd want is data loss. Databases ensure durability, guaranteeing that committed data remains safe even if the system crashes.

Guaranteeing any of these will hinder performance in some way. For instance, guaranteeing isolation implies that you must lock the database while performing writes, which hurts parallelization. However, most modern databases like Postgres are ACID compliant because building reliable systems often requires performance tradeoffs.

### Freshness Consistency
One other form of consistency I didn't touch on above is **freshness consistency**. This involves 

### References
Studying with Alex: [Atomicity](https://www.youtube.com/watch?v=bwgvaLP7Ucg), [Consistency](https://www.youtube.com/watch?v=IUOmz-KMb7k), [Isolation](https://www.youtube.com/watch?v=mBNucbfl2vM), [Durability](https://www.youtube.com/watch?v=O2otAXjEXTk&list=PL6FzkbJhLW0QM8XObve_mf9OPpPvfHh7t&index=3)