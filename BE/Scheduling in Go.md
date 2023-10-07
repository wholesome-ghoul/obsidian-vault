https://www.ardanlabs.com/blog/2018/08/scheduling-in-go-part1.html

- The design and behavior of Go scheduler allows your multithreaded Go programs to be more efficient and performant.
- general understanding of how both OS and Go schedulers work to design your multithreaded software correctly

## OS Scheduler

Takes into account hardware.

Program is just a series of machine instructions that need to be executed one after the other sequentially.

- every program you run creates a Process and each Process is given an initial Thread
- Threads have the ability to create more Threads
- threads run independently of each other
- scheduling is made on thread level not at process level
- threads can run concurrently (each taking a turn on an individual core), or in parallel (each running at the same time on different cores)
- threads also maintain their state to allow for the safe, local, and independent execution of their instructions

- OS scheduler is responsible for making sure cores are not idle when there are threads that can be executed.
- it also creates illusion that all threads that can execute, are executing at the same time
- in the process of creating this illusion, the scheduler needs to run threads with higher priority over the lower priority ones.
- the scheduler also needs to minimize scheduling latencies

### Executing Instructions

Program Counter (PC). Instruction Pointer (IP). Helps thread to keep track of the next instruction to execute.

### Thread States

- Waiting (disk, network, os, sync calls like atomic and mutexes); root cause for bad performance
- Runnable; can also be a cause of bad performance
- Executing

### Types of Work

- CPU-Bound; this work never creates a situation where Thread maybe be placed in a Waiting state
- IO-Bound; this is the work that causes Threads to enter into Waiting state.

### Context Switching

- physical act of swapping Threads on a core is called a context switch
- context switch happens when scheduler pulls an Executing thread off a core and replaces it with a Runnable Thread.
- is expensive, because it takes time to swap Threads on and off a core

### Less is More

### Find Balance

### Cache Lines

### False Sharing

### Scheduling Decision Scenario

## Go Scheduler

### Your Program Starts

Logical Processor (P).
