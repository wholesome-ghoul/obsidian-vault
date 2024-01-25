---
tags: go
deck: go
---

# What is a goroutine? #card
<!-- 1701350114095 cac17040a234fb0e27fddf44e3d912cf -->

Goroutine is a lightweight thread managed by go runtime.

`go f(x,y,z)`

starts a new goroutine running

`f(x,y,z)`

The evaluation of `f`, `x`, `y`, and `z` happens in the current goroutine and the execution of `f` happens in the new one.

# What are channels in go? #card
<!-- 1701350503517 284b6b0189e4b9fd3c5b673530adc339 -->

Channels are typed conduit through which you can send and receive values with the channel operator: <-. Great for communication among goroutines.

```bash
ch <- v # send v to channel ch
v := <- ch # receive from ch, and assign value to v
```

By default, sends and receives block until the other site is ready. This allows goroutines to synchronize without explicit locks or condition variables.

# Explain usage of Range and Close in channels #card
<!-- 1701351393417 2a9f8b68badc14bae0a420b2784f5cc8 -->

Sender can `close` a channel to indicate that no more values will be sent. Receivers can test whether a channel has been closed by assigning a second parameter to the receive expression:

`v, ok := <- ch`

`ok` is `false` if there are no more values to receive and the channel is closed.

The loop `for i := range ch` receives values from the channel repeatedly until it's closed.

Note: only sender should close the channel, never the receiver. Sending on a closed channel will cause a panic. You don't have to close channels, it is only necessary to do so when the receiver must be told there are no more values coming, such as to terminate a `range` loop.
