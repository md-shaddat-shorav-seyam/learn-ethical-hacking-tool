Here’s a full A-to-Z guide to **Redis**, the **Redis server**, and the **`redis-cli`** command-line tool.

Redis is an **in-memory data store** commonly used as a cache, key-value database, message broker, streaming engine, and more. It supports multiple data types such as strings, hashes, lists, sets, sorted sets, and others, and it also supports persistence, replication, and clustering. ([Redis][1])

---

# 1) What Redis is

Redis stands for **Remote Dictionary Server**. At its core, Redis stores data as **key → value** pairs, but unlike a simple key-value store, it gives you rich built-in data structures and atomic operations on them. It is known for being very fast because it primarily works in memory. ([Redis][1])

You will usually see Redis used for:

* caching
* session storage
* rate limiting
* queues
* pub/sub messaging
* leaderboards
* counters
* real-time analytics
* distributed locks

Redis can also persist data to disk and supports replication and other deployment features, so it is not “just a cache.” ([Redis][1])

---

# 2) Main Redis components

There are usually two things beginners confuse:

## Redis server

This is the actual database service process. It stores data and listens for client connections.

Common executable:

```bash
redis-server
```

## Redis CLI

This is the terminal client used to connect to a Redis server and run commands.

Common executable:

```bash
redis-cli
```

The `redis-cli` utility lets you connect to a Redis database, issue commands directly, use interactive mode, and run diagnostic modes such as latency checks. ([Redis][2])

---

# 3) How Redis works conceptually

A Redis server waits for client connections. A client, such as `redis-cli`, sends commands like:

```bash
SET username "seyam"
GET username
```

The server processes those commands and sends responses back.

So the flow is:

```text
redis-cli  --->  Redis server  --->  response
```

Redis commands are request/response based. Client apps and tools interact with Redis by sending commands, and those commands cover data access, server inspection, security, and configuration. ([Redis][3])

---

# 4) Installing Redis and redis-cli

When you install Redis Open Source, `redis-cli` is typically installed along with it. Redis’s official install docs describe Linux, macOS, and Windows-related installation options, and the `redis-cli` reference notes that it is included when Redis is installed. ([Redis][4])

Typical Linux usage after installation:

```bash
redis-server
redis-cli
```

---

# 5) Starting the Redis server

Basic server start:

```bash
redis-server
```

That starts Redis with default settings.

To start using a config file:

```bash
redis-server /etc/redis/redis.conf
```

Once running, Redis usually listens on port `6379` unless configured otherwise.

You can test the connection with:

```bash
redis-cli ping
```

Expected response:

```text
PONG
```

The official `redis-cli` docs show `PING`, `SET`, and `GET` as basic connectivity tests. ([Redis][5])

---

# 6) Connecting with redis-cli

## Local connection

```bash
redis-cli
```

This usually connects to:

* host: `127.0.0.1`
* port: `6379`

## Connect to another host

```bash
redis-cli -h 127.0.0.1 -p 6379
```

## Connect with password

```bash
redis-cli -h 127.0.0.1 -p 6379 -a yourpassword
```

## Use environment variable instead of `-a`

```bash
export REDISCLI_AUTH=yourpassword
redis-cli -h 127.0.0.1 -p 6379
```

Using `REDISCLI_AUTH` is documented by Redis as an alternative to passing the password directly with `-a`. ([Redis][5])

## Connect to a specific database number

```bash
redis-cli -n 1
```

Redis supports multiple logical databases in many setups, often selected by index.

---

# 7) Using redis-cli in two styles

## Direct one-shot mode

You run a single command from the shell:

```bash
redis-cli SET name "Redis"
redis-cli GET name
```

## Interactive mode

You launch the CLI first:

```bash
redis-cli
```

Then type commands inside it:

```text
127.0.0.1:6379> SET name "Redis"
OK
127.0.0.1:6379> GET name
"Redis"
```

The official docs describe both direct command execution and interactive mode. ([Redis][5])

---

# 8) Understanding Redis data model

Redis stores values under keys, but the values can be different data types.

Major data types include:

* strings
* hashes
* lists
* sets
* sorted sets
* streams

Redis docs also mention support for additional data structures and capabilities depending on version and modules/features. ([Redis][1])

---

# 9) Core Redis commands by category

I’ll cover the most important command families you will use with `redis-cli`.

---

# 10) Server and connection commands

## PING

Checks if the server is alive.

```bash
redis-cli PING
```

Response:

```text
PONG
```

## ECHO

Returns a message.

```bash
redis-cli ECHO "hello"
```

## AUTH

Authenticates the connection if the server requires it.

```bash
AUTH yourpassword
```

## SELECT

Switches logical database.

```bash
SELECT 1
```

## QUIT

Exits the CLI session.

```bash
QUIT
```

## INFO

Shows server statistics and details.

```bash
INFO
INFO memory
INFO replication
INFO stats
```

Redis documents `INFO` as returning information and statistics about the server, with optional sections such as memory or replication. ([Redis][6])

## DBSIZE

Returns number of keys in the current database.

```bash
DBSIZE
```

## CLIENT commands

Manage or inspect client connections.

```bash
CLIENT LIST
CLIENT ID
CLIENT INFO
CLIENT SETNAME mycli
```

The Redis command reference includes many `CLIENT` subcommands such as `CLIENT LIST`, `CLIENT ID`, `CLIENT INFO`, and `CLIENT SETNAME`. ([Redis][7])

---

# 11) Key management commands

## EXISTS

Checks whether a key exists.

```bash
EXISTS mykey
```

## DEL

Deletes a key.

```bash
DEL mykey
```

## UNLINK

Deletes asynchronously.

```bash
UNLINK mykey
```

## RENAME

Renames a key.

```bash
RENAME oldkey newkey
```

## EXPIRE

Set TTL in seconds.

```bash
EXPIRE session:1 60
```

## PEXPIRE

Set TTL in milliseconds.

```bash
PEXPIRE session:1 1500
```

## TTL

Get remaining TTL in seconds.

```bash
TTL session:1
```

## PTTL

Get remaining TTL in milliseconds.

```bash
PTTL session:1
```

## TYPE

See key type.

```bash
TYPE mykey
```

## KEYS

Find keys matching a pattern.

```bash
KEYS user:*
```

Use `KEYS` carefully on large databases, because it can be expensive.

## SCAN

Safer incremental iteration over keys.

```bash
SCAN 0
SCAN 0 MATCH user:* COUNT 100
```

The Redis command reference includes `SCAN` specifically for iterating over key names in the database. ([Redis][8])

---

# 12) String commands

Strings are the most basic Redis type. They can hold text, numbers, JSON strings, tokens, counters, and more.

## SET

Store a value.

```bash
SET name "Seyam"
```

## GET

Read a value.

```bash
GET name
```

Redis documents that `GET` returns `nil` if the key does not exist and errors if the stored value is not a string. ([Redis][9])

## SET with expiration

```bash
SET otp "123456" EX 60
```

## SET only if absent

```bash
SET lock:job1 "worker-a" NX EX 30
```

## SET only if present

```bash
SET name "newname" XX
```

Redis documents multiple `SET` options such as `NX` and `XX`, and newer docs also show additional conditional options. ([Redis][10])

## MSET / MGET

Set or get multiple keys.

```bash
MSET a 1 b 2 c 3
MGET a b c
```

## APPEND

Append text.

```bash
APPEND greeting " world"
```

## INCR / DECR

Increment or decrement numbers.

```bash
SET visits 10
INCR visits
DECR visits
INCRBY visits 5
DECRBY visits 2
```

## GETSET or GETDEL-style workflows

Redis has commands for atomic read/change patterns depending on version and exact need.

Example:

```bash
GETDEL tempkey
```

---

# 13) Hash commands

Hashes store field-value pairs inside one key. Great for user profiles or objects.

Example concept:

```text
user:1 -> {name: "Seyam", age: "18", city: "Dhaka"}
```

## HSET

```bash
HSET user:1 name "Seyam" age 18 city "Dhaka"
```

## HGET

```bash
HGET user:1 name
```

## HGETALL

```bash
HGETALL user:1
```

## HMGET

```bash
HMGET user:1 name city
```

## HKEYS

```bash
HKEYS user:1
```

## HVALS

```bash
HVALS user:1
```

## HEXISTS

```bash
HEXISTS user:1 age
```

## HDEL

```bash
HDEL user:1 city
```

## HINCRBY

```bash
HINCRBY user:1 score 10
```

Real use case: user profile cache, shopping cart summary, analytics counters.

---

# 14) List commands

Lists are ordered sequences, useful for queues and task pipelines.

## LPUSH

Push to left.

```bash
LPUSH tasks "task1"
LPUSH tasks "task2"
```

## RPUSH

Push to right.

```bash
RPUSH tasks "task3"
```

## LPOP / RPOP

Pop from left or right.

```bash
LPOP tasks
RPOP tasks
```

## LRANGE

Read items.

```bash
LRANGE tasks 0 -1
```

## LLEN

Count items.

```bash
LLEN tasks
```

## BLPOP / BRPOP

Blocking pop for worker queues.

```bash
BLPOP tasks 0
```

Real use case: background job queue.

---

# 15) Set commands

Sets store unique unordered values.

## SADD

```bash
SADD tags redis database cli
```

## SMEMBERS

```bash
SMEMBERS tags
```

## SISMEMBER

```bash
SISMEMBER tags redis
```

## SREM

```bash
SREM tags cli
```

## SCARD

```bash
SCARD tags
```

## SUNION / SINTER / SDIFF

```bash
SUNION set1 set2
SINTER set1 set2
SDIFF set1 set2
```

Real use case: unique visitors, permissions, tags, feature flags.

---

# 16) Sorted set commands

Sorted sets store unique members with a numeric score. Perfect for rankings and leaderboards.

## ZADD

```bash
ZADD leaderboard 100 alice 150 bob 120 charlie
```

## ZRANGE

```bash
ZRANGE leaderboard 0 -1
```

## ZRANGE with scores

```bash
ZRANGE leaderboard 0 -1 WITHSCORES
```

## ZREVRANGE

Highest to lowest:

```bash
ZREVRANGE leaderboard 0 2 WITHSCORES
```

## ZSCORE

```bash
ZSCORE leaderboard bob
```

## ZINCRBY

```bash
ZINCRBY leaderboard 10 alice
```

## ZREM

```bash
ZREM leaderboard charlie
```

Real use case: top players, top sellers, trending content.

---

# 17) Pub/Sub commands

Redis supports publish/subscribe messaging.

## SUBSCRIBE

```bash
SUBSCRIBE news
```

## PUBLISH

In another terminal:

```bash
PUBLISH news "Hello subscribers"
```

## PSUBSCRIBE

Pattern-based subscription:

```bash
PSUBSCRIBE news:*
```

Real use case: chat notifications, live updates, event broadcasting.

---

# 18) Stream commands

Streams are append-only log structures useful for event processing and consumer groups.

## XADD

```bash
XADD orders * user "seyam" total 500
```

## XRANGE

```bash
XRANGE orders - +
```

## XREAD

```bash
XREAD COUNT 10 STREAMS orders 0
```

## XGROUP / XREADGROUP

Consumer-group processing:

```bash
XGROUP CREATE orders workers 0
XREADGROUP GROUP workers c1 COUNT 1 STREAMS orders >
```

Redis lists streams among its supported data structures and command families. ([Redis][1])

---

# 19) Transactions and atomicity

Redis operations are atomic per command. For grouping commands:

## MULTI

Start transaction:

```bash
MULTI
```

## Queue commands

```bash
SET a 10
INCR a
GET a
```

## EXEC

Run all queued commands:

```bash
EXEC
```

## DISCARD

Cancel transaction:

```bash
DISCARD
```

## WATCH

Optimistic locking:

```bash
WATCH balance
MULTI
DECRBY balance 50
EXEC
```

Real use case: safe balance changes, race-condition reduction.

---

# 20) Persistence-related concepts

Redis supports persistence so memory data can be saved to disk. The official docs describe built-in persistence capabilities, and Redis is commonly configured with snapshotting and/or append-only persistence depending on deployment. ([Redis][1])

Useful commands:

## SAVE

Synchronous save to disk:

```bash
SAVE
```

## BGSAVE

Background save:

```bash
BGSAVE
```

## LASTSAVE

Time of last successful save:

```bash
LASTSAVE
```

Be careful using persistence-related administrative commands on production servers.

---

# 21) Replication and monitoring commands

## INFO replication

```bash
INFO replication
```

## ROLE

Shows whether node is primary/replica:

```bash
ROLE
```

## MONITOR

Streams every processed command:

```bash
MONITOR
```

`MONITOR` is powerful for debugging, but can be very noisy and expensive on busy servers.

## SLOWLOG

Inspect slow commands:

```bash
SLOWLOG GET 10
```

## LATENCY

Latency inspection:

```bash
LATENCY LATEST
LATENCY DOCTOR
```

The `redis-cli` documentation notes special modes for latency checking and printing latency statistics. ([Redis][2])

---

# 22) Dangerous but important commands

## FLUSHDB

Delete all keys in current DB:

```bash
FLUSHDB
```

## FLUSHALL

Delete all keys in all DBs:

```bash
FLUSHALL
```

## CONFIG GET

```bash
CONFIG GET *
CONFIG GET requirepass
```

## CONFIG SET

```bash
CONFIG SET maxmemory 256mb
```

## SHUTDOWN

Stop server:

```bash
SHUTDOWN
```

These should be used very carefully, especially in production.

---

# 23) Redis CLI essentials

Now let’s focus specifically on **`redis-cli`**.

The official `redis-cli` docs describe it as the Redis command-line interface and note that it supports interactive mode, direct command execution, authentication, remote connection, and special operational modes. ([Redis][2])

## Basic syntax

```bash
redis-cli [options] [command]
```

Examples:

```bash
redis-cli PING
redis-cli SET mykey hello
redis-cli GET mykey
```

## Common connection options

```bash
redis-cli -h 127.0.0.1 -p 6379
redis-cli -a mypassword
redis-cli -n 2
```

Common meanings:

* `-h` host
* `-p` port
* `-a` password
* `-n` database number

## URI-style connection

Some environments also use URI-based connection patterns, depending on client and shell workflow.

---

# 24) Interactive redis-cli features

When you launch:

```bash
redis-cli
```

you enter interactive mode. The docs note that interactive mode includes basic line editing capabilities. ([Redis][2])

Example:

```text
127.0.0.1:6379> SET city "Dhaka"
OK
127.0.0.1:6379> GET city
"Dhaka"
127.0.0.1:6379> DEL city
(integer) 1
```

---

# 25) Running raw Redis commands with redis-cli

A very important point:

**`redis-cli` itself is not the database language.**
It is the tool you use to send **Redis commands** to the Redis server.

Example:

```bash
redis-cli HSET user:1 name "Seyam"
```

Here:

* `redis-cli` = client program
* `HSET user:1 name "Seyam"` = Redis command

---

# 26) Viewing all supported commands

Redis exposes introspection commands.

## COMMAND

Returns information about commands available on the server.

```bash
COMMAND
```

Redis documents `COMMAND` as returning details about the server’s runtime command capabilities. ([Redis][11])

## COMMAND DOCS

```bash
COMMAND DOCS
COMMAND DOCS GET SET HSET
```

Redis documents `COMMAND DOCS` as returning documentary information about commands. ([Redis][12])

This is very useful because the exact command set can vary by Redis version and enabled features/modules. Redis’s current docs maintain command references for multiple versions, including 7.2, 7.4, 8.0, 8.2, and 8.4. ([Redis][8])

---

# 27) Output formats in redis-cli

Typical outputs include:

```text
OK
(integer) 1
"hello"
(nil)
(error) WRONGTYPE Operation against a key holding the wrong kind of value
```

Examples:

```bash
SET name "abc"
```

returns:

```text
OK
```

```bash
GET missing
```

returns:

```text
(nil)
```

```bash
GET somehashkey
```

may return a wrong-type error if that key is not a string. Redis explicitly documents this for `GET`. ([Redis][9])

---

# 28) Helpful redis-cli examples

## Example 1: quick health check

```bash
redis-cli PING
redis-cli INFO server
redis-cli INFO memory
```

## Example 2: basic string workflow

```bash
redis-cli SET app:name "MyApp"
redis-cli GET app:name
redis-cli DEL app:name
```

## Example 3: create expiring OTP

```bash
redis-cli SET otp:user1 "834211" EX 120
redis-cli TTL otp:user1
redis-cli GET otp:user1
```

## Example 4: hash-based user profile

```bash
redis-cli HSET user:100 name "Seyam" age 18 city "Dhaka"
redis-cli HGETALL user:100
```

## Example 5: queue with lists

```bash
redis-cli RPUSH jobs "resize-image-1"
redis-cli RPUSH jobs "send-email-2"
redis-cli LPOP jobs
```

## Example 6: leaderboard

```bash
redis-cli ZADD scores 50 alice 70 bob 65 charlie
redis-cli ZREVRANGE scores 0 -1 WITHSCORES
```

---

# 29) Real-world use cases

## Caching

Store computed or database-heavy results:

```bash
SET product:1 '{"id":1,"name":"Cake"}' EX 300
GET product:1
```

## Sessions

```bash
SET session:abc123 '{"user_id":55}' EX 1800
```

## Rate limiting

```bash
INCR rate:user:55
EXPIRE rate:user:55 60
```

## Job queues

```bash
LPUSH email_jobs "welcome:user55"
BRPOP email_jobs 0
```

## Live notifications

```bash
PUBLISH notifications "New order received"
SUBSCRIBE notifications
```

## Rankings

```bash
ZADD game_ranks 999 player1
ZREVRANGE game_ranks 0 9 WITHSCORES
```

---

# 30) Common beginner mistakes

## Using `GET` on non-string data

If a key holds a hash, list, or set, `GET` will fail.

Check type first:

```bash
TYPE mykey
```

## Using `KEYS *` in production

This can be expensive on large datasets.

Prefer:

```bash
SCAN 0 MATCH pattern:* COUNT 100
```

## Forgetting TTL

Temporary data like OTPs, sessions, and cache should usually expire.

## Confusing `redis-cli` commands with shell commands

This is wrong:

```bash
SET mykey hello
```

in your normal Linux shell, unless you are already inside `redis-cli`.

Correct:

```bash
redis-cli SET mykey hello
```

or:

```bash
redis-cli
SET mykey hello
```

## Storing everything as one giant string

Often hashes, lists, sets, and sorted sets are better choices.

---

# 31) Useful administrative commands

```bash
INFO
INFO clients
INFO memory
INFO persistence
INFO replication
DBSIZE
CLIENT LIST
SLOWLOG GET 10
MEMORY USAGE mykey
TIME
```

`INFO` is officially documented with multiple sections, and the command reference catalogs server/admin commands in detail. ([Redis][6])

---

# 32) Security basics

For real deployments:

* do not expose Redis publicly without protection
* use authentication
* restrict network access
* use TLS if supported in your environment
* be careful with admin commands like `FLUSHALL`, `CONFIG SET`, and `SHUTDOWN`

The exact secure setup depends on your Redis edition, version, hosting method, and network architecture.

---

# 33) How to discover command syntax fast

Best methods:

## Inside docs

Redis maintains a full commands reference. ([Redis][7])

## Using introspection

```bash
COMMAND DOCS SET
COMMAND DOCS HSET
```

## Quick cheat sheet

Redis also maintains a recent commands cheat sheet with CLI examples across common categories. ([Redis][13])

---

# 34) Mini practice lab

Try this locally.

Start server:

```bash
redis-server
```

Open CLI:

```bash
redis-cli
```

Then run:

```text
PING
SET username "seyam"
GET username
HSET profile name "Seyam" country "Bangladesh"
HGETALL profile
LPUSH tasks "learn redis"
LPUSH tasks "practice cli"
LRANGE tasks 0 -1
SADD skills redis linux javascript
SMEMBERS skills
ZADD leaderboard 100 seyam 95 ali 110 sara
ZREVRANGE leaderboard 0 -1 WITHSCORES
SET otp "654321" EX 30
TTL otp
INFO memory
DBSIZE
```

That one session gives you exposure to server health, strings, hashes, lists, sets, sorted sets, expiration, and monitoring.

---

# 35) Quick command cheat sheet

## Connection

```bash
redis-cli
redis-cli -h HOST -p PORT
redis-cli -a PASSWORD
redis-cli -n DBNUMBER
PING
AUTH password
SELECT 1
QUIT
```

## Keys

```bash
EXISTS key
DEL key
UNLINK key
RENAME old new
TYPE key
EXPIRE key 60
TTL key
SCAN 0 MATCH user:* COUNT 100
```

## Strings

```bash
SET key value
GET key
MSET a 1 b 2
MGET a b
INCR counter
DECR counter
APPEND key text
```

## Hashes

```bash
HSET user:1 name "Seyam"
HGET user:1 name
HGETALL user:1
HMGET user:1 name age
HEXISTS user:1 city
HDEL user:1 city
```

## Lists

```bash
LPUSH jobs a
RPUSH jobs b
LPOP jobs
RPOP jobs
LRANGE jobs 0 -1
BLPOP jobs 0
```

## Sets

```bash
SADD tags redis cli
SMEMBERS tags
SISMEMBER tags redis
SREM tags cli
```

## Sorted sets

```bash
ZADD scores 10 a 20 b
ZRANGE scores 0 -1 WITHSCORES
ZREVRANGE scores 0 -1 WITHSCORES
ZINCRBY scores 5 a
```

## Pub/Sub

```bash
SUBSCRIBE news
PUBLISH news "hello"
```

## Streams

```bash
XADD mystream * field1 value1
XRANGE mystream - +
XREAD STREAMS mystream 0
```

## Server/admin

```bash
INFO
DBSIZE
CLIENT LIST
SLOWLOG GET 10
MONITOR
FLUSHDB
FLUSHALL
```

---

# 36) Final understanding

The simplest way to remember everything is this:

* **Redis server** is the database service
* **`redis-cli`** is the terminal tool used to talk to that server
* **Redis commands** are the actual instructions, like `SET`, `GET`, `HSET`, `LPUSH`, `ZADD`, `INFO`, and so on

So when you type:

```bash
redis-cli SET name "Seyam"
```

you are using:

* `redis-cli` = the client program
* `SET` = the Redis command
* `name` = key
* `"Seyam"` = value

---

# 37) Best next steps

Practice these in order:

1. `PING`, `SET`, `GET`, `DEL`
2. `EXPIRE`, `TTL`
3. `HSET`, `HGETALL`
4. `LPUSH`, `LRANGE`
5. `SADD`, `SMEMBERS`
6. `ZADD`, `ZREVRANGE WITHSCORES`
7. `INFO`, `CLIENT LIST`, `SCAN`

After that, move to:

* transactions
* pub/sub
* streams
* persistence
* replication

If you want, I can turn this into a **structured learning roadmap with daily practice tasks and hands-on exercises**.

[1]: https://redis.io/docs/latest/get-started/?utm_source=chatgpt.com "Open Source | Docs"
[2]: https://redis.io/docs/latest/develop/tools/cli/?utm_source=chatgpt.com "Redis CLI | Docs"
[3]: https://redis.io/docs/latest/develop/using-commands/?utm_source=chatgpt.com "Using Redis commands | Docs"
[4]: https://redis.io/docs/latest/operate/oss_and_stack/install/archive/install-redis/?utm_source=chatgpt.com "Install Redis | Docs"
[5]: https://redis.io/docs/latest/operate/rs/references/cli-utilities/redis-cli/?utm_source=chatgpt.com "redis-cli | Docs"
[6]: https://redis.io/docs/latest/commands/info/?utm_source=chatgpt.com "INFO | Docs"
[7]: https://redis.io/docs/latest/commands/?utm_source=chatgpt.com "Commands | Docs"
[8]: https://redis.io/docs/latest/commands/redis-7-2-commands/?utm_source=chatgpt.com "Redis 7.2 Commands Reference | Docs"
[9]: https://redis.io/docs/latest/commands/get/?utm_source=chatgpt.com "GET | Docs"
[10]: https://redis.io/docs/latest/commands/set/?utm_source=chatgpt.com "SET | Docs"
[11]: https://redis.io/docs/latest/commands/command/?utm_source=chatgpt.com "COMMAND | Docs"
[12]: https://redis.io/docs/latest/commands/command-docs/?utm_source=chatgpt.com "COMMAND DOCS"
[13]: https://redis.io/tutorials/howtos/quick-start/cheat-sheet/?utm_source=chatgpt.com "Redis Commands Cheat Sheet: CLI, Node.js, Python, C# ..."
