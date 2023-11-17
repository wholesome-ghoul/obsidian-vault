---
tags: system-design
deck: system-design
---

# Latency numbers #card
<!-- 1700179740101 622129279f22368761b4b62d66b27aaa -->

| Operation name                                  | Time  |
| ----------------------------------------------- | ----- |
| L1 cache reference                              | 0.5ns |
| Branch mispredict                               | 5ns   |
| L2 cache reference                              | 7ns   |
| Mutex lock/unlock                               | 100ns |
| Main memory reference                           | 100ns |
| Compress 1K bytes with Zippy                    | 10us  |
| Send 2K bytes over 1 Gbps network               | 20us  |
| Read 1 MB sequentially from memory              | 250us |
| Round trip within the same datacenter           | 500us |
| Disk seek                                       | 10ms  |
| Read 1 MB sasequentially from the network       | 10ms  |
| Read 1 MB sequentially from disk                | 30ms  |
| Send packet CA(California) -> Netherlands -> CA | 150ms |

conclusions:

- memory is fast but the disk is slow
- avoid disk seeks
- simple compressions algorithms are fast
- compress data before sending it over the internet
- data centers are usually in different regions, and it takes time to send data between them

# Availability numbers #card
<!-- 1700179740143 126d90427b81244645b62d8de5e45cc5 -->

Service level agreement (SLA) is commonly used term for service providers, this agreement formally defines the level of uptime your service will deliver.

| Availability %    | Downtime per day    | Downtime per week    | Downtime per month    | Downtime per year    |
|---------------- | --------------- | --------------- | --------------- | --------------- |
| 99%    | 14.40 minutes    | 1.68 hours    | 7.31 hours    | 3.65 days   |
| 99.99%    |  8.64 seconds    | 1.01 minutes | 4.38 minutes| 52.60 minutes |
| 99.999%    |  864.00 | 6.05 seconds| 26.30 seconds| 5.26 minutes|
| 99.9999%    |  86.40 ms | 604.80| 2.63 seconds| 31.56 seconds|
