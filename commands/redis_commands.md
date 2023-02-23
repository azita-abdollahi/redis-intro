## Redis Commands

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
redis> SET key EX 80 NX
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

# SETBIT

Syntax

```
SETBIT key offset value
```

Sets or clears the bit at *offset* in the string value stored at *key*.

The bit is either set or cleared depending on *value*, which can be either 0 or 1.

When *key* does not exist, a new string value is created.

Returns  the original bit value stored at *offset*.

#### Examples

```
redis> SETBIT mykey 7 1
(integer) 0
```

# GETBIT

Syntax

```
GETBIT key offset
```

Returns the bit value at *offset* in the string value stored at *key*.

When *offset* is beyond the string length, the string is assumed to be a contiguous space with 0 bits. When *key* does not exist it is assumed to be an empty string, so *offset* is always out of range and the value is also assumed to be a contiguous space with 0 bits.

Returns the bit value stored at *offset*.

#### Examples

```
redis> SETBIT mykey 7 1
(integer) 0
redis> GETBIT mykey 7
(integer) 1
```

# GETRANGE

Syntax

```
GETRANGE key start end
```

Returns the substring of the string value stored at `key`, determined by the offsets `start` and `end` (both are inclusive). Negative offsets can be used in order to provide an offset starting from the end of the string. So -1 means the last character, -2 the penultimate and so forth.

#### Examples

```
redis> SET mykey "This is a string"
"OK"
redis> GETRANGE mykey 0 3
"This"
```

# SETRANGE

Syntax

```
SETRANGE key offset value
```

Overwrites part of the string stored at *key*, starting at the specified offset, for the entire length of *value*. If the offset is larger than the current length of the string at *key*, the string is padded with zero-bytes to make *offset* fit. Non-existing keys are considered as empty strings, so this command will make sure it holds a string large enough to be able to set *value* at *offset*.

Returns the length of the string after it was modified by the command.

#### Examples

```
redis> SET key1 "Hello World"
"OK"
redis> SETRANGE key1 6 "Redis"
(integer) 11

Example of zero padding:
redis> SETRANGE key2 6 "Redis"
(integer) 11
redis> GET key2
"  Redis"
```

# STRLEN

Syntax

```
STRLEN key
```

Returns the length of the string value stored at `key`. An error is returned when `key` holds a non-string value.

Returns  the length of the string at `key`, or `0` when `key` does not exist.

#### Examples

```
redis> SET mykey "Hello world"
"OK"
redis> STRLEN mykey
(integer) 11
```

# APPEND

Syntax

```
APPEND key value
```

If `key` already exists and is a string, this command appends the `value` at the end of the string. If `key` does not exist it is created and set as an empty string, so `APPEND` will be similar to [`SET`](https://redis.io/commands/set) in this special case.

Returns the length of the string after the append operation.

#### Examples

```
redis> EXISTS mykey
(integer) 0
redis> APPEND mykey "Hello"
(integer) 5
```

# PERSIST

Syntax

```
PERSIST key
```

Remove the existing timeout on `key`, turning the key from *volatile* (a key with an expire set) to *persistent* (a key that will never expire as no timeout is associated).

Returns specifically:

- `1` if the timeout was removed.
- `0` if `key` does not exist or does not have an associated timeout.

#### Examples

```
redis> SET mykey "Hello"
"OK"
redis> EXPIRE mykey 10
(integer) 1
redis> TTL mykey
(integer) 10
redis> PERSIST mykey
(integer) 1
redis> TTL mykey
(integer) -1
```

# RENAME

Syntax

```
RENAME key newkey
```

Renames `key` to `newkey`. It returns an error when `key` does not exist. If `newkey` already exists it is overwritten, when this happens `RENAME` executes an implicit [`DEL`](https://redis.io/commands/del) operation, so if the deleted key contains a very big value it may cause high latency even if `RENAME` itself is usually a constant-time operation.

In Cluster mode, both `key` and `newkey` must be in the same **hash slot**, meaning that in practice only keys that have the same hash tag can be reliably renamed in cluster.

#### Examples

```
redis> SET mykey "Hello"
"OK"
redis> RENAME mykey myotherkey
"OK"
```

# RENAMENX

Syntax

```
RENAMENX key newkey
```

Renames `key` to `newkey` if `newkey` does not yet exist. It returns an error when `key` does not exist.

In Cluster mode, both `key` and `newkey` must be in the same **hash slot**, meaning that in practice only keys that have the same hash tag can be reliably renamed in cluster.

Returns specifically:

- `1` if `key` was renamed to `newkey`.
- `0` if `newkey` already exists.

#### Examples

```
redis> SET mykey "Hello"
"OK"
redis> SET myotherkey "World"
"OK"
redis> RENAMENX mykey myotherkey
(integer) 0
```

# RANDOMKEY

Syntax

```
RANDOMKEY
```

Return a random key from the currently selected database.

Returns the random key, or `nil` when the database is empty.

# TYPE

Syntax

```
TYPE key
```

Returns the string representation of the type of the value stored at `key`. The different types that can be returned are: `string`, `list`, `set`, `zset`, `hash` and `stream`.

Returns type of `key`, or `none` when `key` does not exist.

#### Examples

```
redis> SET key1 "value"
"OK"
redis> TYPE key1
"string"
```

# RESTORE

Syntax

```
RESTORE key ttl serialized-value [REPLACE] [ABSTTL]
  [IDLETIME seconds] [FREQ frequency]
```

Create a key associated with a value that is obtained by deserializing the provided serialized value (obtained via [`DUMP`](https://redis.io/commands/dump)).

If `ttl` is 0 the key is created without any expire, otherwise the specified expire time (in milliseconds) is set.

If the `ABSTTL` modifier was used, `ttl` should represent an absolute [Unix timestamp](http://en.wikipedia.org/wiki/Unix_time) (in milliseconds) in which the key will expire.

For eviction purposes, you may use the `IDLETIME` or `FREQ` modifiers. See [`OBJECT`](https://redis.io/commands/object) for more information.

`RESTORE` will return a "Target key name is busy" error when `key` already exists unless you use the `REPLACE` modifier.

`RESTORE` checks the RDB version and data checksum. If they don't match an error is returned.

 Returns The command returns OK on success.

#### Examples

```
redis> DEL mykey
0
redis> RESTORE mykey 0 "\n\x17\x17\x00\x00\x00\x12\x00\x00\x00\x03\x00\
                        x00\xc0\x01\x00\x04\xc0\x02\x00\x04\xc0\x03\x00\
                        xff\x04\x00u#<\xc0;.\xe9\xdd"
OK
```

# OBJECT

Syntax

```
OBJECT
```

This is a container command for object introspection commands.

# OBJECT ENCODING

Syntax

```
OBJECT ENCODING key
```

Returns the internal encoding for the Redis object stored at `<key>`

Redis objects can be encoded in different ways:

- Strings can be encoded as:
  - `raw`, normal string encoding.
  - `int`, strings representing integers in a 64-bit signed interval, encoded in this way to save space.
  - `embstr`, an embedded string, which is an object where the internal simple dynamic string, `sds`, is an unmodifiable string allocated in the same chuck as the object itself. `embstr` can be strings with lengths up to the hardcoded limit of `OBJ_ENCODING_EMBSTR_SIZE_LIMIT` or 44 bytes.
- Lists can be encoded as `ziplist` or `linkedlist`. The `ziplist` is the special representation that is used to save space for small lists.
- Sets can be encoded as `intset` or `hashtable`. The `intset` is a special encoding used for small sets composed solely of integers.
- Hashes can be encoded as `ziplist` or `hashtable`. The `ziplist` is a special encoding used for small hashes.
- Sorted Sets can be encoded as `ziplist` or `skiplist` format. As for the List type small sorted sets can be specially encoded using `ziplist`, while the `skiplist` encoding is the one that works with sorted sets of any size.

All the specially encoded types are automatically converted to the general type once you perform an operation that makes it impossible for Redis to retain the space saving encoding.

Returns the encoding of the object, or `nil` if the key doesn't exist.

# OBJECT REFCOUNT

Syntax

```
OBJECT REFCOUNT key
```

This command returns the reference count of the stored at `<key>`.

Returns The number of references.

# OBJECT IDLETIME

Syntax

```
OBJECT IDLETIME key
```

This command returns the time in seconds since the last access to the value stored at `<key>`.

The command is only available when the `maxmemory-policy` configuration directive is not set to one of the LFU policies.

Returns The idle time in seconds.

# SCAN

Syntax

```
SCAN cursor [MATCH pattern] [COUNT count] [TYPE type]
```

The `SCAN` command and the closely related commands [`SSCAN`](https://redis.io/commands/sscan), [`HSCAN`](https://redis.io/commands/hscan) and [`ZSCAN`](https://redis.io/commands/zscan) are used in order to incrementally iterate over a collection of elements.

- `SCAN` iterates the set of keys in the currently selected Redis database.
- [`SSCAN`](https://redis.io/commands/sscan) iterates elements of Sets types.
- [`HSCAN`](https://redis.io/commands/hscan) iterates fields of Hash types and their associated values.
- [`ZSCAN`](https://redis.io/commands/zscan) iterates elements of Sorted Set types and their associated scores.

## The COUNT option

While `SCAN` does not provide guarantees about the number of elements returned at every iteration, it is possible to empirically adjust the behavior of `SCAN` using the **COUNT** option. Basically with COUNT the user specified the *amount of work that should be done at every call in order to retrieve elements from the collection*. This is **just a hint** for the implementation, however generally speaking this is what you could expect most of the times from the implementation.

- The default COUNT value is 10.
- When iterating the key space, or a Set, Hash or Sorted Set that is big enough to be represented by a hash table, assuming no **MATCH** option is used, the server will usually return *count* or a bit more than *count* elements per call. Please check the *why SCAN may return all the elements at once* section later in this document.
- When iterating Sets encoded as intsets (small sets composed of just integers), or Hashes and Sorted Sets encoded as ziplists (small hashes and sets composed of small individual values), usually all the elements are returned in the first `SCAN` call regardless of the COUNT value.

Important: **there is no need to use the same COUNT value** for every iteration. The caller is free to change the count from one iteration to the other as required, as long as the cursor passed in the next call is the one obtained in the previous call to the command.

## The MATCH option

It is possible to only iterate elements matching a given glob-style pattern, similarly to the behavior of the [`KEYS`](https://redis.io/commands/keys) command that takes a pattern as its only argument.

To do so, just append the `MATCH <pattern>` arguments at the end of the `SCAN` command (it works with all the SCAN family commands).

It is important to note that the **MATCH** filter is applied after elements are retrieved from the collection, just before returning data to the client. This means that if the pattern matches very little elements inside the collection, `SCAN` will likely return no elements in most iterations. 

## The TYPE option

You can use the `TYPE` option to ask `SCAN` to only return objects that match a given `type`, allowing you to iterate through the database looking for keys of a specific type. The **TYPE** option is only available on the whole-database `SCAN`, not [`HSCAN`](https://redis.io/commands/hscan) or [`ZSCAN`](https://redis.io/commands/zscan) etc.

#### Examples

```
redis 127.0.0.1:6379> scan 0
1) "17"
2)  1) "key:12"
    2) "key:8"
    3) "key:4"
example of iteration using MATCH, COUNT, TYPE:
redis 127.0.0.1:6379> sadd myset 1 2 3 foo foobar feelsgood
(integer) 6
redis 127.0.0.1:6379> sscan myset 0 match f*
1) "0"
2) 1) "foo"
   2) "feelsgood"
   3) "foobar"
redis 127.0.0.1:6379> scan 0 MATCH *11*
1) "288"
2) 1) "key:911"
redis 127.0.0.1:6379> scan 288 MATCH *11*
1) "224"
2) (empty list or set)
redis 127.0.0.1:6379> scan 224 MATCH *11*
1) "80"
2) (empty list or set)
redis 127.0.0.1:6379> scan 80 MATCH *11*
1) "176"
2) (empty list or set)
redis 127.0.0.1:6379> scan 176 MATCH *11* COUNT 1000
1) "0"
2)  1) "key:611"
    2) "key:711"
redis 127.0.0.1:6379> SCAN 0 TYPE zset
1) "0"
2) 1) "geokey"
   2) "zkey"
```

# SADD

Syntax

```
SADD key member [member ...]
```

Add the specified members to the set stored at `key`. Specified members that are already a member of this set are ignored. If `key` does not exist, a new set is created before adding the specified members.

An error is returned when the value stored at `key` is not a set.

Returns the number of elements that were added to the set, not including all the elements already present in the set.

#### Examples

```
redis> SADD myset "Hello"
(integer) 1
```

# SMEMBERS

Syntax

```
SMEMBERS key
```

Returns all the members of the set value stored at `key`.

This has the same effect as running [`SINTER`](https://redis.io/commands/sinter) with one argument `key`.

Returns all elements of the set.

#### Examples

```
redis> SADD myset "Hello"
(integer) 1
redis> SADD myset "World"
(integer) 1
redis> SMEMBERS myset
1) "Hello"
2) "World"
```

# SINTER

Syntax

```
SINTER key [key ...]
```

Returns the members of the set resulting from the intersection of all the given sets.

For example:

```
key1 = {a,b,c,d}
key2 = {c}
key3 = {a,c,e}
SINTER key1 key2 key3 = {c}
```

Keys that do not exist are considered to be empty sets. With one of the keys being an empty set, the resulting set is also empty (since set intersection with an empty set always results in an empty set).

Returns list with members of the resulting set.

#### Examples

```
redis> SADD key1 "a" "b" "c"
(integer) 1
redis> SADD key2 "c" "d" "e"
(integer) 1
redis> SINTER key1 key2
1) "c"
```

# SDIFF

Syntax

```
SDIFF key [key ...]
```

Returns the members of the set resulting from the difference between the first set and all the successive sets.

For example:

```
key1 = {a,b,c,d}
key2 = {c}
key3 = {a,c,e}
SDIFF key1 key2 key3 = {b,d}
```

Keys that do not exist are considered to be empty sets.

 Returns list with members of the resulting set.

#### Examples

```
redis> SADD key1 "a" "b" "c"
(integer) 1
redis> SADD key2 "c" "d" "e"
(integer) 1
redis> SDIFF key1 key2
1) "b"
2) "a"
```

# SORT

Syntax

```
SORT key [BY pattern] [LIMIT offset count] [GET pattern [GET pattern
  ...]] [ASC | DESC] [ALPHA] [STORE destination]
```

Returns or stores the elements contained in the [list](https://redis.io/topics/data-types#lists), [set](https://redis.io/topics/data-types#set) or [sorted set](https://redis.io/topics/data-types#sorted-sets) at `key`.

There is also the [`SORT_RO`](https://redis.io/commands/sort_ro) read-only variant of this command.

By default, sorting is numeric and elements are compared by their value interpreted as double precision floating point number. This is `SORT` in its simplest form:

```
SORT mylist
```

Assuming `mylist` is a list of numbers, this command will return the same list with the elements sorted from small to large. In order to sort the numbers from large to small, use the `DESC` modifier:

```
SORT mylist DESC
```

When `mylist` contains string values and you want to sort them lexicographically, use the `ALPHA` modifier:

```
SORT mylist ALPHA
```

The number of returned elements can be limited using the `LIMIT` modifier. This modifier takes the `offset` argument, specifying the number of elements to skip and the `count` argument, specifying the number of elements to return from starting at `offset`. The following example will return 10 elements of the sorted version of `mylist`, starting at element 0 (`offset` is zero-based):

```
SORT mylist LIMIT 0 10
```

Almost all modifiers can be used together. The following example will return the first 5 elements, lexicographically sorted in descending order:

```
SORT mylist LIMIT 0 5 ALPHA DESC
```

## Sorting by external keys

Sometimes you want to sort elements using external keys as weights to compare instead of comparing the actual elements in the list, set or sorted set. Let's say the list `mylist` contains the elements `1`, `2` and `3` representing unique IDs of objects stored in `object_1`, `object_2` and `object_3`. When these objects have associated weights stored in `weight_1`, `weight_2` and `weight_3`, `SORT` can be instructed to use these weights to sort `mylist` with the following statement:

```
SORT mylist BY weight_*
```

The `BY` option takes a pattern (equal to `weight_*` in this example) that is used to generate the keys that are used for sorting. These key names are obtained substituting the first occurrence of `*` with the actual value of the element in the list (`1`, `2` and `3` in this example).

## Skip sorting the elements

The `BY` option can also take a non-existent key, which causes `SORT` to skip the sorting operation. This is useful if you want to retrieve external keys (see the `GET` option below) without the overhead of sorting.

```
SORT mylist BY nosort
```

# INCR

Syntax

```
INCR key
```

Increments the number stored at `key` by one. If the key does not exist, it is set to `0` before performing the operation. An error is returned if the key contains a value of the wrong type or contains a string that can not be represented as integer. This operation is limited to 64 bit signed integers.

Returns the value of `key` after the increment

#### Examples

```
redis> SET mykey "10"
"OK"
redis> INCR mykey
(integer) 11
```

# DECR

Syntax

```
DECR key
```

Decrements the number stored at `key` by one. If the key does not exist, it is set to `0` before performing the operation. An error is returned if the key contains a value of the wrong type or contains a string that can not be represented as integer. This operation is limited to **64 bit signed integers**.

Returns the value of `key` after the decrement

#### Examples

```
redis> SET mykey "10"
"OK"
redis> DECR mykey
(integer) 9
```

# INCRBY

Syntax

```
INCRBY key increment
```

Increments the number stored at `key` by `increment`. If the key does not exist, it is set to `0` before performing the operation. An error is returned if the key contains a value of the wrong type or contains a string that can not be represented as integer. This operation is limited to 64 bit signed integers.

Returns  the value of `key` after the increment

#### Examples

```
redis> SET mykey "10"
"OK"
redis> INCRBY mykey 5
(integer) 15
```

# DECRBY

Syntax

```
DECRBY key decrement
```

Decrements the number stored at `key` by `decrement`. If the key does not exist, it is set to `0` before performing the operation. An error is returned if the key contains a value of the wrong type or contains a string that can not be represented as integer. This operation is limited to 64 bit signed integers.

 the value of `key` after the decrement

#### Examples

```
redis> SET mykey "10"
"OK"
redis> DECRBY mykey 3
(integer) 7
```

# INCRBYFLOAT

Syntax

```
INCRBYFLOAT key increment
```

Increment the string representing a floating point number stored at `key` by the specified `increment`. By using a negative `increment` value, the result is that the value stored at the key is decremented (by the obvious properties of addition). If the key does not exist, it is set to `0` before performing the operation. An error is returned if one of the following conditions occur:

- The key contains a value of the wrong type (not a string).
- The current key content or the specified increment are not parsable as a double precision floating point number.

Returns the value of `key` after the increment.

#### Examples

```
redis> SET mykey 10.50
"OK"
redis> INCRBYFLOAT mykey 0.1
"10.6"
```

# HSET

Syntax

```
HSET key field value [field value ...]
```

Sets the specified fields to their respective values in the hash stored at `key`.

This command overwrites the values of specified fields that exist in the hash. If `key` doesn't exist, a new key holding a hash is created.

Returns The number of fields that were added.

#### Examples

```
redis> HSET myhash field1 "Hello"
(integer) 1
```

# HGET

Syntax

```
HGET key field
```

Returns the value associated with `field` in the hash stored at `key`.

Returns  the value associated with `field`, or `nil` when `field` is not present in the hash or `key` does not exist.

#### Examples

```
redis> HSET myhash field1 "foo"
(integer) 1
redis> HGET myhash field1
"foo"
redis> HGET myhash field2
(nil)
```

# HGETALL

Syntax

```
HGETALL key
```

Returns all fields and values of the hash stored at `key`. In the returned value, every field name is followed by its value, so the length of the reply is twice the size of the hash.

Returns list of fields and their values stored in the hash, or an empty list when `key` does not exist.

#### Examples

```
redis> HSET myhash field1 "Hello"
(integer) 1
redis> HSET myhash field2 "World"
(integer) 1
redis> HGETALL myhash
1) "field1"
2) "Hello"
3) "field2"
4) "World"
```

# HDEL

Syntax

```
HDEL key field [field ...]
```

Removes the specified fields from the hash stored at `key`. Specified fields that do not exist within this hash are ignored. If `key` does not exist, it is treated as an empty hash and this command returns `0`.

Returns the number of fields that were removed from the hash, not including specified but non existing fields.

#### Examples

```
redis> HSET myhash field1 "foo"
(integer) 1
redis> HDEL myhash field1
(integer) 1
redis> HDEL myhash field2
(integer) 0
```

# HEXISTS

Syntax

```
HEXISTS key field
```

Returns if `field` is an existing field in the hash stored at `key`.

Returns specifically:

- `1` if the hash contains `field`.
- `0` if the hash does not contain `field`, or `key` does not exist.

#### Examples

```
redis> HSET myhash field1 "foo"
(integer) 1
redis> HEXISTS myhash field1
(integer) 1
redis> HEXISTS myhash field2
(integer) 0
```

# HMGET

Syntax

```
HMGET key field [field ...]
```

Returns the values associated with the specified `fields` in the hash stored at `key`.

For every `field` that does not exist in the hash, a `nil` value is returned. Because non-existing keys are treated as empty hashes, running `HMGET` against a non-existing `key` will return a list of `nil` values.

returns list of values associated with the given fields, in the same order as they are requested.

#### Examples

```
redis> HSET myhash field1 "Hello" field2 "World"
(integer) 1
redis> HMGET myhash field1 field2 nofield
1) "Hello"
2) "World"
3) (nil)
```

# HSCAN

Syntax

```
HSCAN key cursor [MATCH pattern] [COUNT count]
```

iterates fields of Hash types and their associated values.

# HINCRBY

Syntax

```
HINCRBY key field increment
```

Increments the number stored at `field` in the hash stored at `key` by `increment`. If `key` does not exist, a new key holding a hash is created. If `field` does not exist the value is set to `0` before the operation is performed.

The range of values supported by `HINCRBY` is limited to 64 bit signed integers.

Returns the value at `field` after the increment operation.

#### Examples

Since the `increment` argument is signed, both increment and decrement operations can be performed:

```
redis> HSET myhash field 5
(integer) 1
redis> HINCRBY myhash field 1
(integer) 6
redis> HINCRBY myhash field -1
(integer) 5
```

# HINCRBYFLOAT

Syntax

```
HINCRBYFLOAT key field increment
```

Increment the specified `field` of a hash stored at `key`, and representing a floating point number, by the specified `increment`. If the increment value is negative, the result is to have the hash field value **decremented** instead of incremented. If the field does not exist, it is set to `0` before performing the operation. An error is returned if one of the following conditions occur:

- The field contains a value of the wrong type (not a string).
- The current field content or the specified increment are not parsable as a double precision floating point number.

Returns the value of `field` after the increment.

#### Examples

```
redis> HSET mykey field 10.50
(integer) 1
redis> HINCRBYFLOAT mykey field 0.1
"10.6"
redis> HINCRBYFLOAT mykey field -5
"5.6"
redis> HSET mykey field 5.0e3
(integer) 0
redis> HINCRBYFLOAT mykey field 2.0e2
"5200"
```

# HKEYS

Syntax

```
HKEYS key
```

Returns all field names in the hash stored at `key`.

Returns list of fields in the hash, or an empty list when `key` does not exist.

#### Examples

```
redis> HSET myhash field1 "Hello" field2 "World"
(integer) 1
redis> HKEYS myhash
1) "field1"
2) "field2"
```

# HLEN

Syntax

```
HLEN key
```

Returns the number of fields contained in the hash stored at `key`.

Returns number of fields in the hash, or `0` when `key` does not exist.

#### Examples

```
redis> HSET myhash field1 "Hello"
(integer) 1
redis> HSET myhash field2 "World"
(integer) 1
redis> HLEN myhash
(integer) 2
```

# HSETNX

Syntax

```
HSETNX key field value
```

Sets `field` in the hash stored at `key` to `value`, only if `field` does not yet exist. If `key` does not exist, a new key holding a hash is created. If `field` already exists, this operation has no effect.

Returns specifically:

- `1` if `field` is a new field in the hash and `value` was set.
- `0` if `field` already exists in the hash and no operation was performed.

#### Examples

```
redis> HSETNX myhash field "Hello"
(integer) 1
redis> HSETNX myhash field "World"
(integer) 0
redis> HGET myhash field
"Hello"
```

# HVALS

Syntax

```
HVALS key
```

Returns all values in the hash stored at `key`.

Returns  list of values in the hash, or an empty list when `key` does not exist.

#### Examples

```
redis> HSET myhash field1 "Hello"
(integer) 1
redis> HSET myhash field2 "World"
(integer) 1
redis> HVALS myhash
1) "Hello"
2) "World"
```

# LPUSH

Syntax

```
LPUSH key element [element ...]
```

Insert all the specified values at the head of the list stored at `key`. If `key` does not exist, it is created as empty list before performing the push operations. When `key` holds a value that is not a list, an error is returned.

It is possible to push multiple elements using a single command call just specifying multiple arguments at the end of the command. Elements are inserted one after the other to the head of the list, from the leftmost element to the rightmost element. So for instance the command `LPUSH mylist a b c` will result into a list containing `c` as first element, `b` as second element and `a` as third element.

Returns the length of the list after the push operations.

#### Examples

```
redis> LPUSH mylist "world"
(integer) 1
redis> LPUSH mylist "hello"
(integer) 2
redis> LRANGE mylist 0 -1 #all elements in myhash
1) "hello"
2) "world"
```

# LRANGE

Syntax

```
LRANGE key start stop
```

Returns the specified elements of the list stored at `key`. The offsets `start` and `stop` are zero-based indexes, with `0` being the first element of the list (the head of the list), `1` being the next element and so on.

These offsets can also be negative numbers indicating offsets starting at the end of the list. For example, `-1` is the last element of the list, `-2` the penultimate, and so on.

Returns list of elements in the specified range.

#### Examples

```
redis> RPUSH mylist "one"
(integer) 1
redis> RPUSH mylist "two"
(integer) 2
redis> RPUSH mylist "three"
(integer) 3
redis> LRANGE mylist 0 0
1) "one"
```

# RPUSH

Syntax

```
RPUSH key element [element ...]
```

Insert all the specified values at the tail of the list stored at `key`. If `key` does not exist, it is created as empty list before performing the push operation. When `key` holds a value that is not a list, an error is returned.

It is possible to push multiple elements using a single command call just specifying multiple arguments at the end of the command. Elements are inserted one after the other to the tail of the list, from the leftmost element to the rightmost element. So for instance the command `RPUSH mylist a b c` will result into a list containing `a` as first element, `b` as second element and `c` as third element.

Returns the length of the list after the push operation.

#### Examples

```
redis> RPUSH mylist "hello"
(integer) 1
redis> RPUSH mylist "world"
(integer) 2
redis> LRANGE mylist 0 -1
1) "hello"
2) "world"
```

# LPUSHX

Syntax

```
LPUSHX key element [element ...]
```

Inserts specified values at the head of the list stored at `key`, only if `key` already exists and holds a list. In contrary to [`LPUSH`](https://redis.io/commands/lpush), no operation will be performed when `key` does not yet exist.

Returns the length of the list after the push operation.

#### Examples

```
redis> LPUSH mylist "World"
(integer) 1
redis> LPUSHX mylist "Hello"
(integer) 2
redis> LPUSHX myotherlist "Hello"
(integer) 0
redis> LRANGE mylist 0 -1
1) "Hello"
2) "World"
redis> LRANGE myotherlist 0 -1
(empty array)
```

# RPUSHX

Syntax

```
RPUSHX key element [element ...]
```

Inserts specified values at the tail of the list stored at `key`, only if `key` already exists and holds a list. In contrary to [`RPUSH`](https://redis.io/commands/rpush), no operation will be performed when `key` does not yet exist.

Returns the length of the list after the push operation.

#### Examples

```
redis> RPUSH mylist "Hello"
(integer) 1
redis> RPUSHX mylist "World"
(integer) 2
redis> RPUSHX myotherlist "World"
(integer) 0
redis> LRANGE mylist 0 -1
1) "Hello"
2) "World"
redis> LRANGE myotherlist 0 -1
(empty array)
```

# LPOP

Syntax

```
LPOP key [count]
```

Removes and returns the first elements of the list stored at `key`.

By default, the command pops a single element from the beginning of the list. When provided with the optional `count` argument, the reply will consist of up to `count` elements, depending on the list's length.

Returns :

-  When called without the `count` argument: the value of the first element, or `nil` when `key` does not exist. 

- When called with the `count` argument: list of popped elements, or `nil` when `key` does not exist.

#### Examples

```
redis> RPUSH mylist "one" "two" "three" "four" "five"
(integer) 5
redis> LPOP mylist
"one"
redis> LPOP mylist 2
1) "two"
2) "three"
redis> LRANGE mylist 0 -1
1) "four"
2) "five"
```

# RPOP

Syntax

```
RPOP key [count]
```

Removes and returns the last elements of the list stored at `key`.

By default, the command pops a single element from the end of the list. When provided with the optional `count` argument, the reply will consist of up to `count` elements, depending on the list's length.

Returns :

-  When called without the `count` argument: the value of the last element, or `nil` when `key` does not exist. 

- When called with the `count` argument: list of popped elements, or `nil` when `key` does not exist.

#### Examples

```
redis> RPUSH mylist "one" "two" "three" "four" "five"
(integer) 5
redis> RPOP mylist
"five"
redis> RPOP mylist 2
1) "four"
2) "three"
redis> LRANGE mylist 0 -1
1) "one"
2) "two"
```

# BLPOP

Syntax

```
BLPOP key [key ...] timeout
```

`BLPOP` is a blocking list pop primitive. It is the blocking version of [`LPOP`](https://redis.io/commands/lpop) because it blocks the connection when there are no elements to pop from any of the given lists. An element is popped from the head of the first list that is non-empty, with the given keys being checked in the order that they are given.

## Non-blocking behavior

When `BLPOP` is called, if at least one of the specified keys contains a non-empty list, an element is popped from the head of the list and returned to the caller together with the `key` it was popped from.

Keys are checked in the order that they are given. Let's say that the key `list1` doesn't exist and `list2` and `list3` hold non-empty lists. Consider the following command:

```
BLPOP list1 list2 list3 0
```

`BLPOP` guarantees to return an element from the list stored at `list2` (since it is the first non empty list when checking `list1`, `list2` and `list3` in that order).

Returns specifically:

- A `nil` multi-bulk when no element could be popped and the timeout expired.
- A two-element multi-bulk with the first element being the name of the key where an element was popped and the second element being the value of the popped element.

#### Examples

```
redis> DEL list1 list2
(integer) 0
redis> RPUSH list1 a b c
(integer) 3
redis> BLPOP list1 list2 0
1) "list1"
2) "a"
```

# BRPOP

Syntax

```
BRPOP key [key ...] timeout
```

`BRPOP` is a blocking list pop primitive. It is the blocking version of [`RPOP`](https://redis.io/commands/rpop) because it blocks the connection when there are no elements to pop from any of the given lists. An element is popped from the tail of the first list that is non-empty, with the given keys being checked in the order that they are given.

Returns specifically:

- A `nil` multi-bulk when no element could be popped and the timeout expired.
- A two-element multi-bulk with the first element being the name of the key where an element was popped and the second element being the value of the popped element.

#### Examples

```
redis> DEL list1 list2
(integer) 0
redis> RPUSH list1 a b c
(integer) 3
redis> BRPOP list1 list2 0
1) "list1"
2) "c"
```

# LMOVE

Syntax

```
LMOVE source destination <LEFT | RIGHT> <LEFT | RIGHT>
```

Atomically returns and removes the first/last element (head/tail depending on the `wherefrom` argument) of the list stored at `source`, and pushes the element at the first/last element (head/tail depending on the `whereto` argument) of the list stored at `destination`.

For example: consider `source` holding the list `a,b,c`, and `destination` holding the list `x,y,z`. Executing `LMOVE source destination RIGHT LEFT` results in `source` holding `a,b` and `destination` holding `c,x,y,z`.

If `source` does not exist, the value `nil` is returned and no operation is performed. If `source` and `destination` are the same, the operation is equivalent to removing the first/last element from the list and pushing it as first/last element of the list, so it can be considered as a list rotation command (or a no-op if `wherefrom` is the same as `whereto`).

This command comes in place of the now deprecated [`RPOPLPUSH`](https://redis.io/commands/rpoplpush). Doing `LMOVE RIGHT LEFT` is equivalent.

Returns the element being popped and pushed.

#### Examples

```
redis> RPUSH mylist "one"
(integer) 1
redis> RPUSH mylist "two"
(integer) 2
redis> RPUSH mylist "three"
(integer) 3
redis> LMOVE mylist myotherlist RIGHT LEFT
"three"
redis> LMOVE mylist myotherlist LEFT RIGHT
"one"
redis> LRANGE mylist 0 -1
1) "two"
redis> LRANGE myotherlist 0 -1
1) "three"
2) "one"
```

# BLMOVE

Syntax

```
BLMOVE source destination <LEFT | RIGHT> <LEFT | RIGHT> timeout
```

`BLMOVE` is the blocking variant of [`LMOVE`](https://redis.io/commands/lmove). When `source` contains elements, this command behaves exactly like [`LMOVE`](https://redis.io/commands/lmove). When used inside a [`MULTI`](https://redis.io/commands/multi)/[`EXEC`](https://redis.io/commands/exec) block, this command behaves exactly like [`LMOVE`](https://redis.io/commands/lmove). When `source` is empty, Redis will block the connection until another client pushes to it or until `timeout` (a double value specifying the maximum number of seconds to block) is reached. A `timeout` of zero can be used to block indefinitely.

This command comes in place of the now deprecated [`BRPOPLPUSH`](https://redis.io/commands/brpoplpush). Doing `BLMOVE RIGHT LEFT` is equivalent.

Returns the element being popped from `source` and pushed to `destination`. If `timeout` is reached, a [Null reply](https://redis.io/docs/reference/protocol-spec#resp-bulk-strings) is returned.

# LLEN

Syntax

```
LLEN key
```

Returns the length of the list stored at `key`. If `key` does not exist, it is interpreted as an empty list and `0` is returned. An error is returned when the value stored at `key` is not a list.

Returns the length of the list at `key`.

#### Examples

```
redis> LPUSH mylist "World"
(integer) 1
redis> LPUSH mylist "Hello"
(integer) 2
redis> LLEN mylist
(integer) 2
```

# LINDEX

Syntax

```
LINDEX key index
```

Returns the element at index `index` in the list stored at `key`. The index is zero-based, so `0` means the first element, `1` the second element and so on. Negative indices can be used to designate elements starting at the tail of the list. Here, `-1` means the last element, `-2` means the penultimate and so forth.

When the value at `key` is not a list, an error is returned.

Returns the requested element, or `nil` when `index` is out of range.

#### Examples

```
redis> LPUSH mylist "World"
(integer) 1
redis> LPUSH mylist "Hello"
(integer) 2
redis> LINDEX mylist 0
"Hello"
redis> LINDEX mylist -1
"World"
redis> LINDEX mylist 3
(nil)
```

# LINSERT

Syntax

```
LINSERT key <BEFORE | AFTER> pivot element
```

Inserts `element` in the list stored at `key` either before or after the reference value `pivot`.

When `key` does not exist, it is considered an empty list and no operation is performed.

An error is returned when `key` exists but does not hold a list value.

Returns the list length after a successful insert operation, `0` if the `key` doesn't exist, and `-1` when the `pivot` wasn't found.

#### Examples

```
redis> RPUSH mylist "Hello"
(integer) 1
redis> RPUSH mylist "World"
(integer) 2
redis> LINSERT mylist BEFORE "World" "There"
(integer) 3
redis> LRANGE mylist 0 -1
1) "Hello"
2) "There"
3) "World"
```

# LREM

Syntax

```
LREM key count element
```

Removes the first `count` occurrences of elements equal to `element` from the list stored at `key`. The `count` argument influences the operation in the following ways:

- `count > 0`: Remove elements equal to `element` moving from head to tail.
- `count < 0`: Remove elements equal to `element` moving from tail to head.
- `count = 0`: Remove all elements equal to `element`.

For example, `LREM list -2 "hello"` will remove the last two occurrences of `"hello"` in the list stored at `list`.

Note that non-existing keys are treated like empty lists, so when `key` does not exist, the command will always return `0`.

Returns the number of removed elements.

#### Examples

```
redis> RPUSH mylist "hello"
(integer) 1
redis> RPUSH mylist "hello"
(integer) 2
redis> RPUSH mylist "foo"
(integer) 3
redis> RPUSH mylist "hello"
(integer) 4
redis> LREM mylist -2 "hello"
(integer) 2
redis> LRANGE mylist 0 -1
1) "hello"
2) "foo"
```

# LSET

Syntax

```
LSET key index element
```

Sets the list element at `index` to `element`.

An error is returned for out of range indexes.

#### Examples

```
redis> RPUSH mylist "one"
(integer) 1
redis> RPUSH mylist "two"
(integer) 2
redis> RPUSH mylist "three"
(integer) 3
redis> LSET mylist 0 "four"
"OK"
redis> LSET mylist -2 "five"
"OK"
redis> LRANGE mylist 0 -1
1) "four"
2) "five"
3) "three"
```

# LTRIM

Syntax

```
LTRIM key start stop
```

Trim an existing list so that it will contain only the specified range of elements specified. Both `start` and `stop` are zero-based indexes, where `0` is the first element of the list (the head), `1` the next element and so on.

For example: `LTRIM foobar 0 2` will modify the list stored at `foobar` so that only the first three elements of the list will remain.

`start` and `end` can also be negative numbers indicating offsets from the end of the list, where `-1` is the last element of the list, `-2` the penultimate element and so on.

Out of range indexes will not produce an error: if `start` is larger than the end of the list, or `start > end`, the result will be an empty list (which causes `key` to be removed). If `end` is larger than the end of the list, Redis will treat it like the last element of the list.

A common use of `LTRIM` is together with [`LPUSH`](https://redis.io/commands/lpush) / [`RPUSH`](https://redis.io/commands/rpush). For example:

```
LPUSH mylist someelement
LTRIM mylist 0 99
```

This pair of commands will push a new element on the list, while making sure that the list will not grow larger than 100 elements. This is very useful when using Redis to store logs.

#### Examples

```
redis> RPUSH mylist "one"
(integer) 1
redis> RPUSH mylist "two"
(integer) 2
redis> RPUSH mylist "three"
(integer) 3
redis> LTRIM mylist 1 -1
"OK"
redis> LRANGE mylist 0 -1
1) "two"
2) "three"
```



## References:

- [Commands | Redis](https://redis.io/commands/)



