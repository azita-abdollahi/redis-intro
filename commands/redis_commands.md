# Redis Commands

# SET

Syntax

```
SET key value [NX | XX] [GET] [EX seconds | PX milliseconds |
EXAT unix-time-seconds | PXAT unix-time-milliseconds | KEEPTTL]
```

  Set `key` to hold the string `value`. If `key` already holds a value, it is overwritten,         regardless of its type. Any previous time to live associated with the key is discarded on     successful `SET` operation.

  **Options**

  The `SET` command supports a set of options that modify its behavior:

- `EX` *seconds* -- Set the specified expire time, in seconds.

- `PX` *milliseconds* -- Set the specified expire time, in milliseconds.

- `EXAT` *timestamp-seconds* -- Set the specified Unix time at which the key will expire, in seconds.

- `PXAT` *timestamp-milliseconds* -- Set the specified Unix time at which the key will expire, in milliseconds.

- `NX` -- Only set the key if it does not already exist.

- `XX` -- Only set the key if it already exist.

- `KEEPTTL` -- Retain the time to live associated with the key.

- `GET` -- Return the old string stored at key, or nil if key did not exist. An error is returned and `SET` aborted if the value stored at key is not a string.
  
   return  `OK` if `SET` was executed correctly.

#### Examples

```
redis> SET mykey "Hello"
"OK"
```

# DEL

Syntax

```
DEL key [key ...]
```

Removes the specified keys. A key is ignored if it does not exist.

return The number of keys that were removed.

#### Examples

```
redis> SET key1 "Hello"
"OK"
redis> DEL key1
(integer) 1
```

# GET

Syntax

```
GET key
```

Get the value of `key`. If the key does not exist the special value `nil` is returned. An error is returned if the value stored at `key` is not a string, because `GET` only handles string values.

return  the value of `key`, or `nil` when `key` does not exist.

#### Examples

```
redis> GET nonexisting
(nil)
redis> SET mykey "Hello"
"OK"
redis> GET mykey
"Hello"
```

# EXISTS

Syntax

```
EXISTS key [key ...]
```

Returns if `key` exists.

The user should be aware that if the same existing key is mentioned in the arguments multiple times, it will be counted multiple times. So if `somekey` exists, `EXISTS somekey somekey` will return 2.

return specifically the number of keys that exist from those specified as arguments.

#### Examples

```
redis> EXISTS key1
(integer) 1
redis> EXISTS nosuchkey
(integer) 0
```

# DUMP

Syntax

```
DUMP key
```

Serialize the value stored at key in a Redis-specific format and return it to the user. The returned value can be synthesized back into a Redis key using the [`RESTORE`](https://redis.io/commands/restore) command.

Returns the serialized value.

#### Examples

```
> SET mykey 10
OK
> DUMP mykey
"\x00\xc0\n\n\x00n\x9fWE\x0e\xaec\xbb"
```

# EXPIRE

Syntax

```
EXPIRE key seconds [NX | XX | GT | LT]
```

Set a timeout on `key`. After the timeout has expired, the key will automatically be deleted. A key with an associated timeout is often said to be *volatile* in Redis terminology.

 **Options**

The `EXPIRE` command supports a set of options:

- `NX` -- Set expiry only when the key has no expiry
- `XX` -- Set expiry only when the key has an existing expiry
- `GT` -- Set expiry only when the new expiry is greater than current one
- `LT` -- Set expiry only when the new expiry is less than current one

 Returns specifically:

- `1` if the timeout was set.
- `0` if the timeout was not set. e.g. key doesn't exist, or operation skipped due to the provided arguments.

#### Examples

```
redis> SET mykey "Hello"
"OK"
redis> EXPIRE mykey 10
(integer) 1
```

# TTL

Syntax

```
TTL key
```

Returns the remaining time to live of a key that has a timeout. This introspection capability allows a Redis client to check how many seconds a given key will continue to be part of the dataset.

- The command returns `-2` if the key does not exist.
- The command returns `-1` if the key exists but has no associated expire.

Returns TTL in seconds, or a negative value in order to signal an error.

#### Examples

```
redis> SET mykey "Hello"
"OK"
redis> EXPIRE mykey 10
(integer) 1
redis> TTL mykey
(integer) 10
```

# EXPIREAT

Syntax

```
EXPIREAT key unix-time-seconds [NX | XX | GT | LT]
```

`EXPIREAT` has the same effect and semantic as [`EXPIRE`](https://redis.io/commands/expire), but instead of specifying the number of seconds representing the TTL (time to live), it takes an absolute [Unix timestamp](http://en.wikipedia.org/wiki/Unix_time) (seconds since January 1, 1970). A timestamp in the past will delete the key immediately.

 **Options**

The `EXPIREAT` command supports a set of options:

- `NX` -- Set expiry only when the key has no expiry
- `XX` -- Set expiry only when the key has an existing expiry
- `GT` -- Set expiry only when the new expiry is greater than current one
- `LT` -- Set expiry only when the new expiry is less than current one

Returns 

- `1` if the timeout was set.
- `0` if the timeout was not set. e.g. key doesn't exist, or operation skipped due to the provided arguments.

#### Examples

```
redis> SET mykey "Hello"
"OK"
redis> EXISTS mykey
(integer) 1
redis> EXPIREAT mykey 1293840000
(integer) 1
redis> EXISTS mykey
(integer) 0
```

# PEXPIRE

Syntax

```
PEXPIRE key milliseconds [NX | XX | GT | LT]
```

This command works exactly like [`EXPIRE`](https://redis.io/commands/expire) but the time to live of the key is specified in milliseconds instead of seconds.

 **Options**

- `NX` -- Set expiry only when the key has no expiry
- `XX` -- Set expiry only when the key has an existing expiry
- `GT` -- Set expiry only when the new expiry is greater than current one
- `LT` -- Set expiry only when the new expiry is less than current one

Returns  specifically:

- `1` if the timeout was set.
- `0` if the timeout was not set. e.g. key doesn't exist, or operation skipped due to the provided arguments.

#### Examples

```
redis> SET mykey "Hello"
"OK"
redis> PEXPIRE mykey 1500
(integer) 1
```

# PTTL

Syntax

```
PTTL key
```

Like [`TTL`](https://redis.io/commands/ttl) this command returns the remaining time to live of a key that has an expire set, with the sole difference that [`TTL`](https://redis.io/commands/ttl) returns the amount of remaining time in seconds while `PTTL` returns it in milliseconds.

- The command returns `-2` if the key does not exist.
- The command returns `-1` if the key exists but has no associated expire.

Returns TTL in milliseconds, or a negative value in order to signal an error.

#### Examples

```
redis> SET mykey "Hello"
"OK"
redis> EXPIRE mykey 1
(integer) 1
redis> PTTL mykey
(integer) 1000
```

# PEXPIREAT

Syntax

```
PEXPIREAT key unix-time-milliseconds [NX | XX | GT | LT]
```

`PEXPIREAT` has the same effect and semantic as [`EXPIREAT`](https://redis.io/commands/expireat), but the Unix time at which the key will expire is specified in milliseconds instead of seconds.

 **Options**

- `NX` -- Set expiry only when the key has no expiry
- `XX` -- Set expiry only when the key has an existing expiry
- `GT` -- Set expiry only when the new expiry is greater than current one
- `LT` -- Set expiry only when the new expiry is less than current one

Returns specifically:

- `1` if the timeout was set.
- `0` if the timeout was not set. e.g. key doesn't exist, or operation skipped due to the provided arguments.

#### Examples

```
redis> SET mykey "Hello"
"OK"
redis> PEXPIREAT mykey 1555555555005
(integer) 1
```

# SELECT

Syntax

```
SELECT index
```

Select the Redis logical database having the specified zero-based numeric index. New connections always use the database 0.(Redis has 16 databases)

Selectable Redis databases are a form of namespacing: all databases are still persisted in the same RDB / AOF file. However different databases can have keys with the same name, and commands like [`FLUSHDB`](https://redis.io/commands/flushdb), [`SWAPDB`](https://redis.io/commands/swapdb) or [`RANDOMKEY`](https://redis.io/commands/randomkey) work on specific databases.

In practical terms, Redis databases should be used to separate different keys belonging to the same application (if needed), and not to use a single Redis instance for multiple unrelated applications.

When using Redis Cluster, the `SELECT` command cannot be used, since Redis Cluster only supports database zero. In the case of a Redis Cluster, having multiple databases would be useless and an unnecessary source of complexity. Commands operating atomically on a single database would not be possible with the Redis Cluster design and goals.

# MOVE

Syntax

```
MOVE key db
```

Move `key` from the currently selected database to the specified destination database. When `key` already exists in the destination database, or it does not exist in the source database, it does nothing. It is possible to use `MOVE` as a locking primitive because of this.

Return specifically:

- `1` if `key` was moved.
- `0` if `key` was not moved.

# FLUSHDB

Syntax

```
FLUSHDB [ASYNC | SYNC]
```

Delete all the keys of the currently selected DB. This command never fails.

By default, `FLUSHDB` will synchronously flush all keys from the database. Starting with Redis 6.2, setting the **lazyfree-lazy-user-flush** configuration directive to "yes" changes the default flush mode to asynchronous.

It is possible to use one of the following modifiers to dictate the flushing mode explicitly:

- `ASYNC`: flushes the database asynchronously
- `SYNC`: flushes the database synchronously

Note: an asynchronous `FLUSHDB` command only deletes keys that were present at the time the command was invoked. Keys created during an asynchronous flush will be unaffected.

# FLUSHALL

Syntax

```
FLUSHALL [ASYNC | SYNC]
```

Delete all the keys of all the existing databases, not just the currently selected one. This command never fails.

By default, `FLUSHALL` will synchronously flush all the databases. Starting with Redis 6.2, setting the **lazyfree-lazy-user-flush** configuration directive to "yes" changes the default flush mode to asynchronous.

It is possible to use one of the following modifiers to dictate the flushing mode explicitly:

- `ASYNC`: flushes the databases asynchronously
- `SYNC`: flushes the databases synchronously

Note: an asynchronous `FLUSHALL` command only deletes keys that were present at the time the command was invoked. Keys created during an asynchronous flush will be unaffected.

# MSET

Syntax

```
MSET key value [key value ...]
```

Sets the given keys to their respective values. `MSET` replaces existing values with new values, just as regular [`SET`](https://redis.io/commands/set). See [`MSETNX`](https://redis.io/commands/msetnx) if you don't want to overwrite existing values.

`MSET` is atomic, so all given keys are set at once. It is not possible for clients to see that some of the keys were updated while others are unchanged.

Return always `OK` since `MSET` can't fail.

#### Examples

```
redis> MSET key1 "Hello" key2 "World"
"OK"
```

# MGET

Syntax

```
MGET key [key ...]
```

Returns the values of all specified keys. For every key that does not hold a string value or does not exist, the special value `nil` is returned. Because of this, the operation never fails.

Returns list of values at the specified keys.

#### Examples

```
redis> MGET key1 key2 nonexisting
1) "Hello"
2) "World"
3) (nil)
```

# KEYS

Syntax

```
KEYS pattern
```

Returns all keys matching `pattern`.

While the time complexity for this operation is O(N), the constant times are fairly low. For example, Redis running on an entry level laptop can scan a 1 million key database in 40 milliseconds.

**Warning**: consider `KEYS` as a command that should only be used in production environments with extreme care. It may ruin performance when it is executed against large databases. This command is intended for debugging and special operations, such as changing your keyspace layout. Don't use `KEYS` in your regular application code. If you're looking for a way to find keys in a subset of your keyspace, consider using [`SCAN`](https://redis.io/commands/scan) or [sets](https://redis.io/topics/data-types#sets).

Supported glob-style patterns:

- `h?llo` matches `hello`, `hallo` and `hxllo`
- `h*llo` matches `hllo` and `heeeello`
- `h[ae]llo` matches `hello` and `hallo,` but not `hillo`
- `h[^e]llo` matches `hallo`, `hbllo`, ... but not `hello`
- `h[a-b]llo` matches `hallo` and `hbllo`

Use `\` to escape special characters if you want to match them verbatim.

Returns list of keys matching `pattern`.

#### Examples

```
redis> MSET firstname Jack lastname Stuntman age 35
"OK"
redis> KEYS *name*
1) "firstname"
2) "lastname"
redis> KEYS a??
1) "age"
redis> KEYS *
1) "firstname"
2) "age"
3) "lastname"
```
