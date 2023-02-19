Redis Commands

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
