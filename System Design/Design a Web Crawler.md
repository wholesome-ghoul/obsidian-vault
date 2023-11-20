---
tags: system-design
deck: system-design
---

# Web crawler usage examples #card
<!-- 1700473867822 a864fbed34ca87a4d6d66e69c74cdd18 -->

- search engine indexing
- web archiving
- web mining
- web monitoring (monitor copyright and trademark infringements/violations)

# Possible questions about designing web crawler #card
<!-- 1700481908686 0572413ecb16d4d51396d5706a33aae9 -->

- what is the main purpose of the crawler? Search engine indexing, data mining or something else?
- How many pages does the crawler collect per month?
- What content types are included? HTML only or other content types such as PDFs and images as well?
- Shall we consider newly added or edited web pages?
- Do we need to store HTML pages crawled from the web? If yes, for how long?
- How do we handle web pages with duplicate content?

# What are the characteristics of a good web crawler? #card
<!-- 1700481908737 bee7af327ca45e0539455b5001f9a263 -->

- Scalability: There are billions of web pages. Web crawler should be extremely efficient using parallelization.
- Robustness: The web is full of traps, such as bad html, unresponsive servers, crashes, malicious links, etc. Crawler must handle all those edge cases.
- Politeness: Crawler should not make too many requests to a website within a short time interval.
- Extensibility: Like all the large-scale system, crawler should be flexible so that it requires minimal changes to support new content types.

# Estimations example for web crawler #card
<!-- 1700481908779 034e6315dc4b1355e7a3fc4756ca7ba1 -->

Assume 1 billion web pages are downloaded every month.

- QPS: 1_000_000_000 / 30 days / 24 hours / 3600 seconds = ~400 pages per second
- Peak QPS: 2 * QPS = 800
- Assume average web page is 500k
- 1 billion pages * 500k = 500 TB storage per month
- Assume data is stored for five years, 500 TB * 12 months * 5 years = 30 PB

# High level design for a web crawler #card
<!-- 1700481908799 3ff82477d556e75c575838ffef550769 -->


![](figure-9-2-U6DZBW26.svg)

## Seed URLs #include

Crawler uses seed URLs as a starting point. So choosing them is crucial. Good seed URLs helps crawler to traverse as many links as possible. General strategy is to divide the entire URL space into smaller ones.

The first approach is based on locality. Another way is to choose seed URLs based on topic: shopping, healthcare, shopping, etc.

## URL Frontier #include #card
<!-- 1700486553305 27f8b58e5dbc0c03b5ecf5c252eb8be9 -->

Crawl state can be divided into two: to be downloaded and already downloaded. The COMPONENT that STORES URLs to be DOWNLOADED is called the URL Frontier.

## HTML Downloader #include

Downloads page from the internet. Those urls are provided by URL Frontier.

## DNS Resolver #include

HTML Downloader calls the DNS Resolver to get the corresponding IP address for the URL.

## Content Parser #include

After the page is downloaded, it must be parsed and validated. Implementing a content parser will slow down the crawling process, thus, the content parser is a separate component.

## Content Seen? #include

Research shows that 29% of the web pages on the internet are duplicated contents. We introduce "Content Seen?" data structure to eliminate data redundancy and shorten processing time. An efficient way to compare two contents is to compare the hash values of the two web pages.

## Content Storage #include

Storage system to store HTML content. The choice of the storage depends on factors such as data type, data size, access frequency, life span, etc. Both disk and memory are used.

- most of the content is stored on disk
- popular content is kept in memory to reduce latency

## Link Extractor #include

Relative paths are converted to absolute URLs by adding prefix.

## URL Filter #include

Excludes certain content types, file extensions, errors links and URLs in "blacklisted" sites.

## URL Seen? #include

"URL Seen?" is a data structure that keeps track of URLs that are visited before or already in URL Frontier. Bloom filter and hash table are common techniques to implement the component.

## URL Storage #include

URL Storage stores already visited URLs.

# Explain Web crawler workflow steps #card
<!-- 1700486255452 f85d200c1d36089b8f8793c5d325d037 -->

<!-- AnkiFront:start -->

![](figure-9-4-OKNDJITV.svg)

<!-- AnkiFront:end -->

<!-- AnkiBack:start -->

- Step 1: Add seed URLs to the URL Frontier
- Step 2: HTML Downloader fetches a list of URLs from the URL Frontier
- Step 3: HTML Downloader gets IP addresses of URLs from the DNS resolver and starts downloading
- Step 4: Content Parser parses HTML pages and checks if pages are malformed
- Step 5: After content is parsed and validated, it is passed to the "Content Seen?" component
- Step 6: "Content Seen?" component checks if HTML page is already in the storage
- Step 7: Link extractor extracts links from HTML pages
- Step 8: Extracted links are passed to the URL filter
- Step 9: After links are filtered, they are passed to the "URL Seen?" component
- Step 10: If a URL has not been processed before, it is added to the URL Frontier

<!-- AnkiBack:end -->

# Explain URL Frontier in web crawler system design #card
<!-- 1700490463776 325ed4cf705bc75d218c6b4ef60a8069 -->

The URL Frontier ensures politeness, URL prioritization, and freshness.

## Politeness #include

Sending too many requests is considered "impolite" or even treated as DoS attack.

Design that manages politeness:

![](figure-9-6-4NQUTTWR.svg)

## Priority #include

Can be measured by PageRank, website traffic, update frequency, etc.


![](figure-9-7-H7KUNVWF.svg)

Combined with politeness manager:

![](figure-9-8-Y4EGITZY.svg)

## Freshness #include

Crawler mush periodically recrawl downloaded pages to keep our data set fresh. However, recrawling all the URLs is time and resource consuming. We could though:

- recrawl based on the web pages' update history
- prioritize URLs and recrawl important pages first and more frequently

## Storage for URL Frontier #include

Storing everything on disk would be a bottleneck for the crawler. Instead, majority of URLs are stored on disk, and to reduce the cost of read/write operations on the disk, we maintain buffers in memory for enqueue/dequeue operations.
