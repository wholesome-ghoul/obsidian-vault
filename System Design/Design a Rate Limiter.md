---
tags: system-design
deck: system-design
---

# Rate limiter usage examples #card
<!-- 1700183227849 3f1863b5128a929deb44ad3ef7db0ac9 -->

- user can write no more than 2 posts per second
- you can create up to 10 accounts per day from the same IP address
- you can claim rewards no more than 5 times per week from the same device

# Benefits of rate limiting #card
<!-- 1700183227928 360773a7011d18f67abccc74f1e03cd1 -->

- preventing resource starvation caused by DoS attack
- prevent servers from being overloaded
- reduce cost

# Possible questions while designing rate limiter #card
<!-- 1700183227945 e0801cf20eccace9dbb40e29d0c00732 -->

- what kind of rate limiter are we going to design? Is it client-side or server-side API rate limiter?
- Does rate limiter throttle API requests based on IP, the user ID, or the other properties?
- What is the scale of the system? Is it built for a startup or a big company with a large user base?
- Will the system work in distributed environment?
- Is the rate limiter a separate service or should it be implemented in application code? In a gateway or on the server-side?
- Do we need to inform users who are throttled?

# What is API gateway? #card
<!-- 1700183227978 91edcc683ad6d3f1aab8bcdc2e7ebeb7 -->

API gateway is a fully managed service that supports rate limiting, SSL termination, authentication, IP whitelisting, servicing static content, etc.

# Rate limiting algorithms #card
<!-- 1700183227995 933e77cfabbccb2e1557597bea2b8907 -->

- Token bucket
- Leaking bucket
- Fixed window counter
- Sliding window log
- Sliding window counter

# How does Token bucket algorithm work? #card
<!-- 1700183228028 1d81519f943c779e1ff1cfe8eaf55d76 -->

- token bucket is a container that has pre-defined capacity. Tokens are put in the bucket at preset rates periodically. Once the bucket is full, no more tokens are added. Once the bucket is full, extra tokens will overflow.
- each request consumes one token. When request arrives, we check if there are enough tokens in the bucket
- if there are enough tokens, request consumes one token, and it goes through
- if there are not enough tokens, the request is dropped.

Token bucket algorithm has two parameters:

- bucket size: the maximum number of tokens in the bucket
- refill rate

![](figure-4-5-FGZ35C5S.svg)

| Pros                                         | Cons                                                        |
| -------------------------------------------- | ----------------------------------------------------------- |
| easy to implement                            | two parameters in the algorithm might be challengin to tune |
| memory efficient                             |                                                             |
| allows a burst of traffic for short periods. |                                                             |

# How does Leaking bucket algorithm work? #card
<!-- 1700183228085 18cf35a6f3d55fac4e03f98c3490a85c -->

Similar to Token bucket except requests are processed at a fixed rate. It is usually implemented with a FIFO queue.

- when request arrives, system check if queue is full. If it's not full, the request is added to the queue.
- otherwise, the request is dropped
- requests are pulled from the queue and processed at regular intervals

![](figure-4-7-AI26NI2Y.svg)

Leaking bucket has two parameters:

- Bucket size: queue size
- Outflow size: how many requests can be processed at a fixed rate, usually in seconds.

| Pros                                                                                                             | Cons                                                                                                                                |
| ---------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| memory efficient given the limited queue size                                                                    | burst of traffic fills up the queue with old requests, and if they are not processed in time, recent requests will lbe rate limited |
| requests are proccesed at a fixed rate, therefore is suitable for use cases that a stable outflow rate is needed | two params can be challenging to tune                                                                                               |

# How does Fixed window counter algorithm work? #card
<!-- 1700183712759 e5a612f4e62b43991133c12d53f28522 -->

- the algorithm divides the timeline into fix-sized time windows and assign a counter for each window
- each request increments the counter by one
- once the counter reaches the pre-defined threshold, new requests are dropped until a new time window starts

![](figure-4-8-WZZYUXFU.svg)

A major problem with this algorithm is that burst of traffic at the edges of windows could cause more requests than allowed quota to go through.

![](figure-4-9-52MK6L22.svg)

| Pros                                         | Cons                                                        |
| -------------------------------------------- | ----------------------------------------------------------- |
| easy to understand | spike in traffic at the edges of a window could cause more requests than allowed quota to go through|
| memory efficient                             |                                                             |
| resetting available quota at the end of a unit time window fits certain use cases |                                                             |

# How does Sliding window log algorithm work? #card
<!-- 1700184155084 5f1d1477b5b4930ca66f33f23bdac69e -->

Sliding window log algorithm fixes Fixed window counter algorithm's issue.

- algorithm keeps track of request timestamps. Timestamp data is usually kept in cache, such as sorted sets of Redis.
- when new request comes in, remove all the outdated timestamps. Outdated timestamps are defined as those older than the start of the current time window.
- add timestamp of the new request to the log
- if the log size is the same or lower than the allowed count, a request is accepted. Otherwise, it is rejected.


| Pros                                         | Cons                                                        |
| -------------------------------------------- | ----------------------------------------------------------- |
| very accurate | consumes a lot of memory, because even if the request is rejected, its timestamp might still be stored in the memory|

# How does Sliding window counter algorithm work?

It is a hybrid approach that combines the fixed window counter and sliding window log.

# How does a client know whether it is being throttled? #card
<!-- 1700184844112 3340b75e3506ac499341434526db3efe -->

```
X-Ratelimit-Remaining: The remaining number of allowed requests within the window.

X-Ratelimit-Limit: It indicates how many calls the client can make per time window.

X-Ratelimit-Retry-After: The number of seconds to wait until you can make a request again without being throttled.
```

# Detailed design of rate limiter #card
<!-- 1700184844158 ada2511f1ef062be7bf35a1ecd73aa7f -->

![](figure-4-13-G2VF2RCQ.webp)

# Possible issues of rate limiter in a distributed environment #card
<!-- 1700184844201 c91a64d939edb5478896dd3cf17af729 -->

- race condition
- synchronization
