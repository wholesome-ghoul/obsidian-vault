---
tags: system-design chat-system
deck: system-design
---

# Possible questions to understand the problem and establish design chat system #card
<!-- 1700366656716 ae1ebbebaa6125ab5ba5cdc0a3f580f6 -->

- What kind of chat shall we design? 1 on 1 or group based?
- Is this a mobile app? Or a web app? Or both?
- What is the scale of this app? A startup app or massive scale?
- For group chat, what is the group member limit?
- What features are important for the chat app? Can it support attachment?
- Is there a message size limit?
- Is end-to-end encryption required?
- How long shall we store the chat history?

# Drawbacks of long polling #card
<!-- 1700367122251 879b07843e0789d954fdb919cf5f65f2 -->

- sender and receiver may not connect to the same chat server.
- server has no good way to tell if a client is disconnected.
- it is inefficient. If a user does not chat much, long polling still makes the periodic connections after timeouts.

![](figure-12-4-6KL7KY4X.svg)

# What is a websocket? #card
<!-- 1700367389100 46241530ab5e4eb00fee64dd65b2b478 -->

- The most common solution for sending async updates from server to client
- It is bi-directional and persistent.
- Starts life as a HTTP connection and could be "upgraded" via some well-defined handshake to a Websocket connection.
- Websocket connection is initiated by the client.
- Works even if a firewall is in place, because it uses port 80 or 443

![](figure-12-5-VPDI2T2E.svg)

# High level design for chat system #card
<!-- 1700367475537 49366012085fa0f6abb9da166bb8681d -->

![](figure-12-7-YA7UWFS6.webp)

# Show example of single server high-level design for chat system (as a starting point) #card
<!-- 1700368815899 4ff8a98fbc3378f3624bf4467ed58836 -->

![](figure-12-8-3R5ORNVB.webp)

# How to choose storage for chat system? #card
<!-- 1700368815924 2255672569622960828748048b911cea -->

Choice is between relational and nosql databases.

Two types of data exist in a typical chat system. The first is generic data, such as user profile, setting, user friends list, etc. These data are stored in a robust and reliable relational databases. Replication and sharding are common techniques to satisfy availability and scalability requirements.

The second is unique to chat systems - chat history data. It is recommended to use key-value stores in this case, for the following reasons:

- Key-value stores allow easy horizontal scaling
- Key-value stores provide very low latency to access data
- Relational databases do not handle long tail of data well. When the indices grow large, random access it expensive
- Key-value stores are adopted by other proven reliable chat applications. For example, both Facebook messenger and Discord use key-value stores

# What should the primary key be in chat system data model? #card
<!-- 1700368815958 b0fee854abccbd98462ab7de93d1318b -->

For 1 on chat primary key is `message_id`, while for group chat - the composite (`channel_id`, `message_id`)

![](figure-12-9-356WMC2A.webp)

![](figure-12-10-2TIQVS3D.webp)

Generating `message_id` is an interesting topic. It should satisfy two requirements:

- IDs must be unique
- IDs should be sortable by time, meaning new rows have higher IDs than old ones

- use global 64-bit sequence number generator like Snowflake
- use local sequence number generator. Local means IDs are only unique within a group. The reason why this works is that maintaining message sequence within one-on-on channel or group channel is sufficient

# What is service discovery? #card
<!-- 1700370139492 3235ba422c6dee5d9f464aeba55ecaa7 -->

Primary role of service discovery is to recommend the best chat server for a client based on the criteria like geographical location, server capacity, etc.

![](figure-12-11-FGGY42QW.webp)

# 1 on 1 chat flow example #card
<!-- 1700370139538 f88e24ff8d033ad2e404191476579460 -->

![](figure-12-12-5BTRQZRL.webp)

Keep in mind, there is a persistent WebSocket connection between User B and Chat server 2

# How to sync messages across multiple devices? #card
<!-- 1700370139581 a20cfdbbcf432441269aa3833d536b9f -->


![](figure-12-13-YB54XAJ3.webp)

Each device maintains a variable called `cur_max_message_id`, which keeps track of the latest message ID on the device. Messages that satisfy the following two conditions are considered as new messages:

- the recipient ID is equal to the currently logged-in user ID.
- message ID in the KV store is larger than `cur_max_message_id`.

# What is online presence in chat system? #card
<!-- 1700370139635 d963b020b0dd767fa73c9e4bf80db0fa -->

Indicator whether user is online or not. There are three cases to consider: login, logout and user disconnection. First two are trivial, let's discuss the 3rd.

When user disconnects from the internet, the persistent connection between client and server is lost. A naive way to handle user disconnection is to mark the user offline and change the status to online when the connection re-establishes. However, this approach has a major flow. It is common for user to disconnect and reconnect to the internet frequently in a short time. For example, when user goes through tunnel. Updating online status on every disconnect/reconnect would make the presence indicator change too often, resulting in a bad UX.

So, we introduce a heartbeat mechanism.

![](figure-12-18-CTDI4PAJ.svg)

# How others know when user's online status changes in chat system? #card
<!-- 1700370139685 974ba12fce72c7bb5e4a1a69973499c8 -->

Presence servers use a publish-subscribe model, in which each friend pair maintains a channel.

![](figure-12-19-KY63S3WN.webp)

The above design is effective for a small user group. For larger groups, informing all members about online status is expensive and time consuming. Assume a group has 100,000 members. Each status change will generate 100,000 events. To solve the performance bottleneck, a possible solution is to fetch online status only when a user enters a group or manually refreshes the friend list.
