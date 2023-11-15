---
tags: system-design
deck: system-design
---

# What is Load balancer? #card
<!-- 1699761472110 1e48dfe45da0b81d705610e6523fc8ff -->

Load balancer distributes incoming traffic evenly among the web servers. For better security, only LB has public IP, while internal web servers have private IPs.

![fig](figure-1-4-2EGRRANZ.webp)

# What is database replication? #card
<!-- 1700026618874 cb1d7befc0e411182a80c216646258ea -->

The process of creating copies of a database. One of the common pattern is master/slave relationship. Generally, master supports only write operations, while slave supports read operations.

# What are advantages of database replication? #card
<!-- 1700027297603 00ba596e547a0ecfd7dc82ca5202fa49 -->

- Better performance: in master/slave model, all write operations happen in master nodes, whereas, read operations are distributed across slave nodes. This improves performance because it allows more queries to be processed in parallel.
- Reliability: if one of the databases are destroyed by any phenomena, data is not lost, as it is replicated across multiple locations.
- High availability: if one of the databases goes offline for some reason, application is still available, as you can access data stored in another database server.

# In database replication, what if one of the databases goes offline? #card
<!-- 1700027297632 994e190992f6d1456579aaf1d75c3600 -->

<!-- AnkiFront:start -->

![](figure-1-5-TJLQVE5N.svg)

<!-- AnkiFront:end -->
<!-- AnkiBack:start -->

- if only one slave database is available and it goes offline, read operations will be directed to the master database temporary. As soon as the issue is found, new slave database will replace the old one. In case of multiple slave databases are available, read operations are redirected to other healthy slave databases.
- if the master goes offline, a slave database will be promoted to the new master. As soon as issue is found, new slave database will replace the old one.

<!-- AnkiBack:end -->

# LB + Database replication example #card
<!-- 1700029345124 5e5cb72b088ed4fda90fb12f811bac42 -->

![](figure-1-6-L2YNDDKF.webp)

# What is cache? #card
<!-- 1700030393101 28ad26632eb51a8c1eeeca86daf09f3b -->

Cache is a temporary storage that stores the result of expensive responses or frequently accessed data in memory so that subsequent requests are served more quickly.

# How does cache tier work? #card
<!-- 1700030393130 2b7a80977265953801c01279405c8a2d -->

The cache tier is a temporary data store layer, much faster than database. The benefits of having separate cache tier include better system performance and the ability to scale the cache tier independently.

# Considerations for using cache #card
<!-- 1700030393168 492614b479ce3f8ef3cef861f01068f9 -->

- decide when to use cache
- expiration policy
- consistency
- mitigating failures
- eviction policy

# What is CDN? #card
<!-- 1700040765089 a4e5423b8d73b93e4f14a651c49acf73 -->

CDN is a network of geographically dispersed servers used to deliver static content, like images, videos, CSS, Javascript, etc.

# Considerations of using CDN #card
<!-- 1700040765129 fcce713f23aa36dcb3fd996c5ae81d8b -->

- cost
- setting an appropriate cache expiry
- CDN fallback
- invalidating files

# Example: cache tier + DB replication + LB + CDN #card
<!-- 1700040765148 ae457f2faf4f8c89be7d59606071f06d -->

![](figure-1-11-VI5Z74Q2.webp)

# What is stateless web tier? #card
<!-- 1700041386450 b860622e02b1099d66adc353c51a435e -->

We store state (for instance, user session data) in the persistent storage such as relational database or NoSQL. Each web server in the cluster can access state from databases. This is called stateless web tier.

# What are sticky sessions? #card
<!-- 1700041386473 f141982a014eedbbb4bc5308cf96faa4 -->

In stateful architecture, each user session is handled by a respective server (the one that authenticated user first, for example). So, if user A accesses server A, user A state is stored in server A. However, if user A accesses server B, it won't retain the session. To overcome this problem we have sticky sessions, which is handled by load balancer. However, this adds some overhead, and on top of everything, adding/removing servers is much more difficult with this approach.

# Example: cache tier + DB replication + LB + CDN + stateless architecture #card
<!-- 1700041386498 3fe0340fa42cadcc5daabdde5bcabc10 -->

![](figure-1-14-CCBCQMO6.webp)

# What is geoDNS? #card
<!-- 1700041743647 65baf94b497d691af7a2452bf8dca54c -->

geoDNS is a DNS service that allows domain names to be resolved to IP addresses based on the location of a user.

# Technical challenges of using multi-data center setup #card
<!-- 1700041743673 c4556a07a22715a674f3c581cfeaa8a5 -->

- traffic redirection
- data synchronization: in failover cases, traffic might be routed to a data center where data is not available
- test and deployment

# Example: cache tier + DB replication + LB + CDN + stateless architecture + data centers #card
<!-- 1700041743701 2eddd773d63b40018b909ae492c754d9 -->

![](figure-1-15-GICUI26J.webp)

# What is message queue? #card
<!-- 1700042019503 ace4b5d60e5b8bed9de2dd697716b7eb -->

Message queue is a durable component, stored in memory, that supports async communication. The basic architecture is simple. Input services, called producers/publishers, create messages, and publish them to a message queue. Other services or servers, called consumers/subscribers, connect to the queue and perform actions defined by the messages.

![](figure-1-17-J2NLNRNY.svg)

Decoupling makes the message queue a preferred architecture for building a scalable and reliable application.

# Example: some of the useful metrics #card

- host level metrics: CPU, memory, disk I/O
- aggregated level metrics: performance of the entire db tier, cache tier
- key business metrics: daily active users, retention, revenue

# Example: cache tier + DB replication + LB + CDN + stateless architecture + data centers + tools #card

![](figure-1-19-MOPDW7TD.webp)

# Drawbacks of vertical scaling #card

- hardware limits
- SPOF
- cost

# Potential problems of sharding in horizontal scaling #card

- resharding data
- celebrity problem
- join and de-normalization
