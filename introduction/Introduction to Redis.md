# Introduction to Redis

Learn about the Redis open source project

Redis is an open source (BSD licensed), in-memory **data structure store** used as a database, cache, message broker, and streaming engine. Redis provides [data structures](https://redis.io/docs/data-types/) such as [strings](https://redis.io/docs/data-types/strings/), [hashes](https://redis.io/docs/data-types/hashes/), [lists](https://redis.io/docs/data-types/lists/), [sets](https://redis.io/docs/data-types/sets/), [sorted sets](https://redis.io/docs/data-types/sorted-sets/) with range queries, [bitmaps](https://redis.io/docs/data-types/bitmaps/), [hyperloglogs](https://redis.io/docs/data-types/hyperloglogs/), [geospatial indexes](https://redis.io/docs/data-types/geospatial/), and [streams](https://redis.io/docs/data-types/streams/). Redis has built-in [replication](https://redis.io/topics/replication), [Lua scripting](https://redis.io/commands/eval), [LRU eviction](https://redis.io/docs/reference/eviction/), [transactions](https://redis.io/topics/transactions), and different levels of [on-disk persistence](https://redis.io/topics/persistence), and provides high availability via [Redis Sentinel](https://redis.io/topics/sentinel) and automatic partitioning with [Redis Cluster](https://redis.io/topics/cluster-tutorial).

You can run **atomic operations** on these types, like [appending to a string](https://redis.io/commands/append); [incrementing the value in a hash](https://redis.io/commands/hincrby); [pushing an element to a list](https://redis.io/commands/lpush); [computing set intersection](https://redis.io/commands/sinter), [union](https://redis.io/commands/sunion) and [difference](https://redis.io/commands/sdiff); or [getting the member with highest ranking in a sorted set](https://redis.io/commands/zrange).

To achieve top performance, Redis works with an **in-memory dataset**. Depending on your use case, Redis can persist your data either by periodically [dumping the dataset to disk](https://redis.io/topics/persistence#snapshotting) or by [appending each command to a disk-based log](https://redis.io/topics/persistence#append-only-file). You can also disable persistence if you just need a feature-rich, networked, in-memory cache.

Redis supports [asynchronous replication](https://redis.io/topics/replication), with fast non-blocking synchronization and auto-reconnection with partial resynchronization on net split.

Redis also includes:

- [Transactions](https://redis.io/topics/transactions)
- [Pub/Sub](https://redis.io/topics/pubsub)
- [Lua scripting](https://redis.io/commands/eval)
- [Keys with a limited time-to-live](https://redis.io/commands/expire)
- [LRU eviction of keys](https://redis.io/docs/reference/eviction)
- [Automatic failover](https://redis.io/topics/sentinel)

You can use Redis from [most programming languages](https://redis.io/clients).

Redis is written in **ANSI C** and works on most POSIX systems like Linux, *BSD, and Mac OS X, without external dependencies. Linux and OS X are the two operating systems where Redis is developed and tested the most, and we **recommend using Linux for deployment**. Redis may work in Solaris-derived systems like SmartOS, but support is *best effort*. There is no official support for Windows builds.

**Refernece**:

​	[Introduction to Redis | Redis](https://redis.io/docs/about/)
