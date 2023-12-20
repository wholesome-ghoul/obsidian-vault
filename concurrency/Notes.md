---
tags: concurrency
deck: concurrency
---

# Why semaphores? #card
<!-- 1703072076386 7731c466897ae1915e53982d530c1112 -->

- semaphores impose deliberate constraints that help programmers avoid errors.
- solutions using semaphores are often clean and organized, making it easy to demonstrate their correctness.
- semaphores can be implemented efficiently on many systems so solutions that use them are portable and usually efficient.

# Basic sync patterns: Rendezvous #card
<!-- 1703072676499 91002e53ac8fa9c14f0624678e577594 -->

<!-- AnkiFront:start -->

a1 < b2; b1 < a2

```
# Thread A
statement a1
statement a2
```

```
# Thread B
statement b1
statement b2
```

<!-- AnkiFront:end -->
<!-- AnkiBack:start -->

```
# Thread A
statemeent a1
aArrived.signal()
bArrived.wait()
statement a2
```

```
# Thread B
statemeent B1
bArrived.signal()
aArrived.wait()
statement b2
```

<!-- AnkiBack:end -->

# Basic sync patterns: Barrier #card
<!-- 1703073057274 651be547346abfd6518ebdf72182b9ad -->

<!-- AnkiFront:start -->

Every thread should run following:

```
rendezvous
critical point
```

No thread executes critical point until after all threads have executed rendezvous.

<!-- AnkiFront:end -->
<!-- AnkiBack:start -->

```
n = number of threads
count = 0 # how many threads have arrived
mutex = Semaphore(1) # provides access to count
barrier = Semaphore(1) # locked (zero or negative) until all threads arrive; unlocked 1 or more

rendezvous

mutex.wait()
  count++
mutex.signal()

if count == n: barrier.signal() # should be protected by mutex but it's ok now

barrier.wait()
barrier.signal()

critical point
```

<!-- AnkiBack:end -->

# Basic sync patterns: Reusable Barrier #card
<!-- 1703073480223 ec39059bb535557e1fec40c726914d89 -->

<!-- AnkiFront:start -->

Barrier solution so that after all threads have passed through, the turnstile is locked again. (series of steps in loop)

<!-- AnkiFront:end -->
<!-- AnkiBack:start -->

```
turnstile = Semaphore(0)
turnstile2 = Semaphore(1)
mutext = Semaphore(1)
```

When all threads arrive at the first, we lock the second and unlock the first. When all threads arrive at the second, we relock the first, which makes it safe for the threads to loop around to the beginning and then open the second.

```
rendezvous

mutex.wait()
  count++
  if count == n:
    turnstile2.wait()  # lock the second
    turnstile.signal() # unlock the first
mutex.signal()

turnstile.wait() # first turnstile
turnstile.signal()

critical point

mutex.wait()
  count--
  if count == 0:
    turnstile.wait() # lock the first
    turnstile2.wait() # unlock the second
mutex.signal()

turnstile2.wait() # second turnstile
turnstile2.signal()
```

<!-- AnkiBack:end -->
