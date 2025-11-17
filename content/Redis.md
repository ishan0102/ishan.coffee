---
title: Redis
date: 2023-10-04
tags:
  - seed
enableToc: false
---
Redis is an in-memory key-value store and can be used as a cache, message queue, database, and pretty much anything else you could use a map for. It's great as a cache since it's in-memory, meaning writes are quick. The tradeoff to this is a lack of persistence, but that can easily be corrected with disk snapshots or the append-only file setting.