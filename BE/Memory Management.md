Software on OS accesses RAM:
- loads bytecode
- stores data values and data structures
- loads run-time systems

## Stack

static memory allocation

- LIFO
- Finite & static (data size is known at compile time)
- Multi-threaded apps can have stack per thread
- Managed by OS
- Local variables, function frames and pointers are saved here

## Heap

dynamic memory allocation

- Slower than stack
- Dynamic size
- Shared among threads
- Global variables, and reference types (objects, strings, etc) are saved here

## Garbage Collection

- Mark & Sweep = Tracing GC
- Reference Countrig GC
- RAII (Resource Acquisition is Initialization)
- Automatic Reference Countring (ARC)
- Ownership