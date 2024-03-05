# Redis Notes

```
# KEY, SCAN
# DEL, UNLINK
# EXISTS
# SET

HSET bike:1 model Deimos brand Ergonom type 'Enduro bikes' price 4972
HGET bike:1 model
HGET bike:1 price
HDEL bike:1 type
HEXISTS bike:1 price
HINCRBY bike:1 price 15
HGETALL bike:1

# Key expiration
SET seat0hold Row:A:Seat:4 PX 50000
SET seat0hold Row:A:Seat:4 EX 50
TTL seat-hold
PTTL seat-hold
PERSIS seat-hold
PEXPIRE seat-hold 1000
GET seat-hold
TYPE seat-hold
OBJECT ENCODING seat-hold

# Lists (implemented as doubly linked list)
RPUSH playlist 51
RPUSH playlist 71
LLEN playlist
LRANGE playlist 0 -1 
LPOP playlist

INCR user:23:visit-count
DECRBY user:23:credit-balance -30
INCRBY user:23:credit-balance -30

DBSIZE

SCAN 0 MATCH "bike:*" COUNT 100
```

```bash
# redis server with redisearch, redisjson
docker run -p 6379:6379 redis/redis-stack-server:latest
```
